## Prop / $emit

### Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 Prop 特性的时候，它就变成了那个组件实例的一个属性 。父组件向子组件传值，通过绑定属性来向子组件传入数据，子组件通过 Props 属性获取对应数据。

    // 父组件
    <template>
      <div class="container">
        <child :title="title"></child>
      </div>
    </template>

    <script>
    import Child from "./component/child.vue";
    export default {
      name: "demo",
      data: function() {
        return {
          title: "我是父组件给的"
        };
      },
      components: {
        Child
      },
    };
    </script>

    // 子组件
    <template>
      <div class="text">{{title}}</div>
    </template>

    <script>
    export default {
      name: 'demo',
      data: function() {},
      props: {
        title: {
          type: String
        }
      },
    };
    </script>

### $emit 子组件向父组件传值（通过事件形式），子组件通过 $emit 事件向父组件发送消息，将自己的数据传递给父组件。

    // 父组件
    <template>
      <div class="container">
        <div class="title">{{title}}</div>
        <child @changeTitle="parentTitle"></child>
      </div>
    </template>

    <script>
    import Child from "./component/child.vue";

    export default {
      name: "demo",
      data: function() {
        return {
          title: null
        };
      },
      components: {
        Child
      },
      methods: {
        parentTitle(e) {
          this.title = e;
        }
      }
    };
    </script>

    // 子组件
    <template>
      <div class="center">
        <button @click="childTitle">我给父组件赋值</button>
      </div>
    </template>

    <script>
    export default {
      name: 'demo',
      data() {
        return {
          key: 1
        };
      },
      methods: {
        childTitle() {
          this.$emit('changeTitle', `我给父组件的第${this.key}次`);
          this.key++;
        }
      }
    };
    </script>

**小总结：常用的数据传输方式，父子间传递。**

## $emit / $on

### 这个方法是通过创建了一个空的 vue 实例，当做 $emit 事件的处理中心（事件总线），通过他来触发以及监听事件，方便的实现了任意组件间的通信，包含父子，兄弟，隔代组件。

    // 父组件
    <template>
      <div class="container">
        <child1 :Event="Event"></child1>
        <child2 :Event="Event"></child2>
        <child3 :Event="Event"></child3>
      </div>
    </template>

    <script>
    import Vue from "vue";

    import Child1 from "./component/child1.vue";
    import Child2 from "./component/child2.vue";
    import Child3 from "./component/child3.vue";

    const Event = new Vue();

    export default {
      name: "demo",
      data: function() {
        return {
          Event: Event
        };
      },
      components: {
        Child1,
        Child2,
        Child3
      },
    };
    </script>

    // 子组件1
    <template>
      <div class="center">
        1.我的名字是：{{name}}
        <button @click="send">我给3组件赋值</button>
      </div>
    </template>

    <script>
    export default{
      name: "demo1",
      data() {
        return {
          name: "政采云"
        };
      },
      props: {
        Event
      },
      methods: {
        send() {
          this.Event.$emit("message-a", this.name);
        }
      }
    };
    </script>

    // 子组件2
    <template>
      <div class="center">
        2.我的年龄是：{{age}}岁
        <button @click="send">我给3组件赋值</button>
      </div>
    </template>

    <script>
    /* eslint-disable */
    export default {
      name: "demo2",
      data() {
        return {
          age: "3"
        };
      },
      props: {
        Event
      },
      methods: {
        send() {
          this.Event.$emit("message-b", this.age);
        }
      }
    };
    </script>

    // 子组件3
    <template>
      <div class="center">我的名字是{{name}}，今年{{age}}岁</div>
    </template>

    <script>
    export default{
      name: 'demo3',
      data() {
        return {
          name: '',
          age: ''
        };
      },
      props: {
        Event
      },
      mounted() {
        this.Event.$on('message-a', name => {
          this.name = name;
        });
        this.Event.$on('message-b', age => {
          this.age = age;
        });
      },
    };
    </script>

**小总结：巧妙的在父子，兄弟，隔代组件中都可以互相数据通信。**

## Vuex

### Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

    Mutation ：是修改State数据的唯一推荐方法，且只能进行同步操作。

    Getter ：Vuex 允许在Store中定义 “ Getter”（类似于 Store 的计算属性）。Getter 的返回值会根据他的依赖进行缓存，只有依赖值发生了变化，才会重新计算。

**小总结：统一的维护了一份共同的 State 数据，方便组件间共同调用。**

## $attrs / $listeners

    Vue 组件间传输数据在 Vue 2.4 版本后有了新方法。除了 Props 外，还有了 $attrs / $listeners。

### $attrs： 包含了父作用域中不作为 Prop 被识别 (且获取) 的特性绑定(Class 和  Style 除外)。当一个组件没有声明任何 Prop 时，这里会包含所有父作用域的绑定 (Class 和 Style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件——在创建高级别的组件时非常有用。

### $listeners: 包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件

    // 父组件
    <template>
      <div class="container">
        <button style="backgroundColor:lightgray" @click="reduce">减dd</button>
        <child1 :aa="aa" :bb="bb" :cc="cc" :dd="dd" @reduce="reduce"></child1>
      </div>
    </template>

    <script>
    import Child1 from './component/child1.vue';
    export default {
      name: 'demo',
      data: function() {
        return {
          aa: 1,
          bb: 2,
          cc: 3,
          dd: 100
        };
      },
      components: {
        Child1
      },
      methods: {
        reduce() {
          this.dd--;
        }
      }
    };
    </script>

    // 子组件1
    <template>
      <div>
        <div class="center">
          <p>aa:{{aa}}</p>
          <p>child1的$attrs:{{$attrs}}</p>
          <button @click="this.reduce1">组件1减dd</button>
        </div>
        <child2 v-bind="$attrs" v-on="$listeners"></child2>
      </div>
    </template>

    <script>
    import child2 from './child2.vue';
    export default {
      name: 'demo1',
      data() {
        return {};
      },
      props: {
        aa: Number
      },
      components: {
        child2
      },
      methods: {
        reduce1() {
          this.$emit('reduce');
        }
      }
    };
    </script>

    // 子组件2
    <template>
      <div>
        <div class="center">
          <p>bb:{{bb}}</p>
          <p>child2的$attrs:{{$attrs}}</p>
          <button @click="this.reduce2">组件2减dd</button>
        </div>
        <child3 v-bind="$attrs"></child3>
      </div>
    </template>

    <script>
    import child3 from './child3.vue';
    export default {
      name: 'demo1',
      data() {
        return {};
      },
      props: {
        bb: Number
      },
      components: {
        child3
      },
      methods: {
        reduce2() {
          this.$emit('reduce');
        }
      }
    };
    </script>

**简单来说，$attrs 里存放的是父组件中绑定的非 props 属性，$listeners 里面存放的是父组件中绑定的非原生事件。**

**小总结：当传输数据、方法较多时，无需一一填写的小技巧。**

## Provider / Inject

    Vue 2.2 版本以后新增了这两个 API ， 这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在其上下游关系成立的时间里始终生效。 简单来说，就是父组件通过 Provider 传入变量，任意子孙组件通过 Inject 来拿到变量。

    // 父组件
    <template>
      <div class="container">
        <button @click="this.changeName">我要改名字了</button>
        <p>我的名字：{{name}}</p>
        <child1></child1>
      </div>
    </template>

    <script>
    import Child1 from './component/child1.vue';
    export default {
      name: 'demo',
      data: function() {
        return {
          name: '政采云'
        };
      },
      // provide() {
      //   return {
      //     name: this.name //这种绑定方式是不可响应的
      //   };
      // },
      provide() {
        return {
          obj: this
        };
      },
      components: {
        Child1
      },
      methods: {
        changeName() {
          this.name = '政采云前端';
        }
      }
    };
    </script>

    // 子组件
    <template>
      <div>
        <div class="center">
          <!-- <p>子组件名字:{{name}}</p> -->
          <p>子组件名字:{{this.obj.name}}</p>
        </div>
        <child2></child2>
      </div>
    </template>

    <script>
    import child2 from './child2.vue';

    export default {
      name: 'demo1',
      data() {
        return {};
      },
      props: {},
      // inject: ["name"],
      inject: {
        obj: {
          default: () => {
            return {};
          }
        }
      },
      components: {
        child2
      },
    };
    </script>

**小总结：传输数据父级一次注入，子孙组件一起共享的方式。**

## $parent / $children & $refs

### $parent / $children： 指定已创建的实例之父实例，在两者之间建立父子关系。子实例可以用 this.$parent 访问父实例，子实例被推入父实例的 $children 数组中。

### $refs： 一个对象，持有注册过 ref 特性[3] 的所有 DOM 元素和组件实例。ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件。

    // 父组件
    <template>
      <div class="container">
        <p>我的title：{{title}}</p>
        <p>我的name：{{name}}</p>
        <child1 ref="comp1"></child1>
        <child2 ref="comp2"></child2>
      </div>
    </template>

    <script>
    import Child1 from './component/child1.vue';
    import Child2 from './component/child2.vue';
    export default {
      name: 'demo',
      data: function() {
        return {
          title: null,
          name: null,
          content: '就是我'
        };
      },
      components: {
        Child1,
        Child2
      },
      mounted() {
        const comp1 = this.$refs.comp1;
        this.title = comp1.title;
        comp1.sayHello();
        this.name = this.$children[1].title;
      },
    };
    </script>

    // 子组件1-ref方式
    <template>
      <div>
        <div class="center">我的父组件是谁:{{content}}</div>
      </div>
    </template>

    <script>
    export default {
      name: 'demo1',
      data() {
        return {
          title: '我是子组件',
          content: null
        };
      },
      mounted() {
        this.content = this.$parent.content;
      },
      methods: {
        sayHello() {
          window.alert('Hello');
        }
      }
    };
    </script>

    // 子组件2-children方式
    <template>
      <div>
        <div class="center"></div>
      </div>
    </template>

    <script>
    export default{
      name: 'demo1',
      data() {
        return {
          title: '我是子组件2'
        };
      },
    };
    </script>

**小总结：父子组件间共享数据以及方法的便捷实践之一。**

## 总结

    • 父子通信：Props / $emit，$emit / $on，Vuex，$attrs / $listeners，provide/inject，$parent / $children＆$refs

    • 兄弟通信：$emit / $on，Vuex

    • 隔代（跨级）通信：$emit / $on，Vuex，provide / inject，$attrs / $listeners














