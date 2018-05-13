# React深入了解
## 1、受控组件

用户在表单填入内容时，即用户与组件互动时，该如何获取用户输入的信息？

>React控制组件的状态改变

>每个状态的改变都有一个与之相关的处理函数

``
render() {
  return <input type="text" value="Hello!" />;
}
``

#### 表单

    class NameForm extends React.Component {
      constructor(props) {
        super(props);
        this.state = {value: ''};    
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
      }    
      handleChange(event) {
        this.setState({value: event.target.value});
      }    
      handleSubmit(event) {
        alert('A name was submitted: ' + this.state.value);
        event.preventDefault();
      }    
      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            <label>
              Name:
              {/* 输入框 */}
              <input type="text" value={this.state.value} onChange={this.handleChange} />
              {/* textarea 标签 */}
              <textarea value={this.state.value} onChange={this.handleChange} />
              {/* select 标签 */}
              <select value={this.state.value} onChange={this.handleChange}>
                          <option value="grapefruit">Grapefruit</option>
                          <option value="lime">Lime</option>
                          <option value="coconut">Coconut</option>
                          <option value="mango">Mango</option>
                        </select>
            </label>
            <input type="submit" value="Submit" />
          </form>
        );
      }
    }

代码分析：通过onchange事件(当离开输入框后，事件回调函数将被触发)和设置value属性来获取用户输入信息。

#### 多个输入的解决方法
处理多个受控的input元素时，可以通过给每个元素添加一个name属性，声明一个函数来处理，函数内部根据 event.target.name的值来选择做什么。

## 2、非受控组件
>非受控组件即组件的状态改变不受React控制

``
render() {
  return <input type="text" />;
}
``

    class NameForm extends React.Component {
      constructor(props) {
        super(props);
        this.handleSubmit = this.handleSubmit.bind(this);
      }    
      handleSubmit(event) {
        alert('A name was submitted: ' + this.textInput.value);
        event.preventDefault();
      }    
      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            <label>
              Name:
              <input type="text" ref={(input) => this.textInput = input} />
            </label>
            <input type="submit" value="Submit" />
          </form>
        );
      }
    }

代码分析：非受控组件将真实数据保存在 DOM 中，所以可以通过ref获取真实的DOM节点。

#### 设置默认值
>使用defaultValue 

    render() {
      return (
        <form onSubmit={this.handleSubmit}>
          <label>
            Name:
            <input
              defaultValue="Bob"
              type="text"
              ref={(input) => this.input = input} />
          </label>
          <input type="submit" value="Submit" />
        </form>
      );
    }
    
    
## 3、map()
#### forEach()和map()比较
>相同点
* 都是数组遍历
* 支持2个参数：一个是回调函数（item,index,list）和上下文
* 回调函数中传递了三个参数值：数组中的当前项item,当前项的索引index,原始数组input
* 克隆一份原数组进行操作  

>不同点
* forEach没有返回值
* map有返回值

#### 渲染多个组件
    
    function NumberList(props) {
      const numbers = props.numbers;
      const listItems = numbers.map((number) =>
        <li key={number.toString()}>
          {number}
        </li>
      );
      return (
        <ul>{listItems}</ul>
      );
    }    
    const numbers = [1, 2, 3, 4, 5];
    ReactDOM.render(
      <NumberList numbers={numbers} />,
      document.getElementById('root')
    );   