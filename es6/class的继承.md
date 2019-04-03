# Class的继承

## Class 可以通过extends关键字实现继承

    class Point {}

    class ColorPoint extends Point {
        constructor (x, y, color) {
            super(x,y); // 调用父类的constructor(x, y)
            this.color = color;
        }
        toString () {
            return this.color + '' + super.toString();  // 调用父类的toString()
        }
    }

    var colorPoint = new ColorPoint(25, 8, 'green');

    cp instanceof ColorPoint // true
    cp instanceof Point // true

    `子类必须在constructor方法中调用super方法，否则新建实例时会报错`

### 父类的静态方法，也会被子类继承

    class A {
      static hello() {
        console.log('hello world');
      }
    }

    class B extends A {
    }

    B.hello()  // hello world

    上面代码中，hello()是A类的静态方法，B继承A，也继承了A的静态方法。

## Class 其他内容

* Object.getPrototypeOf()
    > Object.getPrototypeOf方法可以用来从子类上获取父类。

        Object.getPrototypeOf(ColorPoint) === Point
        // true

        因此，可以使用这个方法判断，一个类是否继承了另一个类。

* super 关键字
    > super作为函数调用时，代表父类的构造函数。子类的构造函数必须执行一次super函数

        class A {}

        class B extends A {
          constructor() {
            super();
          }
        }

        注意，super虽然代表了父类A的构造函数，但是返回的是子类B的实例，即super内部的this指的是B的实例，因此super()在这里相当于A.prototype.constructor.call(this)。

    > super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

        class A {
          p() {
            return 2;
          }
        }

        class B extends A {
          constructor() {
            super();
            console.log(super.p()); // 2
          }
        }

        let b = new B();

        上面代码中，子类B当中的super.p()，就是将super当作一个对象使用。这时，super在普通方法之中，指向A.prototype，所以super.p()就相当于A.prototype.p()。

        这里需要注意，由于super指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的。

        class A {
          constructor() {
            this.p = 2;
          }
        }

        class B extends A {
          get m() {
            return super.p;
          }
        }

        let b = new B();
        b.m // undefined

        上面代码中，p是父类A实例的属性，super.p就引用不到它。

* 类的 prototype 属性和__proto__属性
    > Class 作为构造函数的语法糖，同时有prototype属性和__proto__属性，因此同时存在两条继承链。

    1. 子类的__proto__属性，表示构造函数的继承，总是指向父类。

    2. 子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。

        class A {}

        class B extends A {}

        B.__proto__ === A // true
        B.prototype.__proto__ === A.prototype // true






