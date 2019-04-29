# 解除双向绑定（深拷贝）

    使用 JSON.parse(JSON.stringify()) 黑科技可以解除双向绑定。

    var copyObj = {
        name: 'ziwei',
        arr : [1,2,3]
    }

    var targetObj = JSON.parse(JSON.stringify(copyObj))

    此时 copyObj.arr !== targetObj.arr  已经实现了深拷贝

    该方法缺陷:

    (1). 如果你的对象里有函数,函数无法被拷贝下来
    (2). 无法拷贝copyObj对象原型链上的属性和方法

# elementUI事件传递自定义参数同时保留原来的参数

    通过搜索找到自定义参数传递方式 (p)=>fn(p, '1', 2)

    但是这种方式有个弊端, this指向已经不是原来的 vm 实例了.

    dom
        :span-method="($event) => objectSpanMethod($event,val.shippingAreaConfigList.length)"
    js
        objectSpanMethod({ row, column, rowIndex, columnIndex } , length) {}

# vue新增属性无法双向绑定问题

    如果要新增多级对象的属性需要使用  this.$set(obj, name, value) 方法新增，否则该属性无法实现 双向绑定

# vue.js 如何在页面渲染完后去操作dom，而且只执行一次？

    在接口请求成功的回调里使用

    this.$nextTick(() =>{
      // 在这里面去获取DOM
    })。

    不推荐用updated, beforeUpdate生命周期，这2个生命周期只会在数据发生变化时才触发。如果请求接口的数据是放在created生命周期（推荐放在created里面去发起请求），初次进入页面是不会触发updated, beforeUpdate里面的代码。
    如果非要updated，并且希望第一次进入页面即可获取到DOM节点，那么在mounted生命周期请求接口数据，而不是created了




