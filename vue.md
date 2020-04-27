# vue



### 1.v-model

一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，

```vue
// 父
<my-input v-model="name"></my-input>
//子组件
<template>
  <div>
    <input type="text"
           v-on:change="$emit('input', $event.target.value)"
           :value="value"
           id="">
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  },
  props: {
    value: {
      type: String // 定义value属性
    }
  },
  computed: {
  }
}
</script>

```

但是像单选框、复选框等类型的输入控件可能会将 `value` attribute 用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。`model` 选项可以用来避免这样的冲突：

```vue
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

现在在这个组件上使用 `v-model` 的时候：

```
<base-checkbox v-model="lovingVue"></base-checkbox>
```

这里的 `lovingVue` 的值将会传入这个名为 `checked` 的 prop。同时当 `<base-checkbox>` 触发一个 `change` 事件并附带一个新的值的时候，这个 `lovingVue` 的属性将会被更新。

### 2.sync 修饰符

 

```vue
// 父
 <my-input :name.sync='name'></my-input>
//子组件
<template>
  <div>
    <input type="text"
           v-on:input="$emit('update:name', $event.target.value)"
           id="">
  </div>
</template>

<script>
export default {
  data () {
    return {
      value: ''
    }
  },
  props: {
    name: {
      type: String // 定义value属性
    }
  },
</script>

```

## 生命周期![1586334579376](C:\Users\56515\AppData\Roaming\Typora\typora-user-images\1586334579376.png)

### vue父子组件 渲染

##### 一、 同步引入

例子： import Page from '@/components/page'

##### 二、异步引入

例子：const Page = () => import('@/components/page')
或者： const Page = resolve => require(['@/components/page'], page)

同步引入时生命周期顺序为：父组件的beforeCreate、created、beforeMount --> 所有子组件的beforeCreate、created、beforeMount --> 所有子组件的mounted --> 父组件的mounted

异步引入时生命周期顺序：父组件的beforeCreate、created、beforeMount、mounted --> 子组件的beforeCreate、created、beforeMount、mounted


### nextTick

我们先来看一段Vue的执行代码：

```js
export default {
  data () {
    return {
      msg: 0
    }
  },
  mounted () {
    this.msg = 1
    this.msg = 2
    this.msg = 3
  },
  watch: {
    msg () {
      console.log(this.msg)
    }
  }
}
// 实际效果中，只会输出一次：3

```

数据的变化到 DOM 的重新渲染是一个异步过程，发生在下一个 tick。

vue的数据响应过程包含：数据更改->通知Watcher->更新DOM。而数据的更改不由我们控制，可能在任何时候发生。如果恰巧发生在repaint之前，就会发生多次渲染。

Vue 在更新 DOM 时是异步执行的，只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次，这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环tick中， Vue 刷新队列并执行实际(已去重)的工作。 

```js

<div id="app">
    <p ref="message">{{ message }}</p>
    <button @click="handleClick">updateMessage</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: '未更新'
        },
        methods: {
            handleClick () {
                vm.message = '已更新';
                console.log(111, vm.$refs.message.innerText); // => 111 "未更新"
                vm.$nextTick(() => {
                    console.log(222, vm.$refs.message.innerText); // => 222 "已更新"
                })
            }
        }
    })
</script>

```

Vue.js 提供了 2 种调用 `nextTick` 的方式，一种是全局 API `Vue.nextTick`，一种是实例上的方法 `vm.$nextTick`

官方文档里面还有这么一句话，如果没有提供回调且支持Promise的环境下，则返回一个Promise。也就是说。可以这样使用nextTick

```js
this.$nextTick().then(function(){
    //dom跟新了
})
```

以上就是vue的nextTick方法的实现原理了，总结一下就是：

1. vue用异步队列的方式来控制DOM更新和nextTick回调先后执行
2. microtask因为其高优先级特性，能确保队列中的微任务在一次事件循环前被执行完毕
3. 因为兼容性问题，vue不得不做了microtask向macrotask的降级方案

## 自定义指令

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- `update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

- `componentUpdated`：指令所在组件的 VNode **及其子 VNode** 全部更新后调用。
- `unbind`：只调用一次，指令与元素解绑时调用。



## VUE插槽

### 基础用法

```vue
var child = {
  template: `<div class="child"><slot>后备内容</slot></div>` //如果父组件没传 显示 后背内容
}
var vm = new Vue({
  el: '#app',
  components: {
    child
  },
  template: `<div id="app"><child>test</child></div>`
})
// 最终渲染结果
<div class="child">test</div>


```

- 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

### v-slot指令

`<slot>` 元素有一个特殊的 attribute：`name`。这个 attribute 可以用来定义额外的插槽：

```js
//在向具名插槽提供内容的时候，我们可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称
//-----------------------------------------1
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>
、、
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>
、、
  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
//----------------------------------------2
<base-layout>
  <template v-slot:header ='headerData'> // headerData由子组件传入
// ----子组件<slot name="header" title='header-title'></slot>//
    <h1>Here might be a page title {{headerData.XXXX}}</h1>
  </template>
、、
    
  <template v-slot:default >
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>
、、
  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。 1,2是一样的
```

在child中打印 this.$slots 会显示  一个对象，对象的key是slot的name ,value是插槽的Vnode的数组

一个不带 `name` 的 `<slot>` 出口会带有隐含的名字“default”。

### 独占默认插槽的缩写语法

在上述情况下，当被提供的内容*只有*默认插槽时，组件的标签才可以被当作插槽的模板来使用。这样我们就可以把 `v-slot` 直接用在组件上：

```
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里：

```
function (slotProps) {
  // 插槽内容
}
```

这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样开启了 prop 重命名等其它可能，例如将 `user` 重命名为 `person`：

```
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```

## 实战距离：权限组件

### 一：利用组件包裹，slot的形式...来判断是否显示

![](C:\Users\56515\Desktop\blog\blog_\QQ图片20200419141745.png)

### 二：利用mixin模式

```js
export default rightType => ({ // rightType作为参数传入，返回特定mixin
  computed: {
    hasRight() { // 判断用户是否有权限进入本页面的计算属性
      // 这里的user是之前在app中通过接口返回注入store的用户信息
      const { rightList } = this.$store.state.user;
      return rightList.indexOf(rightType); // 问题解决，美滋滋
    }
  }
})

//--------------------------------------------------------------
/**************** page1.vue ****************/
import NoRightTips from './no-right-tips';
import rightmixin from './right-mixin';

export default {
  mixin: [rightmixin('RIGHT_PAGE_1')],
  template: `
    <div v-if="hasRight">欢迎访问传说中的 page1 !</div>
    <no-right-tips v-else></no-right-tips>
  `,
  components: {
    NoRightTips
  }
}
```

### 三：高阶组件

```js
import NoRightTips from './no-right-tips';

export default (Comp, rightType) => ({
  components: {
    Comp,
    NoRightTips,
  },
  computed: {
    hasRight() {
      const { rightList } = this.$store.state.user;
      return rightList.indexOf(rightType);
    }
  },
  render(h) {
    return this.hasRight ? h(Comp, {}) : h(NoRightTips, {});
  }
})
```

高阶组件的作用，就是为了增强某个组件而存在的。

