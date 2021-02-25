
JavaScript 有8种 7中基本数据类型  和 Object。
本文只介绍基本数据类型

# 基本数据类型

1. Number
2. String
3. Boolean
4. Null
5. Undefind
6. Symbol
7. BigInt



# Number 
JavaScript Number是64位双精度浮点类型


[64位双精度](https://en.wikipedia.org/wiki/Double-precision_floating-point_format)

sign 符号位:1位，  符号位确定数字的符号（+ - ）

exponent（指数位）:11位，指数位也有1位表示±  所以指数范围是−1023~1024

−1023（全0）和+1024（全1）的指数是保留给特殊数字的。

fraction（有效数字） 显示存储52位，有效精度53位（省掉第一个1）



![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba085e61b315498682d6e4ab145c95b9~tplv-k3u1fbpfcp-watermark.image)

范围  

指数最大 指数符号是+  有效数字最大全部写成1 对应：

Number.MAX_VALUE:1.7976931348623157e+308


指数最大 指数符号是- 有效数字只有一个1 对应：

Number.MIN_VALUE:5e-324

精度

最多2^53次方个1 
2^53 = 9007199254740992   (9后面15位数) 

十进制15位数可精确表示

十进制16位不能超过2^53。对于从2^53到2^54的下一个范围，所有值都乘以2，因此可表示的数字为偶数。这样就丢失精度了

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b03736c9e631448db0609d19c0c6ed7a~tplv-k3u1fbpfcp-watermark.image)


# String


# Boolean

只有两个值，true false

下列运算会返回Boolean值

相等运算
1==2(false)  1!=2(true)  3===4（false）  3!==4(true)

比较运算 
1>2 (false)  1>=2(false) 3<4(true)  3<=4(false)

否定运算
!value 
常见有

```javascript
let a = false;
if(!a){
    // ....
}

```
如果是 否定运算的对象是Number String Object 咋办

falsy 值 (虚值) 是在 Boolean 上下文中认定为 false 的值。
JavaScript 在需要用到布尔类型值的上下文中使用强制类型转换(Type Conversion )将值转换为布尔值。 if() while()


false undefind null 0 NaN ''  

空字符串有 '' ""  ``

0 有 -0  0n(BigInt的0)

```javascript

if (false)
if (null)
if (undefined)
if (0)
if (0n)
if (NaN)
if ('')
if ("")
if (``)
```

除了这些， 其他的都是 真值。 被认定为true


# Null 和  Undefind

都表示空，没有本质区别

* 声明变量没有赋值，默认是undefind，不是null
* 函数没写return 默认return undefind 
* 前端**习惯（仅仅是习惯）** 把非对象的空值写为undefind。对象的空值写为null


# Symbol

**Symbol 生成一个全局唯一的值。**


```javascript
// Symbol 可以创建一个独一无二的值（但并不是字符串）。
var race = {
  protoss: Symbol(),
  terran: Symbol(),
  zerg: Symbol()
}

race.protoss !== race.terran // true
race.protoss !== race.zerg // true

//给Symbol 起名  
var race = {
  protoss: Symbol('protoss'),
  terran: Symbol('terran'),
  zerg: Symbol('zerg')
}

// 名字与值无关  相当于注释
var a1 = Symbol('a')
var a2 = Symbol('a')
a1 !== a2 // true
```

# BigInt









