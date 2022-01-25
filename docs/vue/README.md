<!--
 * @Author: xx
 * @Date: 2021-06-22 16:51:49
 * @LastEditors: 青峰
 * @LastEditTime: 2021-06-24 19:54:35
 * @FilePath: /vue-press/docs/vue/README.md
-->

# VUE

[官方文档 yyds](https://cn.vuejs.org/)

## Api

### nextTick

执行过程：

1. dom修改后dom tree实时修改
2. vue异步更新dom，下一个task
3. Promise、MutationObserver以及setTimeout三种方式尝试推入到任务队列
4. 一次task完成后才会ui render

```nextTick``` 并不是把任务插入到任务队列最尾部。下面代码```tiemout```打印的是最新值(应为是下一个task),```nextTick```还是之前值(后面值改变的任务插入在了nexTick回调之后)，vue的dom更新也是异步。

```js
 this.$nextTick(() => console.log(wordTemp.innerHTML));
    setTimeout(() => {
       console.log(wordTemp.innerHTML);
    }, 100);
    this.wordTest = "changed";
```
