环境 nodejs 14.21.1 npm 6.14.17 shell 命令 挂代理 淘宝源

第一个坑

```shell
npm i -g @nestjs/cli
nest new project-name
```

创建项目后安装失败`npm i ` `yarn install `，都试了

后来尝试 ▹▹▹▹▹ Installation in progress... ☕ 自动安装时 ctrl+c 取消， 手动进入目录`npm i `

第二个坑 启动报错

```shell
λ npm run start

> first-project@0.0.1 start D:\6.xiedaimala\9.nestjs\first-project
> nest start

D:\6.xiedaimala\9.nestjs\first-project\node_modules\@nestjs\common\file-stream\streamable-file.js:31
            this.options.length ??= bufferOrReadStream.length;
                                ^^^

SyntaxError: Unexpected token '??='
    at wrapSafe (internal/modules/cjs/loader.js:1001:16)
    at Module._compile (internal/modules/cjs/loader.js:1049:27)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1114:10)
    at Module.load (internal/modules/cjs/loader.js:950:32)
    at Function.Module._load (internal/modules/cjs/loader.js:790:12)
    at Module.require (internal/modules/cjs/loader.js:974:19)
    at require (internal/modules/cjs/helpers.js:101:18)
    at Object.<anonymous> (D:\6.xiedaimala\9.nestjs\first-project\node_modules\@nestjs\common\file-stream\index.js:4:22)
    at Module._compile (internal/modules/cjs/loader.js:1085:14)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1114:10)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! first-project@0.0.1 start: `nest start`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the first-project@0.0.1 start script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\11255\AppData\Roaming\npm-cache\_logs\2023-06-29T03_23_18_756Z-debug.log
```

Node.js 版本升级到 16.6.0 或更高版本， 从 Node.js 16.6.0 开始，空赋值运算符 ??= 是被支持的。

```shell
# 我这里升级为
node -v
v18.16.1

npm -v
9.5.1
```

# 使用 SWC（Speedy Web Compiler）

SWC 比默认的 TypeScript 编译器 快大约 20 倍。详细参考 https://docs.nestjs.com/recipes/swc

```shell
npm i --save-dev @swc/cli @swc/core

```

启动命令为：`npm run start -- -b swc`

要监视文件中的更改，使用命令 `npm run start:dev`

# CRUD 生成器

前置条件 `npm i -g @nestjs/cli`, 已安装过的可忽略
`nest g resource [name]`
