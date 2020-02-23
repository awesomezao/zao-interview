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