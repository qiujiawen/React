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

React Router 使用路由嵌套来让你定义view的嵌套集合，当一个给定的 URL 被调用时，整个集合中（命中的部分）都会被渲染。

> 路径语法

路由路径是匹配一个（或一部分）URL 的 一个字符串模式

* :paramName – 匹配一段位于‘/’ ‘?’ 或‘#’之后的URL，匹配的部分将被作为一个参数。

* () – 在它内部的内容被认为是可选的

* “*” – 匹配任意字符（非贪婪的）直到命中下一个字符或者整个 URL 的末尾，并创建一个 splat 参数

示例：

    <Route path="/hello/:name">         // 匹配 /hello/michael 和 /hello/ryan
    <Route path="/hello(/:name)">       // 匹配 /hello, /hello/michael 和 /hello/ryan
    <Route path="/files/*.*">           // 匹配 /files/hello.jpg 和 /files/path/to/hello.jpg
    
    
> 优先级

根据路由定义的顺序自顶向下匹配路由。

## API

#### 路由器(Router)

Router职责：保持 UI 和 URL 的同步。

> BrowserRouter

    import { BrowserRouter } from 'react-router-dom'
    
    <BrowserRouter
      basename={optionalString}
      forceRefresh={optionalBool}
      getUserConfirmation={optionalFunc}
      keyLength={optionalNumber}
    >
      <App/>
    </BrowserRouter>
    
    
#### Link 

a标签一样，用于跳转。


    import { Link } from 'react-router-dom'
    <Link to="/about">About</Link>
    <Link to={{
      pathname: '/courses',
      search: '?sort=name',
      hash: '#the-hash',
      state: { fromDashboard: true }
    }}/>
    















