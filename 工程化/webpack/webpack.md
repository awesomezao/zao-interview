# webpack

>  本质上，**webpack** 是一个现代 JavaScript 应用程序的*静态模块打包工具*。当 webpack 处理应用程序时，它会在内部构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，此依赖图会映射项目所需的每个模块，并生成一个或多个 *bundle*。 

[深入浅出webpack]( https://webpack.wuhaolin.cn/ )

[hmr原理]( https://zhuanlan.zhihu.com/p/30669007 )

---

# webpack配置



# webpack原理



![](https://gitee.com/ruoyiwen/img/raw/master/blog/16706c9d07bd3b67)

### Entry

入口起点(entry point)指示 webpack 应该使用哪个模块,来作为构建其内部依赖图的开始。

进入入口起点后,webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

每个依赖项随即被处理,最后输出到称之为 bundles 的文件中。

### Output

output 属性告诉 webpack 在哪里输出它所创建的 bundles,以及如何命名这些文件,默认值为 ./dist。

基本上,整个应用程序结构,都会被编译到你指定的输出路径的文件夹中。

### Module

模块,在 Webpack 里一切皆模块,一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。

### Chunk

代码块,一个 Chunk 由多个模块组合而成,用于代码合并与分割。

### Loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。

loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块,然后你就可以利用 webpack 的打包能力,对它们进行处理。

本质上,webpack loader 将所有类型的文件,转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。

### Plugin

loader 被用于转换某些类型的模块,而插件则可以用于执行范围更广的任务。

插件的范围包括,从打包优化和压缩,一直到重新定义环境中的变量。插件接口功能极其强大,可以用来处理各种各样的任务。

## webpack 构建流程

Webpack 的运行流程是一个串行的过程,从启动到结束会依次执行以下流程 :

1. 初始化参数：从配置文件和 Shell 语句中读取与合并参数,得出最终的参数。
2. 开始编译：用上一步得到的参数初始化 Compiler 对象,加载所有配置的插件,执行对象的 run 方法开始执行编译。
3. 确定入口：根据配置中的 entry 找出所有的入口文件。
4. 编译模块：从入口文件出发,调用所有配置的 Loader 对模块进行翻译,再找出该模块依赖的模块,再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理。
5. 完成模块编译：在经过第 4 步使用 Loader 翻译完所有模块后,得到了每个模块被翻译后的最终内容以及它们之间的依赖关系。
6. 输出资源：根据入口和模块之间的依赖关系,组装成一个个包含多个模块的 Chunk,再把每个 Chunk 转换成一个单独的文件加入到输出列表,这步是可以修改输出内容的最后机会。
7. 输出完成：在确定好输出内容后,根据配置确定输出的路径和文件名,把文件内容写入到文件系统。

在以上过程中,Webpack 会在特定的时间点广播出特定的事件,插件在监听到感兴趣的事件后会执行特定的逻辑,并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果。



### 1. 定义 Compiler 类

```javascript
class Compiler {
  constructor(options) {
    // webpack 配置
    const { entry, output } = options
    // 入口
    this.entry = entry
    // 出口
    this.output = output
    // 模块
    this.modules = []
  }
  // 构建启动
  run() {}
  // 重写 require函数,输出bundle
  generate() {}
}
```



我们这里使用@babel/parser,这是 babel7 的工具,来帮助我们分析内部的语法,包括 es6,返回一个 AST 抽象语法树。

```javascript
// webpack.config.js

const path = require('path')
module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'main.js'
  }
}
//
const fs = require('fs')
const parser = require('@babel/parser')
const options = require('./webpack.config')

const Parser = {
  getAst: path => {
    // 读取入口文件
    const content = fs.readFileSync(path, 'utf-8')
    // 将文件内容转为AST抽象语法树
    return parser.parse(content, {
      sourceType: 'module'
    })
  }
}

class Compiler {
  constructor(options) {
    // webpack 配置
    const { entry, output } = options
    // 入口
    this.entry = entry
    // 出口
    this.output = output
    // 模块
    this.modules = []
  }
  // 构建启动
  run() {
    const ast = Parser.getAst(this.entry)
  }
  // 重写 require函数,输出bundle
  generate() {}
}

new Compiler(options).run()
```



Babel 提供了@babel/traverse(遍历)方法维护这 AST 树的整体状态,我们这里使用它来帮我们找出依赖模块。

```javascript
const fs = require('fs')
const path = require('path')
const options = require('./webpack.config')
const parser = require('@babel/parser')
const traverse = require('@babel/traverse').default

const Parser = {
  getAst: path => {
    // 读取入口文件
    const content = fs.readFileSync(path, 'utf-8')
    // 将文件内容转为AST抽象语法树
    return parser.parse(content, {
      sourceType: 'module'
    })
  },
  getDependecies: (ast, filename) => {
    const dependecies = {}
    // 遍历所有的 import 模块,存入dependecies
    traverse(ast, {
      // 类型为 ImportDeclaration 的 AST 节点 (即为import 语句)
      ImportDeclaration({ node }) {
        const dirname = path.dirname(filename)
        // 保存依赖模块路径,之后生成依赖关系图需要用到
        const filepath = './' + path.join(dirname, node.source.value)
        dependecies[node.source.value] = filepath
      }
    })
    return dependecies
  }
}

class Compiler {
  constructor(options) {
    // webpack 配置
    const { entry, output } = options
    // 入口
    this.entry = entry
    // 出口
    this.output = output
    // 模块
    this.modules = []
  }
  // 构建启动
  run() {
    const { getAst, getDependecies } = Parser
    const ast = getAst(this.entry)
    const dependecies = getDependecies(ast, this.entry)
  }
  // 重写 require函数,输出bundle
  generate() {}
}

new Compiler(options).run()
```



将 AST 语法树转换为浏览器可执行代码,我们这里使用@babel/core 和 @babel/preset-env。

```javascript
const fs = require('fs')
const path = require('path')
const options = require('./webpack.config')
const parser = require('@babel/parser')
const traverse = require('@babel/traverse').default
const { transformFromAst } = require('@babel/core')

const Parser = {
  getAst: path => {
    // 读取入口文件
    const content = fs.readFileSync(path, 'utf-8')
    // 将文件内容转为AST抽象语法树
    return parser.parse(content, {
      sourceType: 'module'
    })
  },
  getDependecies: (ast, filename) => {
    const dependecies = {}
    // 遍历所有的 import 模块,存入dependecies
    traverse(ast, {
      // 类型为 ImportDeclaration 的 AST 节点 (即为import 语句)
      ImportDeclaration({ node }) {
        const dirname = path.dirname(filename)
        // 保存依赖模块路径,之后生成依赖关系图需要用到
        const filepath = './' + path.join(dirname, node.source.value)
        dependecies[node.source.value] = filepath
      }
    })
    return dependecies
  },
  getCode: ast => {
    // AST转换为code
    const { code } = transformFromAst(ast, null, {
      presets: ['@babel/preset-env']
    })
    return code
  }
}

class Compiler {
  constructor(options) {
    // webpack 配置
    const { entry, output } = options
    // 入口
    this.entry = entry
    // 出口
    this.output = output
    // 模块
    this.modules = []
  }
  // 构建启动
  run() {
    const { getAst, getDependecies, getCode } = Parser
    const ast = getAst(this.entry)
    const dependecies = getDependecies(ast, this.entry)
    const code = getCode(ast)
  }
  // 重写 require函数,输出bundle
  generate() {}
}

new Compiler(options).run()
```



```
const fs = require('fs')
const path = require('path')
const options = require('./webpack.config')
const parser = require('@babel/parser')
const traverse = require('@babel/traverse').default
const { transformFromAst } = require('@babel/core')

const Parser = {
  getAst: path => {
    // 读取入口文件
    const content = fs.readFileSync(path, 'utf-8')
    // 将文件内容转为AST抽象语法树
    return parser.parse(content, {
      sourceType: 'module'
    })
  },
  getDependecies: (ast, filename) => {
    const dependecies = {}
    // 遍历所有的 import 模块,存入dependecies
    traverse(ast, {
      // 类型为 ImportDeclaration 的 AST 节点 (即为import 语句)
      ImportDeclaration({ node }) {
        const dirname = path.dirname(filename)
        // 保存依赖模块路径,之后生成依赖关系图需要用到
        const filepath = './' + path.join(dirname, node.source.value)
        dependecies[node.source.value] = filepath
      }
    })
    return dependecies
  },
  getCode: ast => {
    // AST转换为code
    const { code } = transformFromAst(ast, null, {
      presets: ['@babel/preset-env']
    })
    return code
  }
}

class Compiler {
  constructor(options) {
    // webpack 配置
    const { entry, output } = options
    // 入口
    this.entry = entry
    // 出口
    this.output = output
    // 模块
    this.modules = []
  }
  // 构建启动
  run() {
    // 解析入口文件
    const info = this.build(this.entry)
    this.modules.push(info)
    this.modules.forEach(({ dependecies }) => {
      // 判断有依赖对象,递归解析所有依赖项
      if (dependecies) {
        for (const dependency in dependecies) {
          this.modules.push(this.build(dependecies[dependency]))
        }
      }
    })
    // 生成依赖关系图
    const dependencyGraph = this.modules.reduce(
      (graph, item) => ({
        ...graph,
        // 使用文件路径作为每个模块的唯一标识符,保存对应模块的依赖对象和文件内容
        [item.filename]: {
          dependecies: item.dependecies,
          code: item.code
        }
      }),
      {}
    )
  }
  build(filename) {
    const { getAst, getDependecies, getCode } = Parser
    const ast = getAst(filename)
    const dependecies = getDependecies(ast, filename)
    const code = getCode(ast)
    return {
      // 文件路径,可以作为每个模块的唯一标识符
      filename,
      // 依赖对象,保存着依赖模块路径
      dependecies,
      // 文件内容
      code
    }
  }
  // 重写 require函数,输出bundle
  generate() {}
}

new Compiler(options).run()
```



```javascript
const fs = require('fs')
const path = require('path')
const options = require('./webpack.config')
const parser = require('@babel/parser')
const traverse = require('@babel/traverse').default
const { transformFromAst } = require('@babel/core')

const Parser = {
  getAst: path => {
    // 读取入口文件
    const content = fs.readFileSync(path, 'utf-8')
    // 将文件内容转为AST抽象语法树
    return parser.parse(content, {
      sourceType: 'module'
    })
  },
  getDependecies: (ast, filename) => {
    const dependecies = {}
    // 遍历所有的 import 模块,存入dependecies
    traverse(ast, {
      // 类型为 ImportDeclaration 的 AST 节点 (即为import 语句)
      ImportDeclaration({ node }) {
        const dirname = path.dirname(filename)
        // 保存依赖模块路径,之后生成依赖关系图需要用到
        const filepath = './' + path.join(dirname, node.source.value)
        dependecies[node.source.value] = filepath
      }
    })
    return dependecies
  },
  getCode: ast => {
    // AST转换为code
    const { code } = transformFromAst(ast, null, {
      presets: ['@babel/preset-env']
    })
    return code
  }
}

class Compiler {
  constructor(options) {
    // webpack 配置
    const { entry, output } = options
    // 入口
    this.entry = entry
    // 出口
    this.output = output
    // 模块
    this.modules = []
  }
  // 构建启动
  run() {
    // 解析入口文件
    const info = this.build(this.entry)
    this.modules.push(info)
    this.modules.forEach(({ dependecies }) => {
      // 判断有依赖对象,递归解析所有依赖项
      if (dependecies) {
        for (const dependency in dependecies) {
          this.modules.push(this.build(dependecies[dependency]))
        }
      }
    })
    // 生成依赖关系图
    const dependencyGraph = this.modules.reduce(
      (graph, item) => ({
        ...graph,
        // 使用文件路径作为每个模块的唯一标识符,保存对应模块的依赖对象和文件内容
        [item.filename]: {
          dependecies: item.dependecies,
          code: item.code
        }
      }),
      {}
    )
    this.generate(dependencyGraph)
  }
  build(filename) {
    const { getAst, getDependecies, getCode } = Parser
    const ast = getAst(filename)
    const dependecies = getDependecies(ast, filename)
    const code = getCode(ast)
    return {
      // 文件路径,可以作为每个模块的唯一标识符
      filename,
      // 依赖对象,保存着依赖模块路径
      dependecies,
      // 文件内容
      code
    }
  }
  // 重写 require函数 (浏览器不能识别commonjs语法),输出bundle
  generate(code) {
    // 输出文件路径
    const filePath = path.join(this.output.path, this.output.filename)
    // 懵逼了吗? 没事,下一节我们捋一捋
    const bundle = `(function(graph){
      function require(module){
        function localRequire(relativePath){
          return require(graph[module].dependecies[relativePath])
        }
        var exports = {};
        (function(require,exports,code){
          eval(code)
        })(localRequire,exports,graph[module].code);
        return exports;
      }
      require('${this.entry}')
    })(${JSON.stringify(code)})`

    // 把文件内容写入到文件系统
    fs.writeFileSync(filePath, bundle, 'utf-8')
  }
}

new Compiler(options).run()
```



我们通过下面的例子来进行讲解,先死亡凝视 30 秒

```javascript
;(function(graph) {
  function require(moduleId) {
    function localRequire(relativePath) {
      return require(graph[moduleId].dependecies[relativePath])
    }
    var exports = {}
    ;(function(require, exports, code) {
      eval(code)
    })(localRequire, exports, graph[moduleId].code)
    return exports
  }
  require('./src/index.js')
})({
  './src/index.js': {
    dependecies: { './hello.js': './src/hello.js' },
    code: '"use strict";\n\nvar _hello = require("./hello.js");\n\ndocument.write((0, _hello.say)("webpack"));'
  },
  './src/hello.js': {
    dependecies: {},
    code:
      '"use strict";\n\nObject.defineProperty(exports, "__esModule", {\n  value: true\n});\nexports.say = say;\n\nfunction say(name) {\n  return "hello ".concat(name);\n}'
  }
})
```



```javascript
// 定义一个立即执行函数,传入生成的依赖关系图
;(function(graph) {
  // 重写require函数
  function require(moduleId) {
    console.log(moduleId) // ./src/index.js
  }
  // 从入口文件开始执行
  require('./src/index.js')
})({
  './src/index.js': {
    dependecies: { './hello.js': './src/hello.js' },
    code: '"use strict";\n\nvar _hello = require("./hello.js");\n\ndocument.write((0, _hello.say)("webpack"));'
  },
  './src/hello.js': {
    dependecies: {},
    code:
      '"use strict";\n\nObject.defineProperty(exports, "__esModule", {\n  value: true\n});\nexports.say = say;\n\nfunction say(name) {\n  return "hello ".concat(name);\n}'
  }
})
```



```javascript
// 定义一个立即执行函数,传入生成的依赖关系图
;(function(graph) {
  // 重写require函数
  function require(moduleId) {
    ;(function(code) {
      console.log(code) // "use strict";\n\nvar _hello = require("./hello.js");\n\ndocument.write((0, _hello.say)("webpack"));
      eval(code) // Uncaught TypeError: Cannot read property 'code' of undefined
    })(graph[moduleId].code)
  }
  // 从入口文件开始执行
  require('./src/index.js')
})({
  './src/index.js': {
    dependecies: { './hello.js': './src/hello.js' },
    code: '"use strict";\n\nvar _hello = require("./hello.js");\n\ndocument.write((0, _hello.say)("webpack"));'
  },
  './src/hello.js': {
    dependecies: {},
    code:
      '"use strict";\n\nObject.defineProperty(exports, "__esModule", {\n  value: true\n});\nexports.say = say;\n\nfunction say(name) {\n  return "hello ".concat(name);\n}'
  }
})
```

可以看到,我们在执行"./src/index.js"文件代码的时候报错了,这是因为 index.js 里引用依赖 hello.js,而我们没有对依赖进行处理,接下来我们对依赖引用进行处理。



```javascript
// 定义一个立即执行函数,传入生成的依赖关系图
;(function(graph) {
  // 重写require函数
  function require(moduleId) {
    // 找到对应moduleId的依赖对象,调用require函数,eval执行,拿到exports对象
    function localRequire(relativePath) {
      return require(graph[moduleId].dependecies[relativePath]) // {__esModule: true, say: ƒ say(name)}
    }
    // 定义exports对象
    var exports = {}
    ;(function(require, exports, code) {
      // commonjs语法使用module.exports暴露实现,我们传入的exports对象会捕获依赖对象(hello.js)暴露的实现(exports.say = say)并写入
      eval(code)
    })(localRequire, exports, graph[moduleId].code)
    // 暴露exports对象,即暴露依赖对象对应的实现
    return exports
  }
  // 从入口文件开始执行
  require('./src/index.js')
})({
  './src/index.js': {
    dependecies: { './hello.js': './src/hello.js' },
    code: '"use strict";\n\nvar _hello = require("./hello.js");\n\ndocument.write((0, _hello.say)("webpack"));'
  },
  './src/hello.js': {
    dependecies: {},
    code:
      '"use strict";\n\nObject.defineProperty(exports, "__esModule", {\n  value: true\n});\nexports.say = say;\n\nfunction say(name) {\n  return "hello ".concat(name);\n}'
  }
})
```