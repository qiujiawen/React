# Redux基础知识

## 配置Redux

>快速构建 React 开发环境
 
``
create-react-app
``

>安装Redux

``
npm install --save redux react-redux
``


## Redux简单介绍

 React有props和state: props意味着父级分发下来的属性，state意味着组件内部可以自行管理的状态，并且整个React没有数据向上回溯的能力，也就是说数据只能单向向下分发(props)，或者自行内部消化(state)。
 如何让react构建的两个组件互相交流，使用对方的数据？解决的唯一办法就是提升state，将state放到共有的父组件中来管理，再作为props分发回子组件。因此为了更好的管理state，就需要一个库来作为更专业的顶层state分发给所有React应用，也就是把所有state集中放到所有组件顶层，然后分发给所有组件，这个库就是Redux。
 
 ## 三个基本原则
 
 >单一数据源
 
 整个应用的 state 被储存在唯一一个 store 中。
 
 >State 是只读的
 
 唯一改变 state 的方法就是触发 action,因此视图和网络请求都不能直接修改 state,它们只能表达想要修改的意图。
 
> 使用纯函数来执行修改(Reducer)

Reducer是一个纯函数，它接收先前的 state 和 action，并返回新的 state。Reducer只是为了描述 action 如何改变 state。

## 理解 Redux 的核心概念

#### Redux的核心由三部分组成：Action, Reducer, Store

#### Action

* Actions 只是描述了有事情发生
* Action是应用和 store 互相通信的载体，它是store数据的唯一来源。

* 通过 store.dispatch() 将 action 传到 store。

* Action 本质上是普通对象,必须包含type这个属性;多数情况下，type 会被定义成字符串常量,来表示将要执行的动作。

> Action的创建

    const action = {
              type: 'ADD_TODO',
              payload: 'Learn Redux'
            };

> 封装创建Action的函数
    
    function addTodo(text) {
      return {
        type: ADD_TODO,
        text
      }
    }
    
#### Reducer

* Reducers 描述了状态的变化以及如何响应 actions 并发送到 store 。即应用如何更新 state

* Reducers最重要的特征是，它是一个纯函数，接受Action和当前State 作为参数，返回一个新的 State。

> 使用reducers需要注意的地方

* 不要修改传入参数；

* 不要执行有副作用的操作，如 API 请求和路由跳转；

* 不要调用非纯函数，如 Date.now() 或 Math.random()。

> reducers代码示例

```
const counter = (state = 0, action) => {
  switch (action.type) {
      case 'INCREMENT':
        return state + 1;
      case 'DECREMENT':
        return state - 1;
      default:
        return state;
  }
}
```

> reducers的拆分 combineReducers(reducers)

代码清单：reducer/todos.js

```
export default function todos(state = [], action) {
      return state;
    }
```

代码清单：reducer/counter.js

```
export default function counter(state = [], action) {
      return state;
    }
```    

代码清单：reducers/index.js

```
import { combineReducers } from 'redux'
import todos from './todos'
import counter from './counter'
export default combineReducers({
  todos,
  counter
})
```

> 设计 State 结构

尽可能地把 state 范式化，不要嵌套。把state想像成数据库，所有数据放到一个对象里，每个数据以ID为主键，不同实体或列表间通过ID相互引用数据。


#### Store

store是保存数据的地方，可以把它看成一个容器；store是用来维持应用所有的 state 树 的一个对象。 改变 store 内state的惟一途径是对它 dispatch 一个 action。

> store的创建 {createStore()}

```
import { createStore } from 'redux'
import reducers from './reducers'
let store = createStore(reducers)
```

createStore() 的第二个参数是可选的, 用于设置 state 初始状态.

> store的三个方法

* 提供 getState() 方法获取 state；

* 提供 dispatch(action) 方法更新 state；

* 通过 subscribe(listener) 注册监听器;

> getState()

store对象包含所有数据。如果想得到某个时点的数据，就要对 Store 生成快照。这种时点的数据集合，就叫做 State。State的获取是通过store.getState()拿到。

redux规定，一个State对应一个View。只要State相同，View 就相同。你知道State，就知道View 是什么样，反之亦然。

```
import { createStore } from 'redux'
import reducers from './reducers'
let store = createStore(reducers);
const state = store.getState();
```

> dispatch(action)

dispatch 分发 action。这是触发 state 变化的惟一途径.

    import { createStore } from 'redux'
    
    // reducer
    const todos = (state = [''], action) => {
      switch (action.type) {
        case 'ADD_TODO':
          console.log(Object.assign([], state, [action.text]))
          return Object.assign([], state, [action.text]);
        default:
          return state;
      }
    }
    
    //store creator
    let store = createStore(todos, [ 'Use Redux' ])
    
    // action creator
    function addTodo(text) {
      return {
        type: 'ADD_TODO',
        text
      }
    }
    
    // dispatch
    store.dispatch(addTodo('Read the docs'))
    store.dispatch(addTodo('Read about the middleware'))

> subscribe(listener)

subscribe(listener)返回一个函数可以用来注销监听器。

subscribe(listener)是监听State变化的函数，一旦 State 发生变化，就自动执行这个函数。只要把 View 的更新函数（对于 React 项目，就是组件的render方法或setState方法）放入listen，就会实现 View的自动渲染。

* 注销监听器

```
// 注意 subscribe() 返回一个函数用来注销监听器
const unsubscribe = store.subscribe(() =>
  console.log(store.getState())
)
// 停止监听 state 更新
unsubscribe();
```


#### 工作流程：

* 首先，用户发出 Action。(store.dispatch(action);)

* 然后，Store 自动调用 Reducer，并且传入两个参数：当前State和收到的Action。Reducer会返回新的State。 State一旦有变化。Store就会调用监听函数.(store.subscribe(listener);)

