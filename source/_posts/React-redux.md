---
title: React-redux 笔记
date: 2017-07-10 15:26:17
tags: [React, 笔记]
keywords: [React-reaux笔记,阮一峰]
categories: [技术笔记]
comments: false
---

# 未完待续

## 简介

<p id="border-blue">如果你的应用有以下场景，可以考虑使用 Redux。</p>

 1. 某个组件的状态 `需要共享`
 1. 某个状态需要在 `任何地方` 都可以获取
 1. 一个组件想要 `改变全局状态`
 1. 一个组件想要 `改变另一个组件状态`

<!-- more -->

## 核心思想

<p id="border-green"> web 应用是一个 `状态机` ，视图与状态是 `相对应` 的,并且 `所有的状态都保存在一个对象里`。</p>


## 基本概念和 API

### Store

 - Store 就是保存数据的地方，你可以把它看成一个容器。
 - `整个应用只能有一个 Store`。

```javascript
import { createStore } from 'redux';
const store = createStore(reducer);

/* createStore函数接受另一个函数作为参数，返回新生成的 Store 对象。 */
```


### State

 - `Store对象包含所有数据`。
 - `store.getState()` 可获得当前 State

```javascript
import { createStore } from 'redux';
const store = createStore(reducer);

const state = store.getState();

/*
 * Redux 规定， 一个 State 对应一个 View。
 * 只要 State 相同，View 就相同。
 * 你知道 State，就知道 View 是什么样，反之亦然。
 */
```

### Action

 - Action 只是一个对象
 - Action 可改变 State, 其变化会同步到 View
 - `type属性必须` ，表示 Action 的名称,其他属性随意

```javascript
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```

### Action Creator

View 要发送多少种消息，就会有多少种 Action。
如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 Action Creator。

```javascript
const ADD_TODO = '添加 TODO';

function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

const action = addTodo('Learn Redux');

/* addTodo函数就是一个 Action Creator。 */
```

### store.dispatch()

`store.dispatch()是 View 发出 Action 的唯一方法。`

```javascript
import { createStore } from 'redux';
const store = createStore(reducer);

/* store.dispatch接受一个 Action 对象作为参数，将它发送出去。*/
store.dispatch({
  type: 'ADD_TODO',
  payload: 'Learn Redux'
});

/* 结合 Action Creator，这段代码可以改写如下。 */
store.dispatch(addTodo('Learn Redux'));
```


### Reducer

 - Reducer 是一个函数
 - 接受 `当前State` 和 `Action` 作为参数并返回新的 `State`

```javascript
const chatReducer = (state = defaultState, action = {}) => {
  const { type, payload } = action;
  switch (type) {
    case ADD_CHAT:
      return Object.assign({}, state, {
        chatLog: state.chatLog.concat(payload)
      });
    case CHANGE_STATUS:
      return Object.assign({}, state, {
        statusMessage: payload
      });
    case CHANGE_USERNAME:
      return Object.assign({}, state, {
        userName: payload
      });
    default: return state;
  }
};
```

### store.subscribe()

<p id="border-blue"> `store.subscribe` 方法是用来设置监听函数，一旦 State 发生变化，就自动执行这个函数。</p>

```javascript
import { createStore } from 'redux';
const store = createStore(reducer);

/**
 * 把 View 的更新函数（对于 React 项目，就是组件的render方法或setState方法）放入listen，就会实现 View 的自动渲染。
 */

let unsubscribe = store.subscribe(listener);

/* store.subscribe 方法返回一个函数，调用该函数就可以 解除监听 。 */
unsubscribe();
```

### 流程步骤

 1. 首先需要在 `reducer 函数内`,定义 type 值和默认值对应的操作
 2. 把 reducer 函数放到 storeCreate(reducer) `进行初始化` ,初始化时 返回的是 reducer 函数内的默认值
 3. 通过 store.dispatch({type:'xxx',value:123}) 来发送 Action 数据格式,以此来触发 reducer 函数,并更新 state 当前值
 4. 在通过 `store.subscribe` 检测 State 的变化,以此来更新 View
