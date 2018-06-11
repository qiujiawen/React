
# Ant Design of React

## 在 create-react-app 中使用

#### 安装命令

```
npm install antd --save
```


#### 高级配置：

因为实际上加载了全部的 antd 组件的样式，因此需要进行按需加载的配置

>安装react-app-rewired

```
npm i react-app-rewired --dev
```

>package.json 文件中修改参数

```
"scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test --env=jsdom",
+   "test": "react-app-rewired test --env=jsdom",
}
```

>安装 babel-plugin-import

babel-plugin-import 是一个用于按需加载组件代码和样式的 babel 插件

```
npm i babel-plugin-import --dev
```


>创建一个 config-overrides.js

然后在项目根目录创建一个 config-overrides.js 用于修改默认配置。

```
const { injectBabelPlugin } = require('react-app-rewired');
module.exports = function override(config, env) {
    config = injectBabelPlugin(['import', { libraryName: 'antd', libraryDirectory: 'es', style: 'css' }], config);
    return config;
};
```


Antd的官网：https://ant.design/docs/react/introduce-cn