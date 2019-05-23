### 通用原生Javascript 实现AJAX

**同时使用hasOwnProperty()方法和in操作符，就可以确定该对象属性到底是否存在于对象中，还是存在于原型中**

    function hasPrototypeProperty(object, name) {
        return !object.hasOwnProperty(name) && (name in object)
    }


