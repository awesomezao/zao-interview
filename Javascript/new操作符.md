## new手写版本1

```javascript
function objectFactory() {

    let obj = new Object() //从Object.prototype上克隆一个对象
    const Constructor = [].shift.call(arguments); //取得外部传入的构造器
    const F = function () {};
    F.prototype = Constructor.prototype;
    obj = new F(); //指向正确的原型

    Constructor.apply(obj, arguments); //借用外部传入的构造器给obj设置属性

    return obj; //返回 obj

};

function Person(name) {
    this.name = name
}

let p1 = objectFactory(Person, 'zao')
```

## new手写版本2

```javascript
function objectFactory() {

    var obj = new Object(),//从Object.prototype上克隆一个对象

    Constructor = [].shift.call(arguments);//取得外部传入的构造器

    var F=function(){};
    F.prototype= Constructor.prototype;
    obj=new F();//指向正确的原型

    var ret = Constructor.apply(obj, arguments);//借用外部传入的构造器给obj设置属性

    return typeof ret === 'object' ? ret : obj;//确保构造器总是返回一个对象

};

```

## new手写版本3

```javascript
// 木易杨
function create() {
	// 1、获得构造函数，同时删除 arguments 中第一个参数
    Con = [].shift.call(arguments);
	// 2、创建一个空的对象并链接到原型，obj 可以访问构造函数原型中的属性
    var obj = Object.create(Con.prototype);
	// 3、绑定 this 实现继承，obj 可以访问到构造函数中的属性
    var ret = Con.apply(obj, arguments);
	// 4、优先返回构造函数返回的对象
	return ret instanceof Object ? ret : obj;
};

```

