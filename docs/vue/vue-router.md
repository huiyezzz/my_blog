<!--
 * @Author: xx
 * @Date: 2021-06-22 17:05:21
 * @LastEditors: 青峰
 * @LastEditTime: 2021-06-22 17:18:48
 * @FilePath: /vue-press/docs/vue/vue-router.md
-->

# VUE-ROUTER

## 简单使用

1, 安装路由插件

```bash
npm install vue-router --save
```

2, 导入以及注册路由插件

```js
import VueRouter from 'vue-router';
Vue.use(VueResource); // 全局注册
```

3, 定义路由

```js
// 定义路由
const routes = [
    {path: '/demo', component: Demo}
];
```

4, 创建router  实例

```js
const router = new VueRouter(
    {routes}
);
```

5, 注入路由

```js
new Vue({
    router,
    render: h => h(App),
}).$mount('#app');
```

6, 配置路由导航

```html
<router-link to="/to-do-list">待办Demo</router-link>
```

## 动态路由

1, 路由配置

```js
 routes: [
    // 动态路径参数 以冒号开头
     path: '/dynamic-demo/:id', component: DynamicDemo, name: 'dynamicDemo',
  ]
 ```

 2, 路由导航  

 ```html
  <router-link :to="{name:'dynamicDemo',params:{id: new Date().getSeconds()}}">动态路由Demo</router-link>
  ```
  
`:to`可以是字符串也可是对象

## 传参

1, 路由参数

 ```html
  <router-link :to="{name:'dynamicDemo',params:{id: new Date().getSeconds()}}">动态路由Demo</router-link>
  
```
  
⚠️：当存在‘path’时，params会被忽略，可以在路由配置中给路有一个‘name’使用‘name’+'params'

2, 查询参数

```html
<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}"
  >Register</router-link>
```

## 路由视图

> 同级展示路由

1, 定义路由导航

```html
<router-view></router-view>
<router-view name="a"></router-view>
<router-view name="b"></router-view>
```

2, 配置路由

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Demo,
        a: DemoA,
        b: DemoB
      }
    }
  ]
})
```

## 路由重定向和别名

重定向

```js
  { path: '/a', redirect: { path: '/b' }}
  
  ```
  
 ```js
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
```

别名

```js
 { path: '/a', component: A, alias: '/b' }
```

## 路由守卫

### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。

### 全局路由守卫

1, 全局前置守卫

```js
router.beforeEach((to, from, next) => {
  // ...
})
```

2, 全局解析守卫
> 在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用

```js
 router.beforeResolve
```

3,全局后置钩子
 > 不会接受 next 函数也不会改变导航本身

```js
router.afterEach((to, from) => {
  // ...
})

```

### 组件内守卫

```js
 beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
    beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
  ```
  
### 路有元信息
  
#### demo

### 滚动行为

```js
scrollBehavior (to, from, savedPosition) {
  if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}
```

### 路由懒加载

```js
const Foo = () => import('./Foo.vue')
```

 或

```js
const Foo = () => Promise.resolve({ /* 组件定义对象 */ })

```

## 业务场景

### 路由组件传参

> 用于解耦，组建中不用依赖于具体的路由配置

1, 路由配置

```js
 {
        name: "routerPass",
        path: '/router-pass-demo/:id',
        component: DynamicDemo,
        // props: true, // 布尔模式,
       //  props:{test:'test',id:'999'} // 对象
        props:(route) => ({test:route.query.test,id:route.params.id}) // 函数
    },
```

2, 组件中定义

```js
 props: ['id', 'test']
```

### 页面登陆校验(路由元)

  ```js
  router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```
