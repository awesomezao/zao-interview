

## 1.介绍一下javascript里的this

ECMAScript规范中这样写：

> this 关键字执行为当前执行环境的 ThisBinding。

MDN上这样写：

> In most cases, the value of this is determined by how a function is called.
> 在绝大多数情况下，函数的调用方式决定了this的值。

可以这样理解，在JavaScript中，`this`的指向是调用时决定的，而不是创建时决定的，这就会导致`this`的指向会让人迷惑，简单来说，`this`具有运行期绑定的特性。

> 以下四个规则来判断this的绑定对象：
>
> 1. 由`new`调用：绑定到新创建的对象
> 2. 由`call`或`apply`、`bind`调用：绑定到指定的对象
> 3. 由上下文对象调用：绑定到上下文对象
> 4. 默认：全局对象
>
> 注意：箭头函数不使用上面的绑定规则，根据外层作用域来决定`this`，继承外层函数调用的`this`绑定。

## 2.this指向

 this的指向不是由定义this决定的， 而是随脚本解析自动赋值的。

**1. 全局环境作用域:** this在全局环境始终指向window


 变量形式

```javascript
console.log(this === window) // true
console.log(window.alert === this.alert) // true
console.log(this.parseInt("021", 10)) // 21
var fruit = "banana"; // 定义一个全局变量，等同于window.fruit = "banana"
```

**2. 函数环境 作用域：** 函数由谁调用， this就指向谁

 **2.1 非严格模式**

```javascript
function fn() {
    console.log(this); //window
}
fn() === window; // true；window.fn（),此处默认省略window
```

 **2.2 严格模式**

  a 全局环境下， this指向window

```javascript
"use strict";
this.b = "MDN";
console.log(this == window) // "MDN"
console.log(b) // "MDN"
```

  b 函数环境下， this为undefined

```javascript
function fn() {
    "use strict"; // 这里是严格模式
    console.log(this); //window
}
fn() === undefined //true
```

**3 对象中的方法函数调用:** 指向 该方法所属的对象
 隐式调用

```javascript
var obj = {
    a: 1,
    fn: function () {
        return this;
    }
}
console.log(obj.fn() == obj); //true  函数被obj调用，指向obj
```

 this动态绑定

```javascript
var obj = {
    a: 1,
    fn: function () {
        return this;
    }
}
console.log(obj.fn()); //1  函数被obj调用，指向obj，输出obj的a=1
var a = 2;
var newfun = obj.fn; //此时更改this指向为全局变量newfun
newfun(); //2 ，this访问全局变量a=2
```

**4 在构造函数中:** this始终指向新对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190111123753719.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTU4NjI1,size_16,color_FFFFFF,t_70)

```javascript
function Person(age, name) {
    this.age = age;
    this.name = name
    console.log(this) // 此处 this 分别指向 Person 的实例对象 p1 p2
}
var p1 = new Person(18, 'zs')
var p2 = new Person(18, 'ww')
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190111123455799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NpbmF0XzIzOTU4NjI1,size_16,color_FFFFFF,t_70)
**5 通过事件绑定的方法:** this 指向 绑定事件的对象

```javascript
oBtn.onclick = function () {
        console.log(this); // btn
    }

    <
    button id = "btn" > hh < /button>
```

**6 定时器函数:** 因为是异步操作， this 指向 window

 延时函数内部的回调函数的this指向全局对象window（ 当然我们可以通过bind方法改变其内部函数的this指向）

 我们常见的window属性和方法有alter， document， parseInt， setTimeout， setInterval， location等等， 这些在默认的情况下是省略了window前缀的。（ window.alter = alter）。

 **6.1 普通定时器**

```javascript
setInterval(function () {
    console.log(this); // window
}, 1000);
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190111123505846.png)
 **6.2 定时器嵌套**

```javascript
function Person() {
    this.age = 0;
    setTimeout(function () {
        console.log(this);
    }, 3000);
}

var p = new Person(); //3秒后返回 window 对象
```

 **6.3 可以改变this指向 - 想见三方法**

```javascript
function Person() {
    this.age = 0;
    setTimeout((function () {
        console.log(this);
    }).bind(this), 3000);
}
var p = new Person(); //3秒后返回构造函数新生成的对象 Person{...}
```

**7 自执行函数(匿名函数):** 指向全局变量window

```javascript
(function inner() {
    console.log(this); //this ==> window
})();
```

**8 箭头函数**

要点：

```javascript
a. 箭头函数的this是在定义函数时绑定的， 不是在执行过程中绑定的
b. 箭头函数中的this始终指向父级对象
c. 所有 call() / apply() / bind() 方法对于箭头函数来说只是传入参数， 对它的 this 毫无影响。
var obj = {
    a: 1,
    fn: () => {
        //箭头函数中的this始终指向定义时的环境
        //箭头函数中的this始终指向父级对象
        console.log(this); //对象内的this调用时一般指向obj，而箭头函数在创建时就指向了obj的父级对象window
    }
}
obj.fn(); //window
```

## 3.如何改变this指向

每个Function构造函数的原型prototype，都有方法call()，apply()，bind()

>+ call()，apply()
>
>  在特定作用域调用函数
>
>+ bind()
>
>  会创建一个函数的实例，this会被绑定到bind()函数

## 3.如何改变this指向

## 4.call和apply的区别

 apply() 与call（） 非常相似， 不同之处在于提供参数的方式， apply（） 使用参数数组， 而不是参数列表 

## 5.如何实现call和apply

### call

```javascript
// call
Function.prototype.mycall = function (context, ...args) {
  context = context ? Object(context) :window
  const fn = Symbol('fn')
  context[fn] = this
  context[fn](...args)
  delete context.fn
  // Reflect.deleteProperty(context,fn)
}

function test(a, b) {
  console.log(this.value)
  console.log(a + b)
}

let obj = {
  value: 'obj'
}

var value = 'window'

test.mycall(obj, 1, 2) // 'obj' 3
test.mycall(window, 1, 2) // 'window' 3
test.mycall(undefined, 1, 2) // 'window' 3
test.mycall(null, 1, 2) // 'window' 3
test.mycall(123, 1, 2) // undefined 3
```

### apply

```javascript
// apply
Function.prototype.myapply = function (context, args) {
  if (!Array.isArray(args)) {
    throw new TypeError('args must be array')
  }
  context = context ? Object(context) : window
  const fn = Symbol('fn')
  context[fn] = this
  context[fn](...args)
  delete context.fn
  // Reflect.deleteProperty(context,fn)
}

let obj = {
  value: 'obj'
}

var value = 'window'

test.myapply(obj, [1, 2]) // 'obj' 3
test.myapply(window, [1, 2]) // 'window' 3
test.myapply(undefined, [1, 2]) // 'window' 3
test.myapply(null, [1, 2]) // 'window' 3
test.myapply(123, [1, 2]) // undefined 3
```



## 6.如何实现bind

```javascript
// bind
Function.prototype.mybind = function (context, ...args1) {
  let _this = this // 缓存调用mybind的函数
  let temp = function () {} // 用于继承和判断绑定后的函数是否被new调用
  let boundFunc = function (...args2) {
    // 判断方式即判断当前this(调用者)是否是new之后返回的对象
    return _this.call(this instanceof temp ? this : context, ...args1, ...args2)
  }
  // 改变temp prototype用于判断
  temp.prototype = this.prototype
  // 继承
  boundFunc.prototype = new temp()
  return boundFunc
}

let obj = {
  name:'zao'
}
function test(age,sex) {
  this.age=age
  this.sex = sex
  console.log(age,sex,this.name)
}
test.prototype.hoppy = 'play'

let zao1 = test.mybind(obj, '12', '男') 
let zaoFunc = zao1() // 12 男 zao
let zaoInstance = new zao1() // 12 男 undefined
```





[前端js中this指向及改变this指向的方法]( https://www.cnblogs.com/hjson/archive/2019/01/11/10254555.html )

[彻底弄清 this call apply bind 以及原生实现]( https://juejin.im/post/5c813aa5f265da2dd94cd7c2 )