# Webpack搭建开发环境

开发环境，就是你的代码在本地服务器上在测试、更改、运行；生产环境，就是你的代码就是已经开始在真实服务器中使用。

webpack 可以适用于开发环境主要是运用 node.js 去搭建一个本地服务。

#### React-hot-loader热更新

只要在编辑器中保存我们编辑的代码，浏览器即可实时刷新。

```
npm install --save-dev react-hot-loader
```

在webpack.config.js文件中，配置如下：

```
const webpack = require('webpack');
module.exports = {
  plugins: [
    // 是webpack内带的插件，不用安装引入webpack就可以
    new webpack.HotModuleReplacementPlugin()
      ]
};
```

更多详情：http://gaearon.github.io/react-hot-loader/getstarted/

#### 自动打开浏览器

资源构建完成之后，自动打开浏览器，提高开发体验

```
npm install open-browser-webpack-plugin --save-dev
```

在webpack.config.js文件中，配置如下：

```
const OpenBrowserPlugin = require('open-browser-webpack-plugin');
module.exports = {
  plugins: [
           new OpenBrowserPlugin({ url: 'http://localhost:8080' })
      ]
};
```

#### 文件夹删除

clean-webpack-plugin插件是用于在构建之前删除/清除构建文件夹。

安装命令：

```
npm i clean-webpack-plugin --save-dev
```

webpack配置示例：

```
const path = require('path');
const rootPath = path.resolve(__dirname,'../');
const CleanWebpackPlugin = require('clean-webpack-plugin');
plugins : [
        new CleanWebpackPlugin(['dev'],{root : rootPath}),
    ]
```

#### 错误重启

```
const webpack = require('webpack');
plugins : [
       //  错误重启
      new webpack.NoEmitOnErrorsPlugin(),
   ]
```

