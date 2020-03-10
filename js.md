1. ['1', '2', '3'].map(parseInt) what & why ?

2. 什么是防抖和节流？有什么区别？如何实现？

3. 介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

4. ES5/ES6 的继承除了写法以外还有什么区别？

5. 有以下 3 个判断数组的方法，请分别介绍它们之间的区别和优劣
   `Object.prototype.toString.call() 、 instanceof 以及 Array.isArray()`

6. 介绍模块化发展历程
   `可从IIFE、AMD、CMD、CommonJS、UMD、webpack(require.ensure)、ES Module、<script type="module"> 这几个角度考虑。`

7. 全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？

8. 下面的代码打印什么内容，为什么？(IIFE具名函数)
   ```javascript
   var b = 10;
   (function b(){
       b = 20;
       console.log(b); 
   })();
   ```

9. 简单改造下面的代码，使之分别打印 10 和 20。（IIFE）
   ```javascript
   var b = 10;
   (function b(){
       b = 20;
       console.log(b); 
   })();
   ```

10. 下面代码输出什么。(IIFE)
    ```javascript
    var a = 10;
    (function () {
        console.log(a)
        a = 5
        console.log(window.a)
        var a = 20;
        console.log(a)
    })()
    ```

11. 使用 sort() 对数组 [3, 15, 8, 29, 102, 22] 进行排序，输出结果。（UTF16，ASCLL）

12. 输出以下代码执行的结果并解释为什么。(伪数组)
    ```javascript
    var obj = {
        '2': 3,
        '3': 4,
        'length': 2,
        'splice': Array.prototype.splice,
        'push': Array.prototype.push
    }
    obj.push(1)
    obj.push(2)
    console.log(obj)
    ```

13. call 和 apply 的区别是什么，哪个性能更好一些。

14. 输出以下代码的执行结果并解释为什么。

    ```js
    var a = {n: 1};
    var b = a;
    a.x = a = {n: 2};
    
    console.log(a.x) 	
    console.log(b.x)
    ```

15. 箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？

16. `a.b.c.d` 和 `a['b']['c']['d']`，哪个性能更高？

17. ES6 代码转成 ES5 代码的实现思路是什么(AST)

18. 为什么普通 for 循环的性能远远高于 forEach 的性能，请解释其中的原因。

19. 数组里面有10万个数据，取第一个元素和第10万个元素的时间相差多少

20. 输出以下代码运行结果

    ```js
    // example 1
    var a={}, b='123', c=123;  
    a[b]='b';
    a[c]='c';  
    console.log(a[b]);
    
    ---------------------
    // example 2
    var a={}, b=Symbol('123'), c=Symbol('123');  
    a[b]='b';
    a[c]='c';  
    console.log(a[b]);
    
    ---------------------
    // example 3
    var a={}, b={key:'123'}, c={key:'456'};  
    a[b]='b';
    a[c]='c';  
    console.log(a[b]);
    ```

21. input 搜索如何防抖，如何处理中文输入

22. var、let 和 const 区别的实现原理是什么

23. 介绍下前端加密的常见场景和方法

24. 写出如下代码的打印结果

    ```js
    function changeObjProperty(o) {
      o.siteUrl = "http://www.baidu.com"
      o = new Object()
      o.siteUrl = "http://www.google.com"
    } 
    let webSite = new Object();
    changeObjProperty(webSite);
    console.log(webSite.siteUrl);
    ```

25. 请写出如下代码的打印结果

     ```js
     function Foo() {
     Foo.a = function() {
     console.log(1)
     }
     this.a = function() {
     console.log(2)
     }
     }
     Foo.prototype.a = function() {
     console.log(3)
     }
     Foo.a = function() {
     console.log(4)
     }
     Foo.a();
     let obj = new Foo();
     obj.a();
     Foo.a();
     ```

26. 分别写出如下代码的返回值

     ```js
     String('11') == new String('11');
     String('11') === new String('11');
     ```

27. 请写出如下代码的打印结果

    > ```js
    > var name = 'Tom';
    > (function() {
    > if (typeof name == 'undefined') {
    >   var name = 'Jack';
    >   console.log('Goodbye ' + name);
    > } else {
    >   console.log('Hello ' + name);
    > }
    > })();
    > ```

28. 扩展题，请写出如下代码的打印结果

    > ```js
    > var name = 'Tom';
    > (function() {
    > if (typeof name == 'undefined') {
    >   name = 'Jack';
    >   console.log('Goodbye ' + name);
    > } else {
    >   console.log('Hello ' + name);
    > }
    > })();
    > ```

29. 输出以下代码运行结果

    > ```js
    > 1 + "1"
    > 
    > 2 * "2"
    > 
    > [1, 2] + [2, 1]
    > 
    > "a" + + "b"
    > ```

30. 为什么 for 循环嵌套顺序会影响性能？

    ```js
    var t1 = new Date().getTime()
    for (let i = 0; i < 100; i++) {
      for (let j = 0; j < 1000; j++) {
        for (let k = 0; k < 10000; k++) {
        }
      }
    }
    var t2 = new Date().getTime()
    console.log('first time', t2 - t1)
    
    for (let i = 0; i < 10000; i++) {
      for (let j = 0; j < 1000; j++) {
        for (let k = 0; k < 100; k++) {
    
        }
      }
    }
    var t3 = new Date().getTime()
    console.log('two time', t3 - t2)
    ```

31. 输出以下代码执行结果

    ```js
    function wait() {
      return new Promise(resolve =>
        setTimeout(resolve, 10 * 1000)
      )
    }
    
    async function main() {
      console.time();
      const x = wait();
      const y = wait();
      const z = wait();
      await x;
      await y;
      await z;
      console.timeEnd();
    }
    main();
    ```

32. 输出以下代码执行结果，大致时间就好（不同于上题）

    ```js
    function wait() {
      return new Promise(resolve =>
        setTimeout(resolve, 10 * 1000)
      )
    }
    
    async function main() {
      console.time();
      await wait();
      await wait();
      await wait();
      console.timeEnd();
    }
    main();
    ```

33. 如何实现骨架屏，说说你的思路

34. setTimeout、Promise、Async/Await 的区别
35. 