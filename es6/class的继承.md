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

* Object.getPrototypeOf()
    > Object.getPrototypeOf方法可以用来从子类上获取父类。

        Object.getPrototypeOf(ColorPoint) === Point
        // true
