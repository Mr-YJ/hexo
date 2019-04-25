---
title: vuex
date: 2019-04-25 14:44:21
tags:
---
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式

## 安装使用
```bash
npm install vuex --save

import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);
```

## 核心概念
* Vuex 使用单一状态树,个应用将仅仅包含一个 store 实例,store基本上就是一个容器，它包含着应用中大部分的状态 (state)
* 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数；一条重要的原则就是要记住 mutation 必须是同步函数
* Action 提交的是 mutation，而不是直接变更状态;Action 可以包含任意异步操作;Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters

<!--more--> 

```bash
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    countMutation(state, value) {
      state.count = value;
    }
  },
  actions: {
    countAction(context, value) {
      context.commit('countMutation', value);
    }
  }
})
```

## 组件中的使用
```bash
import { mapState, mapGetters, mapActions } from "vuex";

computed: {
  // 映射 this.count 为 store.state.count
  ...mapState([
    "count"
  ])
},
methods: {
  // 将this.countAction() 映射为this.$store.dispatch('countAction')
  ...mapActions([
    "countAction"
  ]),
}

// 改变count的值
this.countAction(1)

```
如果直接去改变count
```bash
this.count = 1
```
vue会有警告，但是count也是成功改变的，但是这不利于vue的调试工具devtools-extension跟踪每一次的状态变化，所以建议不要直接修改state，最好通过提交mutation来操作

## getter
有时候我们需要从 store 中的 state 中派生出一些状态，例如对count有额外的操作
```bash
const store = new Vuex.Store({
  state: {
    count: 0,
    number: 0
  },
  getters: {
    // 希望在每次获取count的时候都加一
    doCount: state => {
      return state.count++
    }

    /* getter也可以接受其他getter作为第二个参数
      doNumber: (state, getters) => {
        return state.number + getters.doCount
      }
    */
  }
})
```
组件中使用getter
```bash
computed: {
  // 把 this.doCount 映射为this.$store.getters.doCount
  // 把 this.doNumber 映射为this.$store.getters.doNumber
  ...mapGetters([
    'doCount',
    'doNumber'
  ])
}
```