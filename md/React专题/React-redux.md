## React-redux

#### 简单介绍

为了方便使用，Redux 的作者封装了一个React专用的库React-Redux；它的开发思想是基于UI组件和容器组件分离。

#### 安装React-Redux

```
npm install --save react-redux
```

#### UI组件

UI组件只定义外观并不关心数据来源和如何改变，传入什么就渲染什么。UI组件并不依赖于 Redux。

> UI 组件的特征：

* 只负责 UI 的呈现，不带有任何业务逻辑

* 没有状态（即不使用this.state这个变量）

* 所有数据都由参数（this.props）提供

* 不使用任何 Redux 的 API

#### 容器组件

容器组件来是把UI组件连接到 Redux。

> 容器组件的特征

* 负责管理数据和业务逻辑，不负责 UI 的呈现

* 带有内部状态

* 使用 Redux 的 API

#### 概述

UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑，如果一个组件既有 UI 又有业务逻辑的时候，将它拆分成下面的结构：外面是一个容器组件，里面包了一个UI组件。前者负责与外部的通信，将数据传给后者，由后者渲染出视图。

### API

#### Provider store

{Provider store}使组件层级中的 connect() 方法都能够获得 Redux store。

> react


    ReactDOM.render(
      <Provider store={store}>
        <MyRootComponent />
      </Provider>,
      rootEl
    )


> React Router


    ReactDOM.render(
      <Provider store={store}>
        <Router history={history}>
          <Route path="/" component={App}>
            <Route path="foo" component={Foo}/>
            <Route path="bar" component={Bar}/>
          </Route>
        </Router>
      </Provider>,
      document.getElementById('root')
    )


#### connect() 

connect方法是用来连接 React 组件与 Redux store，连接操作不会改变原来的组件类，反而返回一个新的已与 Redux store连接的组件类。

```
connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
```

connect()的参数：

> [mapStateToProps(state, [ownProps]): stateProps]

批注：读取state

mapStateToProps()是一个函数，用于外部的数据转换为UI组件的参数(props)。执行后应该返回一个对象，建立组件跟 store 的state 的映射关系。
每当store 的state更新的时候，就会自动执行mapStateToProps。

ownProps代表组件本身的props，如果写了第二个参数ownProps，那么当prop发生变化的时候，mapStateToProps也会被调用。

当通过null或undefined取代mapStateToProps，UI组件就不会订阅Store，也就是说Store的更新不会引起UI组件的更新。

```
function mapStateToProps(state){
    if (state < 0) return {value:0};
    return {value : state};
}
```

代码分析：mapStateToProps是一个函数，它接受state作为参数，返回一个对象。这个对象有一个value属性，代表UI组件的同名参数，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。

* 传入了props时:

```
const mapStateToProps = (state, ownProps) => {
  return {
    active: ownProps.filter === state.visibilityFilter
  }
}
```

> [mapDispatchToProps(dispatch,[ownProps]:dispatchProps]

批注：分发action

mapDispatchToProps用于建立UI 组件的参数跟store.dispatch的映射关系。定义了哪些用户的操作应该当作 Action，传给Store；mapDispatchToProps可以是一个object，也作为函数。

* mapDispatchToProps是函数时,会得到dispatch和ownProps（容器组件的props对象）两个参数。

```
const mapDispatchToProps = (dispatch,ownProps) =>{
        return {
            onIncreaseClick : ()=>dispatch(addAction('INCREMENT')),
            onDecrementClick : ()=>dispatch(addAction('DECREMENT'))
        }
    }
```

* mapDispatchToProps是对象时
 
```
const mapDispatchToProps = {
        onIncreaseClick : ()=>addAction('INCREMENT'),
        onDecrementClick : ()=>addAction('DECREMENT')
    };
```

代码分析：mapDispatchToProps对象里每个键名应该对应UI组件的同名参数；键值如果是一个函数，会被当Action creator，返回的Action会由Redux自动发出。


#### 简单完整代码示例

    import {createStore} from 'redux';
    import {connect,Provider} from 'react-redux';
    //展示型组件 Counter
    class Counter extends React.Component{
        render(){
            const {value,onIncreaseClick,onDecrementClick} = this.props;
            return (
                <div>
                    <button type="button" onClick={onIncreaseClick}>+</button>
                    <span>&nbsp;{value}&nbsp;</span>
                    <button type="button" onClick={onDecrementClick}>-</button>
                </div>
            )
        }
    }
    // Reducer
    function reducer(state = 0,action){
        let {type} = action;
        switch(type){
            case 'INCREMENT':
                return state + 1;
            case 'DECREMENT':
                return state - 1;
            default :
                    return state;
        }
    }
    //store creator
    const store = createStore(reducer);
    
    //Action creator
    function addAction(typeText){
        return {
            type:typeText
        }
    }
    //拿取state
    function mapStateToProps(state){
        if (state < 0) return {value:0};
        return {value : state};
    }
    //分发action
    const mapDispatchToProps = (dispatch,ownProps) =>{
        return {
            onIncreaseClick : ()=>dispatch(addAction('INCREMENT')),
            onDecrementClick : ()=>dispatch(addAction('DECREMENT'))
        }
    }
    
    /*
    //mapDispatchToProps是对象
    const mapDispatchToProps = {
        onIncreaseClick : ()=>addAction('INCREMENT'),
        onDecrementClick : ()=>addAction('DECREMENT')
    };
    */
    
    //使用connect()
    const App = connect(
        mapStateToProps,
        mapDispatchToProps
    )(Counter);
    
    
    ReactDOM.render(
        <Provider store = {store}>
            <App/>
        </Provider>,
        document.getElementById('root')
    );





