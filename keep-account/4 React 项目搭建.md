# 项目搭建

yarn config set registry https://registry.npm.taobao.org/

create-react-app 简写为 cra

cra 官网
https://create-react-app.dev

yarn global add create-react-app@3.4.1

create-react-app . --template typescript

不喜欢自动，就运行 echo 'BROWSER=none' > .env 再 yarn start

# 目录说明

![目录说明](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7938bb7a5bbd4da88573e89cc00b52cd~tplv-k3u1fbpfcp-watermark.image)

Mark Directory as -> Not Excluded 排除文件搜索

.jsx -支持标签的 TS
.ts -TypeScript
.tsx -支持标签的 TS

```jsx
// 检查代码错误、过时的标签
<React.StrictMode></React.StrictMode>
```

idnex.html 首页

index.tsx 入口文件
tsconfig.json : ts 配置文件

react-app-env.d.ts :ts 类型生命 注释别删

# css normalize vs css reset

**css normalize 有什么用？**

保证页面在不同浏览器上默认样式基本一样（相近）

**css normalize 和 css reset 有什么区别 ？**

css normalize 保证页面在不同浏览器上默认样式基本一样

css reset 把默认样式全部重置

在 index.css 添加 @import-normalize; 即可

webstorm Edit inspection profile setting 可以修改警告

# 支持 SCSS

`yarn add node-sass@npm:dart-sass` 安装 node-sass，但实际内容用 dart-sass 的内容替换

rename src/App.css to src/App.scss

最难的技术问题 SCSS

- 我想让 React 应用支持 sass
- 然后去安装 node-sass. 但是有缺点：下载慢(github 下载 非 npm) 本地编译慢
- 然后我想用 dart-sass 代替 node-sass
- 但是 React 值支持 node-sass 不吃支持 dart-sass
- 我经过经过搜素和研究
- 发现 npm 6.9 支持新功能叫 package alias
- npm install node-sass@npm:dart-sass 安装 dart-sass 别名是 node-sass 这样就完成了

# import 和 JS import 优化

Vue 中使用@表示 src/目录

## import css

```scss
// react 16.13.1 支持
@import "yyy/yyy.scss";

// 等价于
@import "src/yyy/yyy.scss";

// react 17.0.2 不支持
@import "yyy/yyy.scss";

// 需要手写
@import "src/yyy/yyy.scss";
```

## import js

[参考](https://create-react-app.dev/docs/importing-a-component#absolute-imports)

jsconfig or tsconfig add

```json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "include": ["src"]
}
```

**项目目录不要有空格，会影响 js 引入**

## helper.scss

存放变量 函数等公用

`@import 'helper'` 引用

# css-in-js style-components

```js
// 模板字符串
tag`string text ${expression} string text`;
```

`yarn add styled-components@5.0.1`
`yarn add --dev @types/styled-components@5.0.1`

webstorm plugin `styled components`

# React Router

`npm install react-router-dom`

`npm i --save-dev @types/react-router-dom`

## HashRouter

没有后台服务器只能用 hash 模式

有后台服务器，配置所有路径都到首页才能 用 History

```jsx
    BrowserRouter as Router,
    // 改为
    HashRouter as Router,
```

# 中文字体最佳实践

[github 地址](https://github.com/zenozeng/fonts.css/)

# svg symbols

`yarn eject` 永久添加项目配置文件

使用 svg-sprite-loader 加载 svg

约定：Webpack TypeScript 相关的东西 都 --dev

`npm install svg-sprite-loader -D`
or
`yarn add svg-sprite-loader -D`

```jsx
require("icons/money.svg");
<!-- or -->
import money from 'icons/money.svg'

<svg className="icon">
    <use xlinkHref="#money" />
</svg>

```

TreeShaking:如果没有用到的代码（没用的叶子），会从依赖树上震动（删除）下来

TreeShaking 不适用 require

```jsx
// 批量引入
let importAll = (requireContext: __WebpackModuleApi.RequireContext) =>
  requireContext.keys().forEach(requireContext);
try {
  importAll(require.context("icons", true, /\.svg$/));
} catch (error) {
  console.log(error);
}


// 需要安装
yarn add --dev @types/webpack-env
```
