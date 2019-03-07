---
title: 由浅到深思考基于 Redux 的状态管理
date: 2019-03-07 14:35:32
tags:
categories: 编程 # 分类
thumbnail: https://images.unsplash.com/photo-1551250930-ace1ad395cea?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=400&q=60
---

# 一、为什么要使用Redux?
这是老生常谈的问题了，几乎每一个写Redux的文章里都会提到这个问题。在阮一峰老师的文章中提到过，“如果你不知道是否需要 Redux，那就是不需要它。”如何理解这句话呢？我们从概念的角度和实际项目中可能出现问题的角度来解答。

## 使用场景
 简单说，如果你的UI层非常简单，没有很多互动，Redux 就是不必要的。只有符合了下面的几种情况，才是Redux的使用场景：

> 1.用户的使用方式复杂

> 2.不同身份的用户有不同的使用方式（比如普通用户和管
 理员）
 
> 3.多个用户之间可以写作

> 4.与服务器大量交互，或者使用了WebSocket

## 联系实际

1.假设产品经理提出了这样一个需求：

***写一个简单的Logger来实现用户的动作跟踪记录。***

我们可能会这么写：

在MVVM框架中（以纯Vue举例）：
```js
methods: {
  handleLogin () {
    console.log('[Logger] 用户登录')
    ...
  },
  handleLogout () {
    console.log('[Logger] 用户退出登录')
    ...
  }
}
```


2.接着产品经理说：“***在上述需求的基础上，记录用户的操作时间***”。

此时，我们会在每个输出中添加一个new Date():
```js
methods: {
  handleLogin () {
    console.log('[Logger] 用户登录', new Date())
    ...
  },
  handleLogout () {
    console.log('[Logger] 用户退出登录', new Date())
    ...
  }
}
```
3.需求3：***把控制台中有关Logger的输出全部去掉***

于是前端们又要一个个地把Logger注释掉。。。

4.需求4：***正式上线后，自动收集bug，并还原当时的场景***

到了要完全还原当时的使用场景时，前端开始手无足措了。因为您不知道这个报错，用户是怎么一步一步操作得来的
就算知道用户是如何操作得来的，但在您的电脑上，测试永远都是通过的

## ※ 如何解决撤销的需求
使用Redux，将应用中 ***所有的动作与状态都统一管理***，让一切都有据可循。

这样一来，便可以在开发调试的过程中 ***撤销与重做*** 了。并且可以完整的记录下每个动作。随时随地能够恢复到之前的状态。

# 二、Store
## 2.1 Store是什么？
state是应用的状态。

***store是state的管理者***。

> 二者的关系是：state = store.getState()

```js
{
  //这是一个应用初始state
  counter: 0,
  todos: []
}
```

## 2.2 生成一个store
Redux规定，一个应用只应有一个单一的 store，其管理着唯一的应用状态 state。

想生成一个store，需要调用Redux的createStore.
```js
import {createStore} from 'redux'
...
const store = createStore(reducer,initialState)//store是靠reducer生成的
```
在上面代码中，createStore接收两个参数，返回新生成的 Store 对象：

reducer(***reducer是一个函数，负责更新并返回一个新的state***)和

initialState(用于前后端同构的数据同步)

## 2.3 通过store更改状态state

Redux对Store的规定
- 不能直接修改应用的状态 state
不能直接修改应用的状态state，也就是说，下面的行为是不允许的：
```js
//禁止使用下面的方法
var state = store.getState()
state.counter = state.counter + 1 
```
那么应当如何修改state呢？
必须使用 ***dispatch(action)函数 来改变state***
## 2.4 Store的四个函数
 
```js
getState()//获取整个 state
```


```js
dispatch(action)//触发 state 改变的【唯一途径】
```

```js
subscribe(listener) //可以理解成是 DOM 中的 addEventListener
```

```js
replaceReducer(nextReducer) // 一般在 Webpack Code-Splitting 按需加载的时候用
```
那么此时，我们已经知道了 ***改变state方式***，我们自然想知道，要将什么 ***内容传给state***。

带着这个疑问，引入了Action这个概念。

# 三、Action
## 3.1 Action是什么？
Action的本质是一个包含 **type** 属性的 **对象**。

type: 描述用户行为，是我们追踪用户行为的关键。

举个例子，如果想要 ***增加一个待办事项：***
```js
{
//代码1
    type:'ADD_TODO',
    payload:{
        id: 1,
        content: '待办事项1',
        completed: false 
    }
}
```

## 3.2 生成Action

 
 ```js
 //生成一个“新增一个待办事项”的action
 let id=1
 function addTodo(content){
    return{
        type:'ADD_TODO',
        payload:{
            id: 1,
            content: '待办事项1',
            completed: false 
        }
    } 
 }
 ```
上面的函数就是 **Action Creator**。它的本质是创造action的一个**函数**，返回值是一个action(对象)。
 
## 3.3 将对象action的内容传给state
结合3.2的代码，此时有一个表单如下：
```js
<input type="text" id="todoInput" />
<button id="btn">提交</button>
```
当在输入框输入“代办事项2”后，点击一下提交按钮，我们的state状态需要改变成：
```js
{
  counter: 0,
  todos: [{
    id: 1,
    content: '待办事项1',
    completed: false
  }, {
    id: 2,
    content: '待办事项2',
    completed: false
  }]
}
```
该如何实现将表单数据传入到state中呢？
（假设 store 为全局变量，并引入了 jQuery ）

于是可以这样写：
```js
$('#btn').on('click',function(){
    let content = $('#todoInput').val()
    let action = addTodo(content) //执行Action Creator获得action
    store.dispatch(action) //改变state的唯一方法。
}
```
state状态就能有“新增事项2”了。

但是这里有个问题：

为什么在 **store.dispatch(action)** 之后， Redux会明确知道是提取 **action.payload**，并写到对应的state.todos数组中，而不是直接将整个action对象传进去呢？

带着这个疑问，我们引入了 **Reducer**概念


# 四、Reducer
## 4.1 Reducer是什么？
在上面例子中，store.dispatch(action)后，就会触发 **reducer**执行，reducer的实质是一个同步的 **纯函数**。
> 纯函数：https://en.wikipedia.org/wiki/Pure_function

它根据 **action.type** 来更新state并返回 **nextState**，最后将返回值 **完全覆盖**原来的 **state**，更新state。

## 4.2 Reducer分析action,更新state

在上面待办事项的例子中，Reducer大致如下：

```js
const initState={
    counter:0,
    todos:[]
}

function reducer(state,action){
    state = state || initState;
    
    switch(action.type){
        case 'ADD_TODO':
        var nextState = _.cloneDeep(state) //lodash的深克隆
        //将action.payload放进nextState.todos里
         nextState.todos.push(action.payload)
    }
    
    default:
    //若无修改，必须返回原state，否则就undefined
    return state
}
```

# 五、Redux总结
思想顺序:

**Action Creator => action => store.dispatch(action) => reducer(state, action) => ~~原 state~~ state = nextState**

一个Redux文档的Todo示例：https://www.redux.org.cn/docs/basics/ExampleTodoList.html

# Redux框架应当解决的问题？


Shawn McKay写的《重新思考 Redux》中提到了这样一个公式：
> 工具质量 = 工具节省的时间/使用工具消耗的时间

而有时候我们会发现，***使用原生Redux可能会降低工作效率***。

虽然Redux的数据管理思想是正确的，但是 ***为了更有效率的使用redux，我们需要使用基于redux的框架。***



## ※ 从6个角度看Redux框架需要解决什么问题

### 简化初始化

### 简化 Reducers

### 支持 async/await

### 将 action + reducer 改为两种 action

### 不再显示申明 action type

### Reducer 直接作为 ActionCreator

精读《重新思考 Redux》：https://juejin.im/post/5af8dff9f265da0b9f40616b#heading-2


# 参考文章

1. [黄子毅: 精读《重新思考 Redux》](https://juejin.im/post/5af8dff9f265da0b9f40616b)

2. [kenberkeley: 《Redux 简明教程》](https://github.com/kenberkeley/redux-simple-tutorial)

3. [阮一峰：《React 入门实例教程》](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_one_basic_usages.html)
4. [前端新能源:《 给2019前端的5个建议》](https://juejin.im/post/5c617c576fb9a049e93d33a4) 

5. [kenberkeley：《Redux 进阶教程》](https://github.com/kenberkeley/redux-simple-tutorial/blob/master/redux-advanced-tutorial.md)

