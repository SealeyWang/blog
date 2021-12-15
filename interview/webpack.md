# Webpack

## Webpack 有哪些常见 loader 和 plugin，你用过哪些？

**常见的 loader**

- file-loader 把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
- url-loader 和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
- source-map-load 不熟别说 加载额外的 Source Map 文件，以方便断点调试
- image-loader 加载并且压缩图片文件
- babel-loader 把 ES6 转换成 ES5
- css-loader 加载 CSS，支持模块化、压缩、文件导入等特性
- style-loader 把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
- eslint-loader 通过 ESLint 检查 JavaScript 代码

**常见的 plugin**

- HtmlWebpackPlugin 用来生成 html 文件
- MiniCssExtractPlugin 用来把多个 css 合并成一个 css
- babel-plugin-import 按需加载

## Webpack loader 和 plugin 的区别是什么？

loader 是加载器 ,用于加载文件。让 webpack 拥有加载和解析非 JavaScript 文件的能力

因为 webpack 把所有文件视为模块，原生 webpack 只能解析 JS 文件，别的文件想打包就需要用到 loader

plugin 是插件。

是用来加强 webpack 功能的，比如说 HtmlWebpackPlugin 插件可以生成 html
文件，MiniCssExtractPlugin 用来把 css 合并成一个 css 文件的
套路

### 用法不同

**loader 在 module.rules 中配置，作为模块的解析规则**而存在。

类型为数组，每一项都是一个 Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）

**plugin 在 plugins 中单独配置**

类型为数组，每一项是一个 plugin 的实例，参数都通过构造函数传入。

## webpack 如何按需加载代码？

babel-plugin-import 按需加载

安装`npm i babel-plugin-import -D`

在.babelrc || babel.config.js 文件 配置

```javascript
{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [

    [
        'import',
        {
        libraryName: 'vant',
        libraryDirectory: 'es',
        style: true
        },
        'vant'
    ],
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]

  ]
}
```

## webpack 如何提高构建速度？

- 多入口情况下，使用 CommonsChunkPlugin 来提取公共代码
- 通过 externals 配置来提取常用库
- 利用 DllPlugin 和 DllReferencePlugin 预编译资源模块 通过 DllPlugin 来对那些我们引用但是绝对不会修改的 npm 包来进行预编译，再通过 DllReferencePlugin 将预编译的模块加载进来。
- 使用 Happypack 实现多线程加速编译
- 使用 webpack-uglify-parallel 来提升 uglifyPlugin 的压缩速度。 原理上 webpack-uglify-parallel 采用了多核并行压缩来提升压缩速度
- 使用 Tree-shaking 和 Scope Hoisting 来剔除多余代码

## webpack 怎么配置单页应用？怎么配置多页应用？

单页应用可以理解为 webpack 的标准模式，直接在 entry 中指定单页应用的入口即可.

多页应用的话，可以使用 webpack 的 AutoWebPlugin 来完成简单自动化的构建，但是前提是项目的目录结构必须遵守他预设的规范。 多页应用中要注意的是：

- 每个页面都有公共的代码，可以将这些代码抽离出来，避免重复的加载。比如，每个页面都引用了同一套 css 样式表
- 随着业务的不断扩展，页面可能会不断的追加，所以一定要让入口的配置足够灵活，避免每次添加新页面还需要修改构建配置
