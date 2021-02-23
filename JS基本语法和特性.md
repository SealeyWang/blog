# 表达式和语句

* 表达式
* 语句
* 值
* 返回值 （函数才有返回值）

```
2+3 是表达式 值为5

add(1,2)是表达式 值为函数的返回值

console.log 是表达式 值是函数本身


console.log(2)是表达式 值undefind 应为log 函数没有返回值。JS默认给undefind

```

`var a=1 //是一个语句`





表达式一般有值，语句可能有，可能没有

语句一般会改变环境（声明，赋值）

以上不绝对



return 后面不能加回车，  加回车 会返回undefind （JS就这么设计的）

```javascript

function f1(){
    return 
    1
}
let a = f1(); 
console.log(a)// a 是 undefind 

```

# 标识符规则

* 第一个字符，可以是Unicode字母 $ _ 中文
* 后面的字符除了上面的  还可以是数字

# if else 语句

```
if(表达式){语句}else｛语句2｝
```

特殊情况

```javascript
var a = 2 
if(a = 1) { // 表达式a =1 的值 是1 
//走的是这里
    console.log(1) //  打印的是1 
}else {
    console.log(2)
}

```

if else 简写成问号冒号表达式 (三元表达式)

```javascript
  a = 1? console.log(1): console.log(2)  
```

短路逻辑 && (and)  || (短路或)

A && B && C && D 取第一个假值 或 D

A || B || C || D 取第一个真值或D 

```javascript
if(window.f1) {
    console.log('f1存在')
}
// 简写
window.f1  && console.log('f1存在')



a = b || 100  // a 是真值 a =b  否则a=100
```

# while for 

```javascript
var a = 0.1 
while(a!==1) {
    console.log(1)
    a = a+0.1 // 由于浮点数不精确 a 是不会等于1的 会卡一直循环 卡死程序
}

for (var i = 0; i<5 ; i++ ){
    setTimeout(()=>{
        console.log(i) // 会打印5个5 是延迟执行  i=5 跳出循环后 才打印的 
    })
}


for (let i = 0; i<5 ; i++ ){
    setTimeout(()=>{
        console.log(i) // 会打印0 1 2 3 4 let JS单独为let 设计了逻辑
    })
}

// break 退出所有循环 continue 退出当前循环
//只会作用最近的for 

```

# label

```javascript
foo : {
    console.log(1);
    break foo;
    console.log('不会输出')
}
console.log(2)
//上面代码会打印1 2

// foo不是一个对象  是一个 label 语句 

// 面试  这是啥
{
    foo :1 
}
// 一个label 语句 值是1
```

