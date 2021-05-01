# webpack 是干嘛的

转译代码 （es6-> es5 scss ->csS）

构建 build

代码压缩

代码分析

# 安装

webpack-dev-server 用于本地预览

```
npm i -g webpack@4.41.2 webpack-cli@3.3.10

npm i -g webpack-dev-server@3.9.0


yarn add  webpack@4.41.2 webpack-cli@3.3.10 --dev

yarn add webpack-dev-server@3.9.0 --dev



```

**调用本地安装的 webpack**

```
<!-- 手动确认路径 -->
./node_modules/.bin/webpack --version

<!-- 自动寻找路径  -->
npx webpack
<!-- 但是windows  node 安装路径 不能有空格 （貌似）-->
```

# 1. 转义 JS

x.js

```javascript
export default "xxx";
```

index.js

```javascript
import x from "./x.js";
console.log(x);
```

运行 npx webpack

出去兼容代码 结果变成

```javascript
console.log("xxx")}
```

webpack 会分析 js 代码，然后把 js 代码变成 ie or 低版本浏览器可以用的 js

## 解决警告 初始化 webpack.config.js

```javascript
WARNING in configuration
The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
You can also set it to 'none' to disable any default behavior. Learn more: https://webpack.js.org/configuration/mode/

```

没有设置 模式 mode

Set 'mode' option to 'development' or 'production'

# 2.理解文件名中 hash 的用途

hash 的用途多便于添加缓存，这个缓存是 http 协议里规定的，浏览器必须支持

http 响应头 Cache-Control 可以设置缓存

`cache-control: max-age=315360000`

缓存可以让浏览器从内存或磁盘中读取文件。加快访问速度

如果文件内容变了咋办

Http 缓存是跟着文件命走的

改一下文件名就好了

如何改文件名？
只要对文件内容做一个 hash or md5
webpack 会自动做这件事情

注意首页不能缓存，首页缓存了咋知道引用的 css js 变了

# 3 webpack 插件自动生成 html

## 先写个 build 脚本

```
"scripts": {
"build": "rm -rf dist; npx webpack",
},


```

npx 可以不用写 因为默认会自己去找 webpack

```
"scripts": {
"build": "rm -rf dist;  webpack",
},

```

运行

```
yarn build

npm run build
```

## 自动生成 html 需要使用插件

[HtmlWebpackPlugin](https://webpack.js.org/plugins/html-webpack-plugin/)

```
npm install --save-dev html-webpack-plugin

```

## 如何自动改文件名

```javascript
 output: {
        filename: '[name].[contenthash].js',
    },
```

## 使用模板 去生成 html

```javascript
const HtmlWebpackPlugin = require("html-webpack-plugin");


  plugins: [new HtmlWebpackPlugin({
        title:"my app",
        template: "src/assets/index.html"

    })],
```

## 使用配置的 title

`<title><%= htmlWebpackPlugin.options.title %></title>`

src/assets/index.html

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0"
    />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
```

# webpack 引用 css

yarn add css-loader --dev

yarn add style-loader --dev

[正则表达式 30 分钟入门](https://deerchao.cn/tutorials/regex/regex.htm)

```javascript
module.exports = {
  //  ....
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

这段 webpack 配置意思是：如果发现任何以.css 结尾的文件名，就用 css-loader 去处理这个文件

css-loader 的作用：把 css 文件的内容读到 js 里

style-loader 的作用：把 css-loader 读到的东西变成 style 标签放到 head 里

# webpack-dev-server

yarn add webpack-dev-server --dev

webpack.config.js

```

mode: 'development',


devtool: 'inline-source-map',

devServer: {
    contentBase: './dist',
  },


```

package.json

```json
  "scripts": {
    "build": "rm -rf dist&&npx webpack",
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack serve --open"
  },

```

webpack-dev-server 不会生成 dist
直接在内存中就搞定了

# 使用插件提取 css 文件

关键词 extract 提取

css-loader、style-loader

用 js 生成 style 标签添加到 head 里

如果想把 css 抽成文件，需要用
可以搜 webpack css extract plugin

使用 MiniCssExtractPlugin 把 css 抽成文件

npm install --save-dev mini-css-extract-plugin

```javascript
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  plugins: [new MiniCssExtractPlugin()],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
    ],
  },
};
```

对生成的 css **文件名**加上内容的 hash 值

```javascript
plugins: [new MiniCssExtractPlugin(
      filename: '[name].[contenthash].css',
            chunkFilename: '[id].[contenthash].css',
)],

```

webpack config merge 工具也能合并 webpack 配置

# webpack loader plugin difference

https://stackoverflow.com/questions/37452402/webpack-loaders-vs-plugins-whats-the-difference

Loaders do the pre-processing transformation of virtually any file format when you use sth like require("my-loader!./my-awesome-module") in your code. Compared to plugins, they are quite simple as they (a) expose only one single function to webpack and (b) are not able to influence the actual build process.

Plugins on the other hand can deeply integrate into webpack because they can register hooks within webpacks build system and access (and modify) the compiler, and how it works, as well as the compilation. Therefore, they are more powerful, but also harder to maintain.

loader 会对任何文件格式进行预处理转换。 与插件相比，它们非常简单，
因为

1. 它们在 webpack 中仅是单一功能
2. 它们无法影响实际的构建过程

plugins： 插件可以深入集成到 webpack 中，因为它们可以在 webpack 构建系统中注册钩子（hooks），访问、修改编译器 ，如何更好的完成编译。
因此，它们功能更强大，但也更难以维护。
