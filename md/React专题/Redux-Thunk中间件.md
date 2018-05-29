# Redux-Thunk中间件

## 中间件简单认识

Action 发出以后，Reducer 立即算出 State，叫做同步；Action 发出以后，过一段时间再执行 Reducer，就是异步；中间件是搭建发送action和执行Reducer之间的桥梁。通过使用指定的 middleware，action 创建函数可以返回action对象，也可以返回函数。这时，这个 action 创建函数就成为了thunk。传递两个参数(dispatch,getState)

## 使用方式

* 安装redux-thunk : npm install redux-thunk --save-dev

* 导入thunk： import thunk from 'redux-thunk'

* 导入中间件: import {applyMiddleware} from 'redux'

* 创建store：let store = createStore(applyMiddleware(thunk))

* 激活redux-thunk中间件，只需要在createStore中加入applyMiddleware(thunk)就可以

#### 简单完整代码示例(./App.js)

    import React, { Component } from 'react';
    import { createStore,applyMiddleware } from 'redux';
    import thunk from 'redux-thunk';
    
    //reducer
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
    
    //store
    const store = createStore(reducer,applyMiddleware(thunk));
    
    //action creator
    function addAction(typeText){
        return {
            type:typeText
        }
    }
    
    const clickHandle = (text)=>{
      return (dispatch,getState) =>{
          if(text === 'INCREMENT'){
              dispatch(addAction('INCREMENT'));
          }else {
              let currentState  = getState();
              if (currentState <= 0) return {value:0};
              dispatch(addAction('DECREMENT'))
          }
      }
    };
    
    //点击分发action
    const onIncreaseClick = () =>{
        store.dispatch(clickHandle('INCREMENT'));
    };
    
    //点击分发action
    const onDecrementClick = () =>{
        store.dispatch(clickHandle('DECREMENT'));
    };
    
    class App extends Component {
        constructor(props){
            super(props);
            this.state = {
                value : 0
            }
        }
      render() {
            //获取state
          store.subscribe(
              () =>{
                  this.setState({value:store.getState()})
              }
          );
          return (
              <div>
                  <button type="button" onClick={onIncreaseClick}>+</button>
                  <span>&nbsp;{this.state.value}&nbsp;</span>
                  <button type="button" onClick={onDecrementClick}>-</button>
              </div>
          );
      }
    }
    
    export default App;
