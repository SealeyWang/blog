# 面试题

## 下面代码结果是什么  为什么
```javascript

 ['1','2','3'].map(parseInt)// [1,NaN,NaN]

// 因为 等价于


 ['1','2','3'].map((currentValue,index,array)=>{
     //parseInt('1',0,array) 
     //parseInt('2',1,array) 
     //parseInt('3',2,array) 
    return  parseInt(currentValue,index,array) 
 })

//  parseInt(string, radix)  
```

## 为啥要用promise

1. 使用promise更规范，
自己封装不规范不强，有可能是回调函数接收两个参数（error ,data），或者是封Z装的异步方法传两个回调参数（一个success 一个fail），新人进来，不熟悉每次都得翻着看封装

2. 容易出现回调地狱 ，代码不够清晰
3. 很难进行错误处理



```javascript 
// 熟记下面的用法
return new  Promise((resolve,reject)=>{})

resolve(result)
reject(error)
// resolve reject 会再去调用成功和失败函数
```1