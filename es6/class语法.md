
## 1.class类语法 ##

    class Point {
        constructor (x,y) {         //构造方法
            this.x = x;
            this.y = y;
        }
        toString () {               //类的方法
            return this.x + this.y;
        }
    }
    var point = new Point(1,2);
    point.toString();

    (1)一次向类添加多个方法。
        class Point {
            constructor () {}
        }
        Object.assign(Point.prototype, {
            toString () {},
            toValue () {}
        });
    (2) 实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）
        point.hasOwnProperty('x') // true
        point.hasOwnProperty('y') // true
        point.hasOwnProperty('toString') // false
        point.__proto__.hasOwnProperty('toString') // true

# 2.取值函数（getter）和存值函数（setter）

### 在“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

    class MyClass {
      constructor() {
        // ...
      }
      get prop() {
        return 'getter';
      }
      set prop(value) {
        console.log('setter: '+value);
      }
    }

    let inst = new MyClass();

    inst.prop = 123;
    // setter: 123

    inst.prop
    // 'getter'

# 3. 属性表达式

    let methodName = 'getArea';
    class Square = {
        [methodName] () {}
    }

# 4.name属性

    class Point {}
    Point.name  //Point

# 5.静态方法

### 类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

    class Foo {
      static classMethod() {
        return 'hello';
      }
    }

    Foo.classMethod() // 'hello'

    var foo = new Foo();
    foo.classMethod()
    // TypeError: foo.classMethod is not a function

    (1)父类的静态方法，可以被子类继承。
    (2)静态方法也是可以从super对象上调用的。

# 6.静态属性

###  静态属性指的是 Class 本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性。

# 7.new.target属性

### new是从构造函数生成实例对象的命令。

    function Person(name) {
      if (new.target !== undefined) {
        this.name = name;
      } else {
        throw new Error('必须使用 new 命令生成实例');
      }
    }

    // 另一种写法
    function Person(name) {
      if (new.target === Person) {
        this.name = name;
      } else {
        throw new Error('必须使用 new 命令生成实例');
      }
    }

    var person = new Person('张三'); // 正确
    var notAPerson = Person.call(person, '张三');  // 报错

    需要注意的是，子类继承父类时，new.target会返回子类。

    利用这个特点，可以写出不能独立使用、必须继承后才能使用的类。

    class Shape {
      constructor() {
        if (new.target === Shape) {
          throw new Error('本类不能实例化');
        }
      }
    }

    class Rectangle extends Shape {
      constructor(length, width) {
        super();
        // ...
      }
    }

    var x = new Shape();  // 报错
    var y = new Rectangle(3, 4);  // 正确




