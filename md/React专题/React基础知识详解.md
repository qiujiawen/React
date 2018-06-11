# React基础知识详解
React 是一个 JavaScript 库。
## 1、构建React环境
#### 使用 create-react-app 快速构建 React 开发环境
运行create-react-app命令前，你需要安装node。
     
windows系统下：
```
npm install -g create-react-app
```
Liunx和Mac电脑下：
```
sudo npm install -g create-react-app
```

创建React目录
```
create-react-app my-app
```
进入目录，然后就启动
```
cd my-app
npm start
```

Mac终端命令下创建：

```
sudo npm install -g create-react-app
create-react-app my-app
open -a WebStorm my-app

//Mac命令行指定特定webstorm程序打开文件
//#参数说明：－a指定应用
open -a TextEdit settings.xml

//#参数说明：－e使用文本编辑器打开
open -e settings.xml

//#参数说明：－t使用默认编辑器打开
open -t settings.xml
```

## 2、JSX 语法
JSX的语法：HTML 语言直接写在 JavaScript 语言之中，不加任何引号，它允许 HTML 与 JavaScript 的混写。

    ReactDOM.render(
        <p>Hello, world!</p>,
        document.getElementById('example')
    );

#### 点表示法
可以使用 JSX 中的点表示法来引用 React 组件。从而可以方便地从一个模块中导出许多 React 组件。

    import React from 'react';    
    const MyComponents = {
      DatePicker: function DatePicker(props) {
        return <div>Imagine a {props.color} datepicker here.</div>;
      }
    }    
    function BlueDatePicker() {
      return <MyComponents.DatePicker color="blue" />;
    }

#### 首字母大写
>当元素类型以小写字母开头时，它表示一个内置的组件，如div或span

```
let myDiv = <div className="foo" />;
ReactDOM.render(myDiv, document.getElementById('example'));
```

>渲染 React 组件，需要创建一个大写字母开头的变量

```
class MyComponent extends React.Component{...};
ReactDOM.render(MyComponent, document.getElementById('example'));
```

#### 属性
在 JSX 中有几种不同的方式来指定属性。
>使用 JavaScript 表达式

可以传递任何 {} 包裹的 JavaScript 表达式作为一个属性值

```
<MyComponent foo={1 + 2 + 3 + 4} />
```

注意：JSX 中不能使用 if else 语句，但可以使用三元运算 表达式来替代

>字符串常量

可以将字符串常量作为属性值传递。

```
<MyComponent message="hello world" />
<MyComponent message={'hello world'} />
```

>扩展属性

如果已经有了个 props 对象，并且想在 JSX 中传递它，你可以使用 ... 作为扩展操作符来传递整个属性对象。下面两个组件是等效的：

    function App1() {
      return <Greeting firstName="Ben" lastName="Hector" />;
    }    
    function App2() {
      const props = {firstName: 'Ben', lastName: 'Hector'};
      return <Greeting {...props} />;
    }

#### 子代

props.children：在包含开始和结束标签的 JSX 表达式中，标记之间的内容作为特殊的参数传递。

有几种不同的方法来传递子代：

>字符串常量

```
<MyComponent>Hello world!</MyComponent>
```

代码分析：MyComponent 的 props.children 值将会直接是 "hello world!"

>JSX

可以通过子代嵌入更多的 JSX 元素。

```
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>
```

>JavaScript 表达式

可以将任何 {} 包裹的 JavaScript 表达式作为子代传递

```
<MyComponent>foo</MyComponent>

<MyComponent>{'foo'}</MyComponent>
```

>函数

props.children 可以像其它属性一样传递任何数据,当使用自定义组件，则可以将调用 props.children 来获得传递的子代

    function Repeat(props) {
      let items = [];
      for (let i = 0; i < props.numTimes; i++) {
        items.push(props.children(i));
      }
      return <div>{items}</div>;
    }    
    function ListOfTenThings() {
      return (
        <Repeat numTimes={10}>
          {(index) => <div key={index}>This is item {index} in the list</div>}
        </Repeat>
      );
    }

>布尔值、Null 和 Undefined 被忽略

false、null、undefined 和 true 都是有效的子代，但它们不会直接被渲染

完整代码示例：

    import React from 'react';
    import ReactDOM from 'react-dom';

    //父组件实例化后传入子代，然后通过this.props.children渲染子代
    class Home extends React.Component{
        constructor(props){
            super(props);
        }
        render(){
            return (
                <div>
                    {this.props.children}
                </div>
            )
        }
    }

    class Aaa extends React.Component{
        render(){
            return (
                <div>
                    Aaa
                </div>
            )
        }
    }

    class Bbb extends React.Component{
        render(){
            return (
                <div>
                    Bbb
                </div>
            )
        }
    }
    ReactDOM.render(
        <div>
            <Home>
                Hello world!
                {'Hello world!'}
                <Aaa/>
                <Bbb/>
            </Home>
        </div>,
        document.getElementById('root')
    );


JSX 允许在模板中插入数组，数组会自动展开所有成员

    var arr = [
      <h1>hello,world!</h1>,
      <h2>hello,react!</h2>,
    ];
    ReactDOM.render(
      <div>{arr}</div>,
      document.getElementById('example')
    );

注意:由于 JSX 就是 JavaScript，一些标识符像 class 和 for 不建议作为 XML 属性名。作为替代，React DOM 使用 className 和 htmlFor 来做对应的属性。

## 3、组件
>React 允许将代码封装成组件，然后像插入普通 HTML 标签一样，在网页中插入这个组件。

>组件很像一个函数，有一个可以接收任意数据类型的参数（“props”），并返回一个需要在页面上展示的React元素。

#### 组件的创建
>使用JavaScript函数

    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }
    

>React.createClass(ES5的语法) 

    let Welcome = React.createClass({
      render: function() {
        return <h1>Hello {this.props.name}</h1>;
      }
    });
    
    ReactDOM.render(
      <Welcome name="John" />,
      document.getElementById('example')
    );

代码分析：
* 变量 Welcome 就是一个组件类。模板插入 <Welcome /> 时，会自动生成 Welcome 的一个实例（下文的"组件"都指组件类的实例）。所有组件类都必须有自己的 render 方法，用于输出组件。
注意: 组件类的第一个字母必须大写，否则会报错，比如Welcome不能写成welcome。另外，组件类只能包含一个顶层标签，否则也会报错。
* 组件的用法与原生的 HTML 标签完全一致，可以任意加入属性，比如 <Welcome name="John"> ，就是 Welcome 组件加入一个 name 属性，值为 John。组件的属性可以在组件类的 this.props 对象上获取，比如 name 属性就可以通过 this.props.name 读取。
>extends React.Component(ES6的语法)

    class Welcome extends React.Component {
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }
    
#### 组建的渲染
>HTML 标签

``
const element = <div />;
``

>自定义组件

    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }    
    const element = <Welcome name="Sara" />;
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
    
#### 组件嵌套组件

    //创建一个Welcome组件
    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }
    //创建一个App组件，用来多次渲染Welcome组件    
    function App() {
      return (
        <div>
          <Welcome name="Sara" />
          <Welcome name="Cahal" />
          <Welcome name="Edite" />
        </div>
      );
    }    
    ReactDOM.render(
      <App />,
      document.getElementById('root')
    );
    
#### 组件的拆分

尽可能减少代码耦合，提高代码的复用性，将组件切分为更小的组件

## state
* React 将组件看成是一个状态机，通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。
* React里，组件内部的状态，可以使用 state。因此只需更新组件的 state，然后根据新的 state 重新渲染用户界面。

        class Clock extends React.Component {
          constructor(props) {
            super(props);
            //初始化一个state，this.state必须在构造函数里
            this.state = {date: new Date()};
          }
            //封装改变state状态的函数
           tick(){
            this.setState({ date: new Date() });
          }
          componentDidMount(){
          //每秒改变state状态
            this.interval = setInterval( this.tick.bind(this), 1000 );
          }
        
          componentWillUnmount(){
          //销毁定时器
            clearInterval(this.interval);
          }
          render() {
            return (
              <div>
                <h1>Hello, world!</h1>
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
              </div>
            );
          }
        }        
        ReactDOM.render(
          <Clock />,
          document.getElementById('root')
        );
 
#### setState()
 
this.props 和 this.state 可能是异步更新的，不应该依靠它们的值来计算下一个状态。例如：

``
this.setState({
  counter: this.state.counter + this.props.increment,
});
``

正确方式：

``
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
``

代码分析：先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数

## props
* this.props是获取父组件传递给子组件的属性值。
* this.props 对象的属性与组件的属性一一对应。

## 定义props默认值

defaultProps 用来确保 this.props.name 在父组件没有特别指定的情况下，有一个初始值
    
    class Greeting extends React.Component {
      static defaultProps = {
        name: 'Jack'
      }    
      render() {
        return (
          <div>Hello, {this.props.name}</div>
        )
      }
    }
    ReactDOM.render(
      <Greeting />,
      document.getElementById('example')
    );

或者

    class Greeting extends React.Component {
      render() {
        return (
          <h1>Hello, {this.props.name}</h1>
        );
      }
    }
    Greeting.defaultProps = {
      name: 'Jack'
    };
    ReactDOM.render(
      <Greeting />,
      document.getElementById('example')
    );
    
## PropTypes
组件的属性可以接受任意值，为了约定统一的接口规范，通过指定 propTypes 可以校验props属性值的类型，当别人使用组件时，提示写入的参数是否符合要求。

    import PropTypes from 'prop-types';    
    class Greeting extends React.Component {
      render() {
        return (
          <h1>Hello, {this.props.name}</h1>
        );
      }
    }    
    Greeting.propTypes = {
      name: PropTypes.string
    };  
    
>常用的验证器
    
    optionalArray: PropTypes.array,     //数组
    optionalBool: PropTypes.bool,       //布尔值
    optionalFunc: PropTypes.func,       //函数
    optionalNumber: PropTypes.number,   //数字
    optionalObject: PropTypes.object,   //对象
    optionalString: PropTypes.string,   //字符串
    // 限制你的属性值是某个特定值之一
    optionalEnum: PropTypes.oneOf(['News', 'Photos']),
    
## 数据自顶向下流动

组件可以选择将其状态作为属性传递给其子组件，子组件通过其props属性中接收到父组件的值。这通常被称为自顶向下或单向数据流，任何状态始终由某些特定组件所有，并且从该状态导出的任何数据或UI只能影响树中下方的组件。
    
## 获取真实的DOM节点
>Refs 提供了一种访问在 render 方法中创建的 DOM 节点或 React 元素的方式。
 
在 React 数据流中, 属性（props）是父组件与子组件数据传递的唯一方式。要修改子组件的状态，需要使用新的props重新渲染它。但是，有时需要在典型数据流外强制修改子组件。要修改的子组件可以是React组件实例，也可以是 DOM 元素。对于这两种情况，React 提供了Refs方式。
 
> 使用 refs 的情况

* 处理焦点、文本选择或媒体控制。
* 触发强制动画。
* 集成第三方 DOM 库

注意： 应该尽量通过 state/props 更新组件，不要使用该功能去更新组件的DOM。

>获取设置ref

    class MyComponent extends React.Component {
      handleClick = () =>{
      //获取到元素的方式
          this.refs.myTextInput.focus();
          let ele = findDOMNode(this.refs.myTextInput);
          let ele2 = this.refs.myTextInput;
          
          console.log( ele );
          console.log( ele.innerHTML );
          console.log( ele2.innerHTML );
        },
        render() {
          return (
            <div>
              <input type="text" ref="myTextInput" />
              <input type="button" value="Focus the text input" onClick={this.handleClick} />
            </div>
          );
        }
    }

>为 DOM 元素添加 Ref

    class CustomTextInput extends React.Component {
      constructor(props) {
        super(props);
        this.focus = this.focus.bind(this);
      }    
      focus() {
        // 直接使用原生 API 使 text 输入框获得焦点
        this.textInput.focus();
      }    
      render() {
        // 使用 `ref` 的回调将 text 输入框的 DOM 节点存储到 React
        // 实例上（比如 this.textInput）
        return (
          <div>
            <input
              type="text"
              ref={(input) => { this.textInput = input; }} />    
            <input
              type="button"
              value="Focus the text input"
              onClick={this.focus}
            />
          </div>
        );
      }
    }
    
>不能在函数式组件上使用 ref 属性    

如果想要使用 ref，应当像使用生命周期方法或者 state一样，应该将其转换为 class 组件。如下是不能正常使用ref：

    function InputComponent() {
      return <input type="text"/>;
    }    
    class MyComponent extends React.Component {
          handleClick = () =>{
              this.refs.myTextInput.focus();
            },
            render() {
              return (
                <div>                 
                  <InputComponent ref="myTextInput" />
                  <input type="button" value="Focus the text input" onClick={this.handleClick} />
                </div>
              );
            }
        }
  
代码分析：组件 MyComponent 的子节点有一个文本输入框，用于获取用户的输入。这时就必须获取真实的 DOM 节点，虚拟 DOM是拿不到用户输入的。为了做到这一点，文本输入框必须有一个 ref 属性，然后 this.refs.[refName] 就会返回这个真实的 DOM 节点。
  
>对父组件暴露 DOM 节点
    
    function CustomTextInput(props) {
      return (
        <div>
          <input ref={props.inputRef} />
        </div>
      );
    }    
    class Parent extends React.Component {
      constructor(props) {
        super(props);
        this.inputElement = React.createRef();
      }
      render() {
        return (
          <CustomTextInput inputRef={this.inputElement} />
        );
      }
    }  
   
代码分析：父组件的ref属性通过props传递给子组件，从而间接访问子组件的节点 
  
## 组件的生命周期
每当组件第一次加载到DOM中的时候，这在React中被称为挂载。

每当组件生成的这个DOM被移除的时候，这在React中被称为卸载。

在组件类上声明特殊的方法，当组件挂载或卸载时执行这些方法，这些方法就被称作生命周期钩子

组件的生命周期分成三个状态
* Mounting：已插入真实 DOM
* Updating：正在被重新渲染
* Unmounting：已移出真实 DOM  

React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。
* componentWillMount()
* componentDidMount()
* componentWillUpdate(object nextProps, object nextState)
* componentDidUpdate(object prevProps, object prevState)
* componentWillUnmount() 

React 还提供两种特殊状态的处理函数
>已加载组件收到新的参数时调用

componentWillReceiveProps(object nextProps)
>组件判断是否重新渲染时调用

shouldComponentUpdate(object nextProps, object nextState)   
    
## 事件处理

* React事件绑定属性的命名采用驼峰式写法，而不是小写。
* 如果采用 JSX 的语法你需要传入一个函数作为事件处理函数，而不是一个字符串(DOM元素的写法)  

``
<button onClick={activateLasers}></button>
``  
  
在 React 中另一个不同是你不能使用返回 false 的方式阻止默认行为。必须明确的使用 preventDefault

    function ActionLink() {
      function handleClick(e) {
        e.preventDefault();
        console.log('The link was clicked.');
      }    
      return (
        <a href="#" onClick={handleClick}>
          Click me
        </a>
      );
    }  
    
>向事件处理程序传递参数

``
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
``

``
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
``

