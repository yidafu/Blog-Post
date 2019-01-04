---
title: React Redux & Redux Sage 使用记录
date: '2018-12-29'
tags:
  - react
  - React-Redux
  - Redux-Saga
category: frontend
---

# 介绍

这篇文章主要介绍 React 全家桶中状态管理`redux`及副作用解决方案的使用方法.

# 使用

### 起步

首先先使用`react-rect-app`创建一个空的项目,

```shell
creat-react-app redux-demo
```

添加依赖

```shell
yarn add redux react-redux redux-saga
```

>需要添加`redux`作为基本库,因为`react-redux`并不包含`redux`

### 核心概念

+ state: 用来描述应用的普通对象
+ action: Action 就是一个普通 JavaScript 对象用来描述发生了什么
+ reducer: reducer 是一个纯函数,只是一个接收 state 和 action，并返回新的 state 的函数

### 实际使用

目录结构:

```
redux-demo
├── package.json
├── src
|   ├── App.js
|   ├── index.js
│   ├── components
│   |   └── Counter.js
│   └── redux
│       ├── index.js
│       ├── action
│       │    └── index.js
│       ├── constants
│       │    └── index.js
│       ├── reducers
│       │    ├── index.js
│       │    └── count.js
│       ├── sages
│       │    ├── index.js
│       │    └── count.js
├── config
├── script
└── test
```

## 引入redux

```js
import React from 'react'
import ReactDOM from 'react-dom'
import {Provider} from 'react-redux'

import store from './redux'
import App from './App'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root'))
```

## action

一般的`action`定义为

```js
export function plus(payload) {
  return {
    type: 'PLUS',
    payload
  }
}
```

`type`指明了触发的`reducer`.

```js
export default function( state = initialState, action ) {
  switch(action.type) {
    case 'PLUS': { // <<=== 这里就是 'PLUS' 对应 state 变化
      return {
        count: ++state.count,
      }
    }
    // ...
  }
}
```

但是为了解耦, type一般会定义常量: `const PLUS = 'PLUS'`,统一放在`constants/index.js`里面.

```js
import {PLUS} from '@/redux/contants'

export function plus(payload) {
  return {
    type: PLUS,
    payload
  }
}

export default function( state = initialState, action ) {
  switch(action.type) {
    case PLUS: {
      return {
        count: ++state.count,
      }
    }
    // ...
  }
}
```

## reducers

`reducer`函数必须返回一个新的的`state`. 

一般`state`都是有个结构比较复杂的对象,这里就会涉及对象的**浅拷贝和深拷贝**,属性的覆盖.

可以使用`Object.assin()`, rest 对象展开符.

```js
initialState = {
  list: [{a:1}, {b:2},{c:3}]
}
export default function( state = initialState, action ) {
  switch(action.type) {
    case MAEGE_ARRAY: {
      return {
        list: [...state.list, ...action.payload]
      }
    }
    // ...
  }
}
```

### 多个reducer

当应用比较复杂的时候,往往会见`reducer` 拆分成多个文件.

```js
import {combineReducers} from 'redux'
import cart from './cart'
import login from './login'

export default combineReducers({
  cart,
  login,
})
```

## saga

`react-redux`本身是不能做异步请求.`redux-saga`就是一个比较好的`redux`异步请求解决方案.

引入`redux-saga`

```js
import {createStore, applyMiddleware} from 'redux'
import createSagaMiddleware from 'redux-saga'
import reducers from './reducers'
import sagas from './sagas'

const sagaMiddleware = createSagaMiddleware()
const store = createStore(
  reducers,
  applyMiddleware(sagaMiddleware)
)

sagaMiddleware.run(sagas)

export default store
```

新建`sagas/`目录.

同`reducer`也将`saga`拆分.

```js
import {fork, all} from 'redux-saga/effects'

import { watchAddToCart} from './cart'
import {watchLogin} from './login'

export default function* root() {
  yield all([
    fork(watchAddToCart),
    fork(watchLogin),
  ])
}
```

`redux-saga`示例:

```js
export function* login(action) {
  try{
    const loginState = yield call(userLogin, action.payload)
    if(loginState.data.log_state == 0){
      yield put(actions.loginFailed({loginState: 'failed'}))
    } else {
      yield put(actions.loginSuccess(loginState.data))
    }
  } catch(err) {
    yield put(actions.loginFailed({loginState: 'failed'}))
  }
}

export function* watchLogin() {
  yield takeLatest(types.LOGIN_ACTION, login)
}
```

