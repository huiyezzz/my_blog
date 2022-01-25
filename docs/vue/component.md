<!--
 * @Author: xx
 * @Date: 2021-06-22 17:52:58
 * @LastEditors: 青峰
 * @LastEditTime: 2021-06-22 18:16:26
 * @FilePath: /vue-press/docs/vue/component.md
-->

# 组件

## 基础

### 基本使用

> 使用Vue.component('组件名',{//没有el,与new Vue接受相同的选项})
Demo

html

```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

js

```js
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
});
new Vue({el:'#components-demo'});
```

组件可以复用，但要注意的是**data必须是一个函数**防止多个组件共享data

```js
data: function () {
  return {
    count: 0
  }
}
```

## 组件间通信

### 父组件向子组件传值

> prop

子组件

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

父组件

```js
<blog-post title="My journey with Vue"></blog-post>
```

### 子组件通知父组件

> $emit 发射事件通知父组件

```js
$emit(事件名,内容)
```

父组件

```js
 <custome-input :cs-value="csValue" @change="change">
    </custome-input>
```

子组件

```js
Vue.component('custome-input',{
  props:['cs-value'],
  template:'<input :value="csValue" @change="$emit(\'change\',$event.target.value)"></input>'
});
```

### 无关联的组件

使用```vuex```共享数据，或者```EventBus```(使用事件总线时要留意组件销毁后订阅是否也销毁)

## 自定义组件的v-model

> v-model默认会根据不同的类型来监听不同事件，自定义组件时可以通过“model”来自定义

官方示例

```js
Vue.component('base-checkbox', {
  model: {
    prop: 'checked', // 名字
    event: 'change' // 监听的事件
  },
  props: {
    checked: Boolean // 声明
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

## 绑定原生事件

> .native

```html
<base-input v-on:focus.native="onFocus"></base-input>
```

> $listeners 一个对象，包含了作用在这个组件上的所有监听器

官方使用示例

```js
Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  computed: {
    inputListeners: function () {
      var vm = this
      // `Object.assign` 将所有的对象合并为一个新对象
      return Object.assign({},
        // 我们从父级添加所有的监听器
        this.$listeners,
        // 然后我们添加自定义监听器，
        // 或覆写一些监听器的行为
        {
          // 这里确保组件配合 `v-model` 的工作
          input: function (event) {
            vm.$emit('input', event.target.value)
          }
        }
      )
    }
  },
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on="inputListeners"
      >
    </label>
  `
})
```

### .sync

> 通过自定义的方式实现双向绑定，遵循update:myPropName模式
html

```html
<text-document v-bind:title.sync="doc.title"></text-document>
```

js

```js
this.$emit('update:title', newTitle)
```

## 插槽

### 基本使用

组件模版

```vue
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

html中使用

```html
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

当组件渲染的时候，```<slot></slot>``` 将会被替换为“Your Profile”,
插槽内可以包含任何模板代码,或组件。

### 编译作用域

> 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

```html
<navigation-link url="/profile">
  Clicking here will send you to: {{ url }}
  <!--
  这里的 `url` 会是 undefined，因为 "/profile" 是
  _传递给_ <navigation-link> 的而不是
  在 <navigation-link> 组件*内部*定义的。
  -->
</navigation-link>
```

为了让 user 在父级的插槽内容中可用，我们可以将 user 作为 ```<slot>``` 元素的一个 ```attribute``` 绑定上去

```html
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

在父级作用域中使用带值的 v-slot 来定义我们提供的插槽prop的名字：

```html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
  或
    <template #default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

### 解构插槽prop

> 作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里,这意味着 v-slot 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式

```html
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```

解构插槽prop拿到的是包含当前插槽绑定的所有数据形如{a:{},b:{}}

使用解构```v-slot={ a: A }```那么A表示的就是a所对应的数据

### 多个插槽

```<slot>``` 元素有一个特殊的 attribute：name。这个 attribute 可以用来定义额外的插槽

 定义

 ```html
  <slot name="header"></slot>
  ```

  使用
  
```html
    <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>
  ```
  
## 综合练习

封装一个待办组件

组件功能：

- 添加待办集
- 自定义待办集名称
- 添加待办
- 待办数限制
- 完成待办

```vue
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title></title>
    <link rel="stylesheet" href="">
</head>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<style type="text/css" media="screen">
 .over{
  text-decoration: line-through;
 }
</style>
<script type="text/javascript">
Vue.component('custome-input', {
    model: {
        prop: "inputValue",
        event: 'myChange'
    },
    data: function() {
        return {
            log: []
        }
    },
    props: {
        tableTitle: {
            type: String,
            default: '默认待办事项集'
        },
        title: String,
        inputValue: String
    },
    template: `<label for="checkbox">做点什么呢？
  <input id="checkbox" :value="inputValue"  v-on="inputListener" />
  
          <table>
         <caption>{{tableTitle}}</caption>
         <thead>
          <tr >
          <th></th>
           <th>序号</th>
           <th>待办</th>
           <th>创建时间</th>
          </tr>
         </thead>
         <tbody>
          <tr :class={over:item.did} v-for="(item,index) in log" :key="item.id">
          {{item.did}}
           <td><input type="checkbox" v-model="item.did"></td>
           <td>{{index+1}}</td>
           <td>{{item.content}}</td>
           <td>{{item.date}}</td>
          </tr>
         </tbody>
        </table>
        </label>
        `,
    computed: {
        inputListener: function() {
            var vm = this;
            return Object.assign({},
                this.$listeners, {
                    input: function(event) {

                        vm.$emit('input', event.target.value);
                    },
                    change: (event) => {
                        if (this.log.length > 3) {
                            this.$emit('haha', '记录已达上限，请升级VIP🔓');
                            // return;
                        } else {
                            vm.log.push({ content: event.target.value, date: new Date().toLocaleDateString(),did:false ,id:new Date().getTime()});
                            vm.$emit('myChange', event.target.value);

                        }
                        // vm.$emit('update:title', '我被更新了');

                    }
                });
        }
    },
    watch: {
        log: function() {
            if (this.log.length > 4) {
                this.$emit('haha', '记录已达上限，请升级VIP🔓');
            }
        }
    }
});
</script>

<body>
    <div id="app-1">
        <button type="button" @click="addToDoWorkSpace">添加待办集</button>
        <div v-for="toDoWorkSpace in toDoWorkSpaces">
            <custome-input v-model="toDoWorkSpace.currentValue" :title.sync="title" :table-title="toDoWorkSpace.name" @haha="haha" :key="toDoWorkSpace.id"></custome-input>
        </div>
    </div>
</body>
<script type="text/javascript">
new Vue({
    el: '#app-1',
    data: { inputValue: '我是初始值', toDoWorkSpaces: [], title: '我是旧值', toDoWorkSpaceName: '' },
    methods: {
        myClick: function() {
            // window.alert('我被电击饿了s');
            console.log('我被点击了')
        },
        haha: function(event) {
            window.alert(event)
        },
        addToDoWorkSpace: function() {
            this.toDoWorkSpaces.push({ id: new Date().getTime(), name: window.prompt("请输入代办集的名字："),currentValue:'' });

        }
    }
});
</script>

</html>
```
