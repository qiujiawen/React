# Webpack搭建生产环境

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
        new CleanWebpackPlugin(['build'],{root : rootPath}),
    ]
```

#### css文件单独加载和css3自动添加前缀

实际开发中需要把css文件单独打包出来，优化性能。使用 mini-css-extract-plugin插件替代 extract-text-webpack-plugin 插件。

```
npm install mini-css-extract-plugin --save-dev
```

在webpack.config.js文件中，配置如下：

```
const ExtractTextPlugin = require("mini-css-extract-plugin");
module: {
        rules: [
            {
                test: /\.(sass|scss|css)$/,
                use: [
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            importLoaders: 1,
                            minimize: true,
                        }
                    },
                    'sass-loader',
                    {
                        //  css3自动添加前缀
                        loader: "postcss-loader",
                        options: {
                            plugins: loader => [
                                require('autoprefixer')(),
                            ],
                        }
                    }
                ]
            },
        ]
    },
```

#### 应用代码和JS框架分离

修改webpack配置中的entry入口，并且添加CommonsChunkPlugin插件抽取出第三方资源。

webpack4 移除 CommonsChunkPlugin，取而代之的是两个新的配置项：optimization.splitChunks 和 optimization.runtimeChunk。

```
module.exports = {
    entry : {
        app : './src/page/index.js',
        vendor: [
                "react",
                "react-dom",
                "prop-types",
                "react-router-dom",
            ]
    }
}
```

官方默认配置：

```
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async',  // 通过chunks选项可以选择块，有3个值："initial"、"async"和"all"。分别用于选择初始块、按需加载的块和所有块
      minSize: 30000,   // 要生成的块的最小大小
      minChunks: 1,     // 分割前必须共享模块的最小块数
      maxAsyncRequests: 5,  //按需加载时的最大并行请求数
      maxInitialRequests: 3,    //入口点处的最大并行请求数
      automaticNameDelimiter: '~',  //默认情况下,webpack将使用块的起源和名称(如:vendors~main.js)生成名称。使用此选项可以指定用于生成名称的分隔符
      name: true, // 要控制代码块的命名
      // 默认模式会将所有来自node_modules的模块分配到一个叫vendors的缓存组；所有重复引用至少两次的代码，会被分配到default的缓存组
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,  // 缓存组选择哪些模块
          priority: -10 // 一个模块可以属于多个缓存组。优化会优先使用更高的缓存组,类似css 设置z-index
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true  // 允许复用已经存在的代码块，而不是新建一个新的
        }
      }
    }
  }
};
```

>optimization.runtimeChunk

optimization.runtimeChunk: true会给每个入口文件的输出再添加一个块，其中只包含运行时。

更多详情：https://webpack.js.org/plugins/split-chunks-plugin/

#### 压缩资源

利用 compression-webpack-plugin 插件对文件进行压缩

```
npm i -D compression-webpack-plugin
```

```
const CompressionPlugin = require('compression-webpack-plugin');
module.exports = {
  plugins: [
          /**
            *  gzip压缩
            *  asset: 目标资源名称.[file]会被替换成原始资源。[path]会被替换成原始资源的路径,[query]                          会被替换成查询字符串。默认值是 "[path].gz[query]"。
            *  algorithm：默认值是 "gzip"
            *  test：所有匹配该正则的资源都会被处理。默认值是全部资源
            *  threshold：只有大小大于该值的资源会被处理。单位是bytes，1KB = 1,024 Bytes
            *  minRatio：只有压缩率小于这个值的资源才会被处理
            */
           new CompressionPlugin({
               asset: "[path].gz[query]",
               algorithm: "gzip",
               test: /\.js$|\.css$|\.html$/,
               threshold: 10240,
               minRatio: 0.8
           }),
      ]
};
```


