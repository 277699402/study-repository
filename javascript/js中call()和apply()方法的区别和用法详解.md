# call()和apply()方法的区别和用法详解

## 定义

    每个函数都包含两个非继承而来的方法：call()方法和apply()方法。

    call和apply可以用来重新定义函数的执行环境，也就是this的指向; call和apply都是为了改变函数运行时的context，即上下文而存在的，也就是为了改变函数内部this的指向

## 语法

**call()**

调用一个对象的方法，用*另一个对象替换当前对象*,可以继承另外一个对象的属性，它的语法是:

    Function.call(obj[, param1[, param2[, [,...paramN]]]]);

    obj: 这个对象将代替Function类中的this对象
    params: 一串参数列表

    说明: call方法可以用来代替另一个对象调用一个方法，call方法可以将一个函数的对象上下文从初始的上下文改变为obj指定的新对象，如果没有提供obj参数，那么Global对象被用于obj。

**apply()**

    和call()方法一样，只是参数列表不同，语法：

    Function.apply(obj[, argArray]);

    obj：这个对象将代替Function类里this对象
    argArray：这个是数组，它将作为参数传给Function

    说明：如果argArray不是一个有效数组或不是arguments对象，那么将导致一个TypeError，如果没有提供argArray和obj任何一个参数，那么Global对象将用作obj。

## 相同点

    call()和apply()方法的相同点就是这两个方法的作用是一样的。都是在特定的作用域中调用函数，等于设置函数体内this对象的值，以扩充函数赖以运行的作用域。

    一般来说，this总是指向调用某个方法的对象，但是使用call()和apply()方法时，就会改变this的指向，看个例子：

    function add(a, b) {
        return a + b;
    }

    function sub(a, b) {
        return a - b;
    }

    console.log(add.call(sub, 2, 1));//3

    为什么add.call(sub, 2, 1)的执行结果是3呢，因为call()方法改变了this的指向，使得sub可以调用add的方法，也就是用sub去执行add中的内容，再来看一个例子：

    function People(name, age) {
        this.name = name;
        this.age = age;
    }

    function Student(name, age, grade) {
        People.call(this, name, age);
        this.grade = grade;
    }

    var student = new Student('小明', 21, '大三');
    console.log(student.name + student.age + student.grade);//小明21大三

    在这个例子中，我们并没有给Student的name和age赋值，但是存在这两个属性的值，这还是要归功于call()方法，它可以改变this的指向。
    在这个例子里，People.call(this, name, age);中的this代表的是Student，这也就是之前说的，使得Student可以调用People中的方法，因为People中有this.name = name;等语句，这样就将name和age属性创建到了Student中。

    总结一句话就是call()可以让**括号里的对象来继承括号外函数的属性**。

    至于apply()方法作用也和call()方法一样，可以这么写:

    People.apply(this, [name, age]);

    或者这么写：

    People.apply(this, arguments);

### 不同点

    从定义中也可以看出来，call()和apply()的不同点就是接收参数的方式不同。

    apply()方法接收两个参数，一个是函数运行的作用域（this），另一个是参数数组。
    call()方法不一定接受两个参数，第一个参数也是函数运行的作用域（this），但是传递给函数的参数必须列举出来。

    在给对象参数的情况下,如果参数的形式是数组的时候,比如之前apply()方法示例里面传递了参数arguments,这个参数是数组类型,并且在调用Person的时候参数的列表是对应一致的(也就是Person和Student的参数列表前两位是一致的)就可以采用apply()方法。

    但是如果Person的参数列表是这样的(age,name)，而Student的参数列表是(name,age,grade)，这样就可以用call()方法来实现了,也就是直接指定参数列表对应值的位置Person.call(this,age,name)。

### apply()的其他用法

**apply有一个巧妙的用处,就是可以将一个数组默认的转换为一个参数列表**

    ([param1,param2,param3]转换为param1,param2,param3)，借助apply的这点特性，所以就有了以下高效率的方法：

**Math.max可以实现得到数组中最大/最小的一项**

    因为Math.max参数里面不支持Math.max([param1,param2])，也就是数组，但是它支持Math.max(param1,param2,param3…)，所以可以根据apply的那个特点来解决：

    var array = [1, 2, 3];
    var max = Math.max.apply(null, array);
    console.log(max);//3

    这样轻易的可以得到一个数组中最大的一项，apply会将一个数组装换为一个参数接一个参数的传递给方法，这块在调用的时候第一个参数给了一个null，这个是因为没有对象去调用这个方法，我们只需要用这个方法帮我运算，得到返回的结果就行，所以直接传递了一个null过去，当然，第一个参数使用this也是可以的

    var array = [1, 2, 3];
    var max = Math.max.apply(this, array);
    console.log(max);//3

    使用this就相当于用全局对象去调用Math.max，所以也是一样的。

**Array.prototype.push可以实现两个数组合并**

    同样的，push方法没有提供push一个数组，但是它提供了push(param1,param,…paramN)所以同样也可以通过apply来装换一下这个数组，即:

    var arr1 = [1, 2, 3];
    var arr2 = [4, 5, 6];
    Array.prototype.push.apply(arr1, arr2);
    console.log(arr1);//[ 1, 2, 3, 4, 5, 6 ]

    可以这样理解，arr1调用了Array的push方法，参数是通过apply将数组装换为参数列表的集合，其实，arr1也可以调用自己的push方法：

    var arr1 = [1, 2, 3];
    var arr2 = [4, 5, 6];
    arr1.push.apply(arr1, arr2);
    console.log(arr1);//[ 1, 2, 3, 4, 5, 6 ]

    也就是只要有push方法，arr1就可以利用apply方法来调用该方法，以及使用apply方法将数组转换为一系列参数，所以也可以这样写：

    var arr1 = [1, 2, 3];
    var arr2 = [4, 5, 6];
    [].push.apply(arr1, arr2);
    console.log(arr1);//[ 1, 2, 3, 4, 5, 6 ]




