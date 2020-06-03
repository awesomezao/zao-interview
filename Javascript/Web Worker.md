[JavaScript 中的多线程 -- Web Worker]( https://zhuanlan.zhihu.com/p/25184390 )

### Web Worker简介

Web Worker 是HTML5标准的一部分，这一规范定义了一套 API，它允许一段JavaScript程序运行在主线程之外的另外一个线程中。
值得注意的是， Web Worker 规范中定义了两类工作线程，分别是专用线程Dedicated Worker和共享线程 Shared Worker，其中，Dedicated Worker只能为一个页面所使用，而Shared Worker则可以被多个页面所共享。

## 快速上手

### 创建worker

只需调用Worker() 构造函数并传入一个要在 worker 线程内运行的脚本的URI，即可创建一个新的worker。

```
var myWorker = new Worker("my_task.js");

// my_task.js中的代码 
var i = 0;
function timedCount(){
    i = i+1;
    postMessage(i);
    setTimeout(timedCount, 1000);
}
timedCount();复制代码
```

另外，通过URL.createObjectURL()创建URL对象，可以实现创建内嵌的worker

```
var myTask = `
    var i = 0;
    function timedCount(){
        i = i+1;
        postMessage(i);
        setTimeout(timedCount, 1000);
    }
    timedCount();
`;

var blob = new Blob([myTask]);
var myWorker = new Worker(window.URL.createObjectURL(blob));复制代码
```

这样，就可以结合NEJ、Webpack进行模块化管理、打包了。

> 注意：传入 Worker 构造函数的参数 URI 必须遵循同源策略。Worker线程的创建的是异步的，主线程代码不会阻塞在这里等待worker线程去加载、执行指定的脚本文件，而是会立即向下继续执行后面代码。
>
> 提示：本文所有的示例代码均可直接拷贝到chrome控制台中运行。

### Worker线程数据通讯方式

Worker 与其主页面之间的通信是通过 onmessage 事件和 postMessage() 方法实现的。

在主页面与 Worker 之间传递的数据是通过拷贝，而不是共享来完成的。传递给 Worker 的对象需要经过序列化，接下来在另一端还需要反序列化。页面与 Worker 不会共享同一个实例，最终的结果就是在每次通信结束时生成了数据的一个副本。

也就是说，Worker 与其主页面之间只能单纯的传递数据，不能传递复杂的引用类型：如通过构造函数创建的对象等。并且，传递的数据也是经过拷贝生成的一个副本，在一端对数据进行修改不会影响另一端。

```
var myTask = `
    onmessage = function (e) {
        var data = e.data;
        data.push('hello');
        console.log('worker:', data); // worker: [1, 2, 3, "hello"]
        postMessage(data);
    };
`;

var blob = new Blob([myTask]);
var myWorker = new Worker(window.URL.createObjectURL(blob));

myWorker.onmessage = function (e) {
    var data = e.data;
    console.log('page:', data); // page: [1, 2, 3, "hello"]
    console.log('arr:', arr); // arr: [1, 2, 3]
};

var arr = [1,2,3];
myWorker.postMessage(arr);复制代码
```

### 通过可转让对象来传递数据

前面介绍了简单数据的传递，其实还有一种性能更高的方法来传递数据，就是通过可转让对象将数据在主页面和Worker之间进行来回穿梭。可转让对象从一个上下文转移到另一个上下文而不会经过任何拷贝操作。这意味着当传递大数据时会获得极大的性能提升。和按照引用传递不同，一旦对象转让，那么它在原来上下文的那个版本将不复存在。该对象的所有权被转让到新的上下文内。例如，当你将一个 ArrayBuffer 对象从主应用转让到 Worker 中，原始的 ArrayBuffer 被清除并且无法使用。它包含的内容会(完整无差的)传递给 Worker 上下文。

```
var uInt8Array = new Uint8Array(1024*1024*32); // 32MB
for (var i = 0; i < uInt8Array .length; ++i) {
  uInt8Array[i] = i;
}

console.log(uInt8Array.length); // 传递前长度:33554432

var myTask = `
    onmessage = function (e) {
        var data = e.data;
        console.log('worker:', data);
    };
`;

var blob = new Blob([myTask]);
var myWorker = new Worker(window.URL.createObjectURL(blob));
myWorker.postMessage(uInt8Array.buffer, [uInt8Array.buffer]);

console.log(uInt8Array.length); // 传递后长度:0复制代码
```

### importScripts()

Worker 线程能够访问一个全局函数imprtScripts()来引入脚本，该函数接受0个或者多个URI作为参数。

浏览器加载并运行每一个列出的脚本，每个脚本中的全局对象都能够被 worker 使用。如果脚本无法加载，将抛出 NETWORK_ERROR 异常，接下来的代码也无法执行。而之前执行的代码(包括使用 window.setTimeout() 异步执行的代码)依然能够运行。importScripts() 之后的函数声明依然会被保留，因为它们始终会在其他代码之前运行。

> 注意：脚本的下载顺序不固定，但执行时会按照传入 importScripts() 中的文件名顺序进行。这个过程是同步完成的；直到所有脚本都下载并运行完毕， importScripts() 才会返回。

### Worker上下文

Worker执行的上下文，与主页面执行时的上下文并不相同，最顶层的对象并不是window，而是个一个叫做WorkerGlobalScope的东东，所以无法访问window、以及与window相关的DOM API，但是可以与setTimeout、setInterval等协作。

WorkerGlobalScope作用域下的常用属性、方法如下：

1、self

我们可以使用 WorkerGlobalScope 的 self 属性来或者这个对象本身的引用

2、location

　　location 属性返回当线程被创建出来的时候与之关联的 WorkerLocation 对象，它表示用于初始化这个工作线程的脚步资源的绝对 URL，即使页面被多次重定向后，这个 URL 资源位置也不会改变。

3、close

　　关闭当前线程

4、importScripts

　　我们可以通过importScripts()方法通过url在worker中加载库函数

5、XMLHttpRequest

　　有了它，才能发出Ajax请求

6、setTimeout/setInterval以及addEventListener/postMessage

### 终止 terminate()

在主页面上调用terminate()方法，可以立即杀死 worker 线程，不会留下任何机会让它完成自己的操作或清理工作。另外，Worker也可以调用自己的 close() 方法来关闭自己

```
    // 主页面调用
    myWorker.terminate();

    // Worker 线程调用
    self.close();复制代码
```

### 处理错误

当 worker 出现运行时错误时，它的 onerror 事件处理函数会被调用。它会收到一个实现了 ErrorEvent 接口名为 error的事件。该事件不会冒泡，并且可以被取消；为了防止触发默认动作，worker 可以调用错误事件的 preventDefault() 方法。

错误事件有三个实用的属性：filename - 发生错误的脚本文件名；lineno - 出现错误的行号；以及 message - 可读性良好的错误消息。

```
var myTask = `
    onmessage = function (e) {
        var data = e.data;
        console.log('worker:', data);
    };

    // 使用未声明的变量
    arr.push('error');
`;

var blob = new Blob([myTask]);
var myWorker = new Worker(window.URL.createObjectURL(blob));
myWorker.onerror = function onError(e) {
    // ERROR: Line 8 in blob:http://www.cnblogs.com/490a7c32-7386-4d6e-a82b-1ca0b1bf2469: Uncaught ReferenceError: arr is not defined
    console.log(['ERROR: Line ', e.lineno, ' in ', e.filename, ': ', e.message].join(''));
}复制代码
```

## 总结

最后总结下Web Worker为javascript带来了什么，以及典型的应用场景。

### 强大的计算能力

可以加载一个JS进行大量的复杂计算而不挂起主进程，并通过postMessage，onmessage进行通信，解决了大量计算对UI渲染的阻塞问题。

### 典型应用场景

1、数学运算

Web Worker最简单的应用就是用来做后台计算，对CPU密集型的场景再适合不过了。

2、图像处理

通过使用从``中获取的数据，可以把图像分割成几个不同的区域并且把它们推送给并行的不同Workers来做计算，对图像进行像素级的处理，再把处理完成的图像数据返回给主页面。

3、大数据的处理

目前mvvm框架越来越普及，基于数据驱动的开发模式也越愈发流行，未来大数据的处理也可能转向到前台，这时，将大数据的处理交给在Web Worker也是上上之策了吧。

