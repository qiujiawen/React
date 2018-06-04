# React-Router

#### 安装、引用

>安装

```
npm install --save react-router
npm install --save react-router-dom
```

>引用

    import { BrowserRouter as Router, Route } from 'react-router-dom'    
    <Router>
      <div>
        <Route exact path="/" component={Home}/>
        <Route path="/news" component={NewsFeed}/>
      </div>
    </Router>

#### 简介

react-router 是一个基于React之上的强大路由库；react-router向应用中快速地添加视图和数据流，同时保持页面与 URL 间的同步。

示例：a链接通过一个地址跳转到一个页面，react-router通过匹配a链接的地址找到需要渲染到该页面的组件。

#### 获取 URL 参数

>原生js

示例URL地址：http://localhost/demo.html?id=1&page=2#section=3

* window.location.href

获取到 http://localhost/demo.html?id=1&page=2#section=3

* location.search

获取到 ?id=1&page=2#section=3

* location.hash

获取到 #section=3

* window.location.port

设置或获取与 URL 关联的端口号码

* window.location.protocol

设置或获取 URL 的协议部分


#### 路由配置

路由配置是一组指令，用来告诉 router 如何匹配 URL以及匹配后如何执行代码。

#### 路由匹配原理

路由拥有三个属性来决定是否“匹配“一个 URL：

> 路由嵌套

React Router 使用路由嵌套来让你定义view的嵌套集合，当一个给定的 URL 被调用时，整个集合中（命中的部分）都会被渲染。父级组件通过this.props.children来渲染子组件

    import React from 'react';
    import ReactDOM from 'react-dom';
    import {BrowserRouter as Router,Route} from 'react-router-dom';

    class Home extends React.Component{
        constructor(props){
            super(props);
        }
        render(){
            return (
                <div>
                    home
                    {/*
                    添加了this.props.children，路由匹配/aaa和/bbb时才会渲染Aaa组件和Bbb组件
                    */}
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
        <Router>
            <div>
                <Route path = '/' >
                    <Home>
                            <Route path = '/bbb' component = {Bbb}/>
                            <Route path = '/aaa' component = {Aaa}/>
                    </Home>
                </Route>
            </div>
        </Router>,
        document.getElementById('root')
    );

> 路径语法

路由路径是匹配一个（或一部分）URL 的 一个字符串模式

* :paramName – 匹配一段位于‘/’ ‘?’ 或‘#’之后的URL，匹配的部分将被作为一个参数。paramName类似于“*”

* () – 在它内部的内容被认为是可选的

* “*” – 匹配任意字符（非贪婪的）直到命中下一个字符或者整个 URL 的末尾，并创建一个 splat 参数

示例：

    <Route path="/hello/:name">         // 匹配 /hello/michael 和 /hello/ryan 和 /hello/'输入任意值，除了/ ? 或 #'
    <Route path="/hello(/:name)">       // 匹配 /hello, /hello/michael 和 /hello/ryan
    <Route path="/files/*.*">           // 匹配 /files/hello.jpg 和 /files/path/to/hello.jpg
    
    
> 优先级

路由匹配规则是从上到下执行，每次URL地址变化，都会遍历一次Route集合。

```
// 同时渲染Aaa组件和Bbb组件
<Route path = '/aaa' component = {Aaa}/>
<Route path = '/aaa' component = {Bbb}/>
```


## 基本组件

React Router中有三种类型的组件：路由器组件，路由匹配组件和导航组件。

```
import { BrowserRouter, Route, Link } from 'react-router-dom'
```

#### 路由器组件

React Router的核心应该是一个路由器组件。两种路由器 BrowserRouter 和 HashRouter 。

BroswerRouter是需要服务端配合的，服务端重定向到首页,会有刷新页面就发现找不到路径了隐患。

HashRouter用于静态文件。

Router职责：保持 UI 和 URL 的同步。

        import { BrowserRouter } from 'react-router-dom'

        <BrowserRouter
          basename={optionalString}
          forceRefresh={optionalBool}
          getUserConfirmation={optionalFunc}
          keyLength={optionalNumber}
        >
          <App/>
        </BrowserRouter>

#### 路由匹配组件

Route 是用于声明路由映射到应用程序的组件层。

有两个路由匹配组件：Route 和 Switch

当Route的path匹配时渲染组件，当它不匹配时，它将呈现null。如果Route没有path的话，将永远匹配。

    <Route path='/about' component={About}/>
    <Route path='/contact' component={Contact}/>
    <Route component={Always}/>

Switch组件用于和Route组合在一起，类似||（逻辑或)

    <Switch>
      <Route exact path='/' component={Home}/>
      <Route path='/about' component={About}/>
      <Route path='/contact' component={Contact}/>
    </Switch>

##### 路由匹配组件渲染方式

有3种方法：component render  children

>component

component方法提供了内联功能，则每次渲染都会创建一个新组件。这会导致现有组件卸载和安装新组件，而不是仅更新现有组件。

```
 <Route path = '/' component = {App}/>
```

>render

方便于内联渲染和包装，不需要重新安装组件。

```
<Route path = '/' render = {() =>
    <App/>
}/>
```

>children

children是适用于：不管是否匹配都需要渲染组件。根据路线是否匹配来动态调整用户界面。

##### 路由匹配组件(Route) exact和strict

>exact

exact为true时，只有当路径location.pathname 完全匹配时才匹配。

```
//添加exact，exact为true
<Route exact path="/one" component={About}/>
```

path	location.pathname	exact	matches?

/one	/one/two	        true	no

/one	/one/two	        false	yes

>strict

path尾部是否有斜线，如果有斜线，那么location.pathname的尾部必须也要有斜线才能匹配。path尾部没有斜线，location.pathname有没有斜线都可以匹配

```
<Route strict path="/one/" component={About}/>
```

path	location.pathname	是否匹配

/one/	/one	不能

/one/	/one/	可以

/one/	/one/two	可以

```
<Route exact strict path="/one" component={About}/>
```

path	location.pathname	是否匹配?

/one	/one	可以

/one	/one/	不能

/one	/one/two	不能


##### 路由匹配组件(Route)-Route props

当一个Route被匹配成功时，Route就会向渲染的组件传递props。

传递的props有三个属性值：match、location、history

在component render  children方法下打印Route props，完整代码示例：

    import React from 'react';
    import ReactDOM from 'react-dom';
    import {BrowserRouter as Router,Route,Link} from 'react-router-dom';

    class Home extends React.Component{
        constructor(props){
            super(props);
        }
        render(){
            return (
                <div>
                    home
                </div>
            )
        }
    }

    class Aaa extends React.Component{
        render(){
            console.log(this.props);
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
        <Router>
            <div>
                <div>
                    <Link to = '/home'>跳转home页面</Link><br/>
                    <Link to = '/aaa'>跳转a页面</Link><br/>
                    <Link to = '/bbb'>跳转b页面</Link>
                </div>
                {/*路径是否匹配组件都会渲染*/}
                {/*children方法的时候打印route props*/}
                <Route path = '/home' children = {
                    (props) =>{
                        console.log(props);
                        return <Home/>
                    }
                }/>
                {/*render方法的时候打印route props*/}
                <Route  path = '/bbb' render = {(props) =>{
                    console.log(props);
                    return <Bbb/>
                }
                }/>
                {/*component方法的时候打印route props*/}
                <Route path = '/aaa' component = {Aaa}/>
            </div>
        </Router>,
        document.getElementById('root')
    );

##### 页面传值

Link跳转页面的时候，通过to: object可以传递数据，this.props.location.state可以取出传递的数据

完整代码示例：

    import React from 'react';
    import ReactDOM from 'react-dom';
    import {BrowserRouter as Router,Route,Link} from 'react-router-dom';

    class Home extends React.Component{
        render(){
            return (
                <div>
                    home
                </div>
            )
        }
    }

    class Aaa extends React.Component{
        render(){
            //这里可以获取到a链接传递的数据
            console.log(this.props);
            return (
                <div>
                    Aaa
                </div>
            )
        }
    }

    ReactDOM.render(
        <Router>
            <div>
                <div>
                    <Link to = '/home'>跳转home页面</Link><br/>
                    {/*跳转时传递数据*/}
                    <Link to = {
                        {
                            pathname:'/aaa',
                            state:{
                                strValue:'我是a链接的state'
                            }
                        }
                    }>跳转a页面</Link>
                </div>
                <Route path = '/home' component = {Home}/>
                <Route path = '/aaa' component = {Aaa}/>
            </div>
        </Router>,
        document.getElementById('root')
    );




#### 导航组件

React路由器提供了一个Link组件用于创建链接。

    //to: string
    <Link to='/'>Home</Link>
    //to: object
    <Link to={{
      pathname: '/courses',
      search: '?sort=name',
      hash: '#the-hash',
      state: { fromDashboard: true }
    }}/>
    // <a href='/'>Home</a>

NavLink组件是一种特殊的类型，Link的to属性与当前位置相匹配时，它可以将自身定义为“active” 。

    <NavLink to='/react' activeClassName='hurray'>React</NavLink>
    // <a href='/react' className='hurray'>React</a>


## API

#### match、location、history

>match

一个match对象包含Route path匹配URL的信息

match对象包含以下属性：params、isExact、path(route中的path)、url(Link中的to)

>location

location指地址栏当前的位置

location对象包含以下属性：key、pathname(Link对象中的pathname)、search、hash、state(Link对象中的state)

>history

history对象在刷新跳转时都会引起变化，history.location也是变化的

代码示例：

    import React from 'react';
    import ReactDOM from 'react-dom';
    import {BrowserRouter as Router,Route,Link} from 'react-router-dom';

    class Home extends React.Component{
        render(){
            return (
                <div>
                    home
                </div>
            )
        }
    }

    class Aaa extends React.Component{
        render(){
        //打印出获取到的match、location、history对象
            console.log(this.props.match);
            console.log(this.props.location);
            console.log(this.props.history);
            return (
                <div>
                    Aaa
                </div>
            )
        }
    }

    ReactDOM.render(
        <Router>
            <div>
                <div>
                    <Link to = '/home'>跳转home页面</Link><br/>
                    <Link to = '/aaa'>跳转a页面</Link>
                </div>
                <Route path = '/home' component = {Home}/>
                <Route path = '/aaa' component = {Aaa}/>
            </div>
        </Router>,
        document.getElementById('root')
    );

    
#### Redirect组件

有些时候，我们匹配一个路径，但是可能这个路径，我们更希望它指向一个新的展示界面，而不是它原本的路径匹配界面。

Redirect组件的必须属性是to属性，表示重定向的新地址。


    import React from 'react';
    import ReactDOM from 'react-dom';
    import {BrowserRouter as Router,Route,Redirect} from 'react-router-dom';

    class Home extends React.Component{
        render(){
            return (
                <div>
                    home
                </div>
            )
        }
    }

    class Other extends React.Component{
        render(){
            return (
                <div>
                    other
                </div>
            )
        }
    }
    //这个实例中，因为重定向，所以每个路由展示界面都是other界面
    ReactDOM.render(
        <Router>
            <div>
            //Redirect的基本使用方式
                <Route path = '/home' render = {() =>
                    <Redirect to="/other"/>
                }/>
                <Route path = '/other' component = {Other}/>
            </div>
        </Router>,
        document.getElementById('root')
    );

    















