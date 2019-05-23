### 通用原生Javascript 实现AJAX

**同时使用hasOwnProperty()方法和in操作符，就可以确定该对象属性到底是否存在于对象中，还是存在于原型中**

    使用in操作符无论该属性在于实例中还是原型中能够访问到属性时返回true
    使用hasOwnProperty()方法只有能够访问到实例属性时才会返回true

    function hasPrototypeProperty(object, name) {
        return !object.hasOwnProperty(name) && (name in object)
    }


