# 1.基础配置

为什么需要打包工具？

在前端开发中，我们会使用到各种框架，库。还有我们熟悉的ES6。

但是这些东西不经过打包编译的话，浏览器是不认识的，所以就有了打包工具。

**常见的打包工具：**

- webpack
- Vite
- Gulp
- Grunt
- ...

## 1.1 webpack简介

在webpack中，认为一切资源都是模块，包括js文件，图片，还有css文件等等。

而webpack的其中一个功能就是找出这些模块之间的关系，并且将这些模块打包成一个几个文件。

![image-20230915220811274](https://raw.githubusercontent.com/zml212/FigureBed/main/202309152208387.png)

在webpack中，功能是有限的：

- 开发模式：仅能编译JS中共的`ES module`语法
- 生产模式：能编译JS中的`ES module`语法，还可以压缩JS代码

**使用webpack:**

1. 下载webpack:`npm i webpack -D`
2. 下载webpack-cli:`npm i webpack-cli -D`
3. 执行命令：`npx webpack 文件入口 --mode=development`

默认webpack打包的文件会放到dist文件夹内。

## 1.2 Webpack5大核心概念

- entry(入口)

指示webpack从哪个文件开始打包

- output(输出)

指示Webpack打包之后的文件输出到哪个位置，如何命名等等

- loader(加载器)

Webpack本身只能处理js，json等资源，其他的资源处理需要借助loader

- plugins(插件)

进一步扩展Webpack的功能 

- mode(模式)

主要有两种模式：

- 开发模式：development
- 生产模式：production

## 1.3 Webpack配置文件

在项目根目录创建一个文件：`webpack.config.js`

因为最后这个打包是在node.js环境下运行的，所以里面的模块化书写方式为node.js的模块化。

```javascript
const path = require('path')
module.exports = {
    // 模式
    mode: 'development',
    // 入口
    entry: path.join(__dirname, 'src', 'index.js'),
    // 输出
    output: {
        // 输出路径
        path: path.join(__dirname, 'dist'),
        // 输出文件名
        filename: 'index.js',
    },
    // 插件
    plugins: [

    ],
    // 加载器
    module: {

    },
}
```

当我们配置好这些之后，直接在终端执行命令`npx webpack`就可以了。

## 1.4 开发模式介绍

开发模式就是在开发过程中使用到的模式，主要有以下几点功能：

1. 编译代码，使得浏览器能够识别运行

开发师我们有样式，字体图标，图片资源，html资源等等，但是webpack默认是不会处理这些资源的，所以我们要加载配置项来编译这些资源。

2. 代码质量检查，树立代码规范

提前检查代码的一些隐患，让代码运行时能够更贱健壮。

提前检查代码规范还有格式，统一团队编程风格，让代码更加美观

### 1.5处理样式资源

### 1.5.1 介绍

Webpack本身是不会处理样式资源的，所以我们需要借助loade来帮助解析样式资源。

比如CSS、less、sass、scss、style

[Webpack loader文档](https://webpack.docschina.org/loaders/)

### 1.5.2 处理CSS资源

1. 下载包

首先我们需要下载处理css样式的loader包

```javascript
npm i css-loader style-loader -D
```

注意需要下载两个loader。

2. 功能介绍

`css-loader`：负责将CSS文件编译为Webpack能够识别的模块

`style-loader`：会动态创建爱哪一个Style标签，里面防止Webpack中CSS模块的内容

此时的样式就会以Style标签的形式在页面上生效

3. 使用

```javascript
// 加载器
    module: {
        rules: [
            // loader的配置
            {
                test: /\.css$/, // 匹配以.css结尾的文件
                use: ["style-loader", "css-loader"], // 上面符合条件的文件，使用这些loader处理
            }
        ]
    },
```

在`use`中执行loader的顺序是从右到左。

此时再执行`npx webpack`就可以成功打包了。

### 1.5.2 处理less资源

1. 下载包

```javascript
npm i less-loader -D
```

2. 功能介绍

`less-loader`：负责将less文件编译为css文件

3. 使用

```javascript
// 加载器
    module: {
        rules: [
            // loader的配置
            {
                test: /\.css$/, // 匹配以.css结尾的文件
                use: ["style-loader", "css-loader"], // 上面符合条件的文件，使用这些loader处理
            },
            {
                test: /\.less$/,
                use: ["style-loader", "css-loader", "less-loader"]
            }
        ]
    },
```

虽然我们之前配置过了关于css文件loader，但是我们这一次匹配的是以`.less`结尾的文件，它经过`less-loader`处理之后就会变为css文件，所以还是需要配置一遍css有关的loader。

### 1.5.3 处理scss sass资源

1. 下载包

```javascript
npm i sass sass-loader -D
```

2. 功能介绍

`sass-lodaer`:负责将sass或者scss文件编译为css文件

3. 使用：

```javascript
module: {
        rules: [
            // loader的配置
            {
                test: /\.s[ac]ss$/,
                use: ["style-loader", "css-loader", "sass-loader"],
            }
        ]
    },
```

### 1.5.4 处理styl资源

1. 下载包

```javascript
npm i stylus-loader -D
```

2. 功能介绍

`stylus-loader`：负责将styl文件编译为css文件

3. 使用：

```javascript
module: {
        rules: [
            // loader的配置
            {
                test: /\.s[ac]ss$/,
                use: ["style-loader", "css-loader", "stylus-loader"],
            }
        ]
    },
```

## 1.6 处理图片资源

在webpack4的时候，我们处理图片资源需要用到`file-loader`和`url-loader`进行处理

现在webpack5已经将这两个loader进行了内置，所以我们只需要进行一些配置就可以使用了。

### 1.6.1 配置

在没有配置的时候，图片资源同样可以打包，但是这个它没有进行优化，比如转为base64格式，我们可以通过配置开启这个选项。

```javascript
module: {
        rules: [
            // loader的配置
            {
                test: /\.(png|jpe?g|gif|webp|svg)$/,
                type: "asset",
                parser: {
                    dataUrlCondition: {
                        // 小于10kb就转为base64
                        maxSize: 10 * 1024,
                    }
                }
            }
        ]
    },
```

## 1.7 修改打包资源之后的存放路径

