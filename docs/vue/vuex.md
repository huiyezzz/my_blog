<!--
 * @Author: xx
 * @Date: 2021-06-22 16:55:27
 * @LastEditors: 青峰
 * @LastEditTime: 2021-06-22 17:03:29
 * @FilePath: /vue-press/docs/vue/vuex.md
-->

# VUEX

## 简介

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化

示意图

![image](https://vuex.vuejs.org/vuex.png)

## 核心概念

- State -数据
- Getter -按需要获取数据
- Mutation -对数据进行操作，同步
- Action -对mutation操作，可异步
- Module -模块划分

---

### 1. State

定义

```js
const state = {
    toDoList: storage.get('toDoList')
};
```

使用

```js
1. 直接取

  count () {
      return this.$store.state.count
    }
2. 通过辅助函数取

   computed: {
            ...mapState('todo', ['toDoList'])
            
             <!--...mapState('todo', {'myToDoList':'toDoList'})-->
        },
```

### 2. Getters

> getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的,通过方法访问时不会缓存

定义

```js
const getters = {
    getDoingList: (state) => {
        return state.toDoList.filter(item => !item.checked);
    },

<!--也可以返回一个带参数的函数来实现传参-->

  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}

```

使用

```js
 computed: {
            ...mapGetters('todo', {doingList: 'getDoingList', doneList: 'getDoneList'})
        }
        
```

### 3. Mutation

> 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

定义

```js
const mutations = {
    editTodo(state, {todo, checked = todo.checked}) {
        const index = state.toDoList.indexOf(todo);
        state.toDoList.splice(index, 1, {...todo, checked}); // 需遵守 Vue 的响应规则发生异变才行
    }
};
```

使用

```js
store.commit('editTodo', {省略})
```

### 4.Action

> Action 提交的是 mutation，而不是直接变更状态。
Action 可以包含任意异步操作。

定义

```js
const actions = {
    // 编辑待办
    editTodo({commit}, todo) {
        commit('editTodo', {todo, checked: !todo.checked});
    }
};
```

```js
异步
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

使用

```js
1.
store.dispatch('increment')

2.
  methods: {
            ...mapActions('todo', ['removeTodo', 'editTodo'])
        },
        
```

#### 4.2 组合Action

> store.dispatch 可以处理被触发的 action 的处理函数返回的 Promise，并且 store.dispatch 仍旧返回 Promise

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

```js
store.dispatch('actionA').then(() => {
  // ...
})
```

## 模块化

### 示例代码

目录结构

```js
stroe/[modules,index.js]
```

modules/todo.js

```js
import storage from "../../utils/storage";

const state = {
    toDoList: storage.get('toDoList')
};

const actions = {
    // 添加待办
    addTodo({commit}, todo) {
        commit('addTodo', todo);
    },
    // 移除待办
    removeTodo({commit}, id) {
        commit('removeTodo', id);
    },
    // 编辑待办
    editTodo({commit}, todo) {
        commit('editTodo', {todo, checked: !todo.checked});
    }
};

const mutations = {
    addTodo(state, todo) {
        state.toDoList.push(todo);
        storage.set('toDoList', state.toDoList)
    },
    removeTodo(state, id) {
        state.toDoList = [...state.toDoList.filter(item => item.id !== id)];
        storage.set('toDoList', state.toDoList)
    },
    editTodo(state, {todo, checked = todo.checked}) {
        const index = state.toDoList.indexOf(todo);
        state.toDoList.splice(index, 1, {...todo, checked}); // 异变
        storage.set('toDoList', state.toDoList)
    }
};

const getters = {
    getDoingList: (state) => {
        return state.toDoList.filter(item => !item.checked);
    },
    getDoneList: (state) => {
        return state.toDoList.filter(item => item.checked);
    }

}
export default {
    namespaced: true,
    state,
    mutations,
    actions,
    getters
}

```

index.js

```js
import todo from './modules/todo'
import Vuex from 'vuex'
import Vue from "vue";
Vue.use(Vuex);
export default new Vuex.Store({
    modules: {
        todo
    }
});
```
