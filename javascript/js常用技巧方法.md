## 确定该对象属性到底是否存在于对象中，还是存在于原型中

**同时使用hasOwnProperty()方法和in操作符，就可以确定该对象属性到底是否存在于对象中，还是存在于原型中**

    使用in操作符无论该属性在于实例中还是原型中能够访问到属性时返回true
    使用hasOwnProperty()方法只有能够访问到实例属性时才会返回true

    function hasPrototypeProperty(object, name) {
        return !object.hasOwnProperty(name) && (name in object)
    }

## 可以使用es5中的 Object.keys() 方法来取得对象上所有可枚举的实例属性

    function Person() {}
    Person.prototype.name = 'aa';
    Person.prototype.age = 29;

    var keys = Object.keys(Person.prototype);  //['name','age']

    var p1 = new Person();
    p1.name = 'bb';
    var p1keys = Object.keys(p1);       //['name']

**如果想要得到所有实例属性，无论是否可枚举，都可以使用 Object.getOwnPropertyNames() 方法**

    var keys = Object.getOwnPropertyNames(Person.prototype);  //['constructor',name,age]

**注意结果中包含了不可枚举的constructor属性**

## 原型链实现继承

    function SuperType() {
        this.property = true;
    }

    SuperType.prototype.getSuperValue = function () {
        return this.property;
    }

    function SubType() {
        this.subproperty = false;
    }

    //继承了SuperType
    SubType.prototype = new SuperType();

    SubType.prototype.getSubValue = function () {
        return this.subproperty;
    }

    var instance = new SubType();
    console.log(instance.getSuperValue);  //true

## 函数递归

    普通模式:

        function factory(num) {
            return num <= 1 ? 1 : num * arguments.callee(num - 1);   //arguments.callee 是一个指向正在执行的函数的指针
        }

    严格模式:

        var factory = (function f(num) {
            return num <= 1 ? 1 : num * f(num - 1);
        })

## 闭包

**有权访问另外一个函数作用域中的变量的函数**

    function aa(propertyName) {
        return function(object) {
            var value = object[propertyName]
        }
    }

## 判断某个值是不是原生数组/函数/正则表达式

    function isArray(value) {
        return Object.prototype.toString.call(value) == '[object Array]'
    }
    function isFunction(value) {
        return Object.prototype.toString.call(value) == '[object Function]'
    }
    function isRegExp(value) {
        return Object.prototype.toString.call(value) == '[object RegExp]'
    }

## 实现类数组对象

所谓类数组对象，就是指可以**通过索引属性访问元素**并且**拥有 length** 属性的对象。

    var arrLike = {
      0: 'name',
      1: 'age',
      2: 'job',
      length: 3
    }

**类数组对象与数组的区别是类数组对象不能直接使用数组的方法。 **

    我们一般是通过 Function.call 或者 Function.apply 方法来间接调用数组的方法。

    Array.prototype.push.call(arrLike, 'hobby');
    Array.prototype.push.apply(arrLike, ['hobby']);


***使用 [].push.call() 可以实现一个类数组对象***


