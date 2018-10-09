# 前端分享 ---- 2018.10.4

[TOC]

## 问题
### 1. CSS命名混乱
**CSS 模块化**
OOCSS
```
1. 上下文无关
2. 使用 Class,不使用 ID
```
BEM
```
Block、Element、Modifier
核心概念是 —— 块（block）（Nicole 称之为“物体（object）”）由子元素（element）构成，并且可以修改（modified）（或“主题化”）。

eg.
.block-name__element--modifier
```
Scoped CSS
```
<style scoped>
  .pricing-panel {
    width: 300px;
    margin-bottom: 30px;
  }
</style>

编译后

<style>
  .base-panel[data-v-d17eko1] {
    ...
  }
  .pricing-panel[data-v-b52c41] {
    width: 300px;
    margin-bottom: 30px;
  }
</style>
```
CSS Modules
```
<template>
  <button :class="$style.button" />
</template>

<style module>
  .button {
    color: red
  }
</style>

```

**结论**:
1. 在 vue 项目中style块必须使用scoped
2. 命名选取有意义的单词,描述出当前模块的功能,不能使用数字命名如`xx1, xx2`,推荐使用 BEM 的格式

附:
[网易前端规范](http://nec.netease.com/standard/css-practice.html)

### 2. v-if v-for 用在同一组件上
> vue 文档: **永远不要把 v-if 和 v-for 同时用在同一个元素上**。

当 Vue 处理指令时，v-for 比 v-if 具有更高的优先级
如:
```
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
会经过如下计算
```
this.users.map(function (user) {
  if (user.isActive) {
    return user.name
  }
})
```
**结论**:
1. 使用计算属性让其返回过滤后的列表

```
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
      return user.isActive
    })
  }
}
```

2. 将 v-if 移动至容器元素上

```
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

### 3. v-for 未携带 key 值
> vue 文档: **总是用 key 配合 v-for**

react也要求执行列表渲染时也会要求给每个组件添加上key这个属性

key的作用主要是为了高效的更新 Virtual DOM.
基于 diff 算法:
1. 两个相同的组件产生类似的DOM结构，不同的组件产生不同的DOM结构。

2. 同一层级的一组节点，他们可以通过唯一的id进行区分。
![img](https://pic1.zhimg.com/80/v2-bb1147af7c458f0b09d6a3c2f74b0100_hd.png)

基于 key 值得运用,可以使Virtual DOM的Diff算法的复杂度从 O( n^3 ) 降到O(n)

还有过渡效果: 当有相同标签名的元素切换时，需要通过 key 特性设置唯一的值来标记以让 Vue 区分它们
```
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>
```

**结论**:
 不要忘了 key!!!
 
### 4. 研发流程遵循
接受需求变更一定要先评审

## 技术分享
### 深度作用选择器
> 对第三方UI库进行局部样式更改,已经在当前组件范围对子组件进行样式穿透

`>>>`: 用于纯 css 样式
```
<style scoped>
.parent  >>> .child-title {
    XXXX: XXX
    ...
}
</style>
```
`/deep/`: 用于scss
```
<style lang="scss" scoped>
.parent /deep/ .child-title {
    XXXX: XXX
    ...
}
</style>
```

### Vue组件高阶属性
#### 1. `inheritAttrs` default: true
如果在调用子组件时传了子组件未声明的 props, 那么渲染后冗余参数将作为html自定义属性显示在子组件的根元素上
```
//parent.vue
<Child a='aa' b='bb' c="cc">

//child.vue
<script>
export default {
    name: 'child',
    props:['a','b']
}
</script>
```
>>

```js
//child.vue
<script>
export default {
    name: 'child',
    props:['a','b'],
    inheritAttrs:false
}
</script>
```

####2. `$attrs`

vm.$attrs

> 类型：{ [key: string]: string }
只读
包含了父作用域中不作为 prop 被识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind=”$attrs” 传入内部组件——在创建高级别的组件时非常有用。

```
//parent.vue
<template>
    <div>
        <child class="parent-custom" a="a" b="b" c="c"></child>
    </div>
</template>

//child.vue
<script>
export default {
    name: 'child',
    props:['a','b'],
    inheritAttrs:false,
    mounted(){
        console.log('child:$attrs:',this.$attrs); ==> //child:$attrs: {c: "c"}
    }
}
</script>
```

```
//parent.vue
<template>
    <div>
        <child a="a" b="b" c="c"></child>
    </div>
</template>
//child.vue
<template>
    <div>
        <grand-child v-bind="$attrs" d="d"></grand-child>
    </div>
</template>
<script>
export default {
    name: 'child',
    props:['a','b'],
    inheritAttrs:false
}
</script>
//grandchild.vue
<script>
export default {
    name: 'grandchild',
    props:[],
    //props:['c'],
    inheritAttrs:false,
    mounted(){
        console.log('grandchild:$attrs:',this.$attrs); ==> //grandchild:$attrs: {d: "d", c: "c"}

        //如果props:['c']
        //控制台输出:
        //grandchild:$attrs: {d: "d"}
    },
}
</script>
```

#### 3. `provide / inject`
> 类型：
provide：Object | () => Object
inject：Array<string> | { [key: string]: string | Symbol | Object }

这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。
```
//parent.vue
<template>
    <div>
        <child></child>
    </div>
</template>
<script>
export default {
    name: 'parent',
    provide: {
        data: 'I am parent.vue'
    },
    components:{
        Child
    }
}
</script>
//child.vue
<template>
    <div>
        <grand-child></grand-child>
    </div>
</template>
<script>
export default {
    name: 'child',
    components:{
        GrandChild
    }
}
</script>
//grandchild.vue
<script>
export default {
    name: 'grandchild',
    inject: ['data'],
    mounted(){
        console.log('grandchild:inject:',this.data);  => //grandchild:inject: I am parent.vue
    }
}
</script>
```

#### 4. `watch`
对象/数组 的新旧值完全相等
```
data() {
    return {
        obj: {
            a: 1
        }
    }
},
watch: {
    obj: function (newValue, oldValue) {
        oldValue === newValue // true
    }
}
```

```
watch: {
    'obj.a': function (newValue, oldValue) {
        oldValue === newValue // false
    }
}
```