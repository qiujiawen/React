# node

## 安装流程

1、访问[nodejs](https://nodejs.org/en/)官网，下载并安装，一路next。

2、安装成功

使用下面命令会出现版本号，是说明安装成功了

```
node -v
// 或者
npm -v
```

3、继续运行命令安装

``
npm i -g nrm
``

``
nrm use taobao
``

运行nrm use taobao命令后，成功出现网址，那么node安装就完成了。

## 安装插件的参数

1、自动把模块和版本号添加到dependencies部分;开发依赖模块

```
npm install module-name -save
// 或者
npm install module-name -S
```

2、自动把模块和版本号添加到devdependencies部分;产品依赖模块

```
npm install module-name -save-dve
// 或者
npm install module-name -D
```