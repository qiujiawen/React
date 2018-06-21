
# Webpack的搭建

#### Webpack的简介

Webpack做的事情：当浏览器不能直接识别一些拓展语言（Scss，TypeScript等）的时候,Webpack的工作就是将其转换成浏览器可以识别的语言；而且 webpack 能把各种资源，如js、jsx、coffee、样式、图片等作为模块来使用和处理，并且也将它们转换成浏览器可以识别的资源。

## Webpack的安装

#### 初始化npm

```
npm init -y
```

执行命令后，自动创建一个 package.json文件，该文件是用于管理本地安装的npm软件包的配置文件。

#### Webpack的安装命令

```
npm i -D webpack webpack-cli webpack-dev-server
```

#### 配置文件

创建一个 webpack.config.js 配置文件，它是运行webpack命令的时候加载的配置文件。

当webpack命令运行的时候，会加载该文件，根据该文件的配置内容进行执行。

是用node语法进行配置，默认导出文件 -- module.exports={}

## Webpack的核心概念

>入口 - entry

入口文件是整个应用程序的执行起点，webpack会从入口文件开始分析文件模块之间的依赖，并加载，然后打包成一个文件。(单入口、多入口)

>出口 - output

配置webpack如何向硬盘中写入文件，注意：入口可以多个，但是输出配置只能有一个，配置值为一个对象格式。

>加载器 - Loader

Loader是webpack中用于处理任务的核心内容，他是一个文件内容预处理器，可以通过Loader来处理和转换指定的文件；也就是指webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理。Loader是在打包完成之前执行。

文档：https://webpack.docschina.org/loaders/

>插件 - Plugins

插件让webpack对打包后的文件进一步处理，比如优化代码、压缩文件，插件是打包完成后执行。

文档：https://doc.webpack-china.org/plugins

## 初始化一个简易的webpack脚手架

1、初始化npm(npm init -y)和创建 webpack.config.js 配置文件

2、安装依赖

```
// 安装 webpack
npm i -D webpack webpack-cli webpack-dev-server
// 安装 React和路由
npm install -S react react-dom react-router-dom
// 安装处理图片和字体文件的loader
npm install -D css-loader style-loader
// 安装支持高级语法(babel)的loader
npm install -D babel-core babel-loader babel-preset-env babel-preset-react babel-preset-stage-2 babel-polyfill
```

3、配置webpack.config.js文件

简易配置文件示例：

    const path = require('path');
    const HtmlWebpackPlugin = require('html-webpack-plugin');
    module.exports = {

    //入口
        entry: './src/page/index.js',

    //出口
        output: {
            filename: '[name].js',
            path: path.resolve(__dirname,'dist')
        },

    //loader
        module: {
            rules: [
                {
                    test: /\.js$/,
                    use: [
                        {
                            loader: "babel-loader",
                            options: {
                                presets: ['env', "react", "stage-2"]
                            }
                        }
                    ],
                },
                {
                    test: /\.(jpg|png|jpeg|gif)$/,
                    use: [
                        {
                            loader: 'url-loader',
                            options: {
                                limit: 8192
                            }
                        },
                    ]
                },
                {
                    test: /\.(ttf|eot|woff|woff2|svg)$/,
                    use: [
                        {
                            loader: 'file-loader',
                            options: {
                                limit: 8192
                            }
                        },
                    ]
                },
                {
                    test: /\.(sass|scss|css)$/,
                    use: [
                        'style-loader',
                        {
                            loader: 'css-loader',
                            options: {minimize: true}
                        },{
                            loader: "sass-loader"
                        }

                    ]
                }
            ]
        },

        //插件
        plugins: [
            new HtmlWebpackPlugin(
                {
                    title : 'my App',
                    filename : 'index.html',
                    template:  path.resolve(__dirname, "./src/index.html"),
                }
            )
        ],
        devServer: {
            contentBase: path.resolve(__dirname,'dist'),
            compress: true,
            port: 9002
        }
    }

package.json文件完整示例：

    {
      "name": "webpackDemo",
      "version": "1.0.0",
      "description": "webpack creat",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "webpack-dev-server --open --mode development",
        "build": "--mode production"
      },
      "keywords": [],
      "author": "jw",
      "license": "ISC",
      "dependencies": {
        "react": "^16.4.0",
        "react-dom": "^16.4.0",
        "react-router-dom": "^4.3.1"
      },
      "devDependencies": {
        "babel-core": "^6.26.3",
        "babel-loader": "^7.1.4",
        "babel-polyfill": "^6.26.0",
        "babel-preset-env": "^1.7.0",
        "babel-preset-react": "^6.24.1",
        "babel-preset-stage-2": "^6.24.1",
        "css-loader": "^0.28.11",
        "file-loader": "^1.1.11",
        "html-webpack-plugin": "^3.2.0",
        "less-loader": "^4.1.0",
        "style-loader": "^0.21.0",
        "url-loader": "^1.0.1",
        "webpack": "^4.12.0",
        "webpack-cli": "^3.0.4",
        "webpack-dev-server": "^3.1.4"
      }
    }

4、运行 npm start 启动

#### webpack和webpack-dev-server的基本命令

    webpack 开发环境下编译
    webpack -p 产品编译及压缩
    webpack --watch 开发环境下持续的监听文件变动来进行编译
    webpack -d 引入 source maps
    webpack --progress 显示构建进度
    webpack --display-error-details 这个很有用，显示打包过程中的出错信息
    webpack --profile 输出性能数据，可以看到每一步的耗时

注意：

webpack的配置文件并没有固定的命名，也没有固定的路径要求，如果你直接用webpack来执行编译，webpack默认读取的将是当前目录下的webpack.config.js。

如果有其它命名的需要或是你有多份配置文件，可以使用--config参数传入路径。

#### 执行webpack-dev-server后面参数的意思

    webpack-dev-server - 在 localhost:8080 建立一个 Web 服务器
    webpack-dev-server --devtool eval - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
    webpack-dev-server --progress - 显示合并代码进度
    webpack-dev-server --colors - 命令行中显示颜色
    webpack-dev-server --content-base build - webpack-dev-server服务会默认以当前目录伺服文件，如果设置了content-base的话，服务的根路径则为build目录
    webpack-dev-server --inline 可以自动加上dev-server的管理代码，实现热更新
    webpack-dev-server --hot 开启代码热替换，可以加上HotModuleReplacementPlugin
    webpack-dev-server --port 3000 设置服务端口

#### resolve

resolve下常用的是extension和alias字段的配置

>extension

```
resolve: {
    extensions: ["", ".js", ".jsx", ".css", ".json"],
},
```

作用：require或是import引入文件的时候没有加文件后缀。比如：import component from './component';

>alias

    let path = require('path');
    let pathToReact = path.join(__dirname, "./node_modules/react/dist/react.js");
    let pathToReactDOM = path.join(__dirname, "./node_modules/react-dom/dist/react-dom.js");

    module.exports = {
        ...
        resolve: {
          extensions: ["", ".js", ".jsx", ".css", ".json"],
          alias: {
            'react': pathToReact,
            'react-dom': pathToReactDOM
          }
        }
    };

作用：配置别名，加快webpack构建的速度。每当 "react" 在代码中被引入，它会使用压缩后的 React JS 文件，而不是到 node_modules 中找；每当 Webpack 尝试去解析那个压缩后的文件，就会阻止它，因为这不必要。

#### mode

mode配置选项会告诉webpack相应地使用其内置优化，也用于分离开发环境和生产环境。

官方文档: https://webpack.js.org/concepts/mode/#mode-production

mode的使用：

在 webpack.config.js 配置文件：

```
module.exports = {
  mode: 'production'
};
```

或者在 package.json 文件

```
"scripts": {
    "dev": "--mode development",
    "build": "--mode production"
  }
```

### 公共

#### ProvidePlugin

自动加载模块，而不必到处使用 import 或 require。

```
plugins:[
        new webpack.ProvidePlugin({
            React: "React",
            ReactDOM: "ReactDom",
            PropTypes: "PropTypes",
            ReactRouterDom: "ReactRouterDOM",
        }),
    ],
```

#### webpack-merge

将生产环境和开发环境做了区分，但是还是要注意不要做重复的原则，保留通用配置。为了将这些配置合并在一起，需要使用webpack-merge插件，通过“通用”配置，我们不必在环境特定的配置中写重复代码。

安装插件命令：

```
npm install --save-dev webpack-merge
```

```
const merge = require('webpack-merge');
```

详情：https://webpack.js.org/guides/production/#setup

#### html-webpack-plugin

简化创建HTML文件

```
npm install --save-dev html-webpack-plugin
```

在webpack.config.js文件中，配置如下：

```
// 引用这个plugin
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
          new HtmlWebpackPlugin(
              {
                  title : 'my App',
                  filename : 'index.html',
                  template:  path.resolve(__dirname, "./src/index.html"),
              }
          )
      ]
};
```

index.html文件中：

```
<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="UTF-8">
    <title><%= htmlWebpackPlugin.options.title %></title>
</head>
<body>
<div id="root"></div>
</body>
</html>
```

配置属性：

```
title：生成html文件的标题
filename：就是html文件的文件名，默认是index.html
template：指定你生成的文件所依赖哪一个html文件模板
inject：true body head false
    true 默认值，script标签位于html文件的 body 底部
    body script标签位于html文件的 body 底部
    head script标签位于html文件的 head中
    false 不插入生成的js文件，这个几乎不会用到的
minify：使用minify会对生成的html文件进行压缩，默认是false。
cache：默认是true的，表示内容变化的时候生成一个新的文件。
showErrors：当webpack报错的时候，会把错误信息包裹再一个pre中，默认是true。
```

插件GitHub地址：https://github.com/jantimon/html-webpack-plugin#configuration

