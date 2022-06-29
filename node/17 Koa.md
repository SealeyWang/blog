
都是TJ写的web框架, 封装了请求和相应

# koa 与 express 不相同之处
1. 
express 使用的是回调， 容易陷入回调地狱

koa 使用的是 async/await 
依赖 node v7.6.0 或 ES2015及更高版本和 async 方法支持.

2. 
express 中间件是线性模型， 执行顺序从开始 结束

koa 中间件是洋葱模型/U型 ，执行顺序从 外到里 再到外  


3. 
express 做的更多，更接近web框架  有一些内置中间件 比如router

koa 没有内置中间件，全部要自己去选择, 几乎所有功能都要靠中间件完成


它们api 都差不多 不过 koa 把request, response挂都context上



- 

```js
import Koa from 'koa';

const app = new Koa();

app.use(async (ctx,next) => {
    ctx.body = 'hello'
    await next()
    ctx.body += 'wsl'
    // ctx.request
    // ctx.response
})


app.use(async (ctx, next) => {
    ctx.body = ctx.body +='world'
    await next()
})

app.use( async (ctx, next)=> {
    ctx.set('Content-Type', 'text/html')
    await next()
})

app.listen(3000, () => {
    console.log('监听3000 端口')
})



```


