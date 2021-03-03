
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

[ASCII](https://zh.wikipedia.org/wiki/ASCII)

[字符编码笔记：ASCII，Unicode 和 UTF-8](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)

[String](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)

字符存储用编号表示，下图是ASCII 编码对应表


![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d95d298d81a64c04a73c49bc729d899c~tplv-k3u1fbpfcp-watermark.image)

ASCII 码一共规定了128个字符的编码（包括32个不能打印出来的控制符号），只占用了一个字节的后面7位，最前面的一位统一规定为0。


* 0 用48 表示 
* A 用65 表示 
* a 用 97表示

如何表示汉字

GB2312（国标2312） 是中国国家标准局推出的中文 收录常用汉字、英文字母 日文假名。然而很多生僻字显示不了

微软推出GBK(国标扩) ，可以显示生僻字 兼容GB2312

后来为了解决全世界的文字推出了[Unicode(万国码)](https://en.wikipedia.org/wiki/Unicode) 

Unicode 收录13万字符（大于16位），全世界通用。Unicode 只是一个符号集，它只规定了符号的二进制代码，却没有规定这个二进制代码应该如何存储。


UTF-8 是Unicode的一种实现方式 推出UTF-8，-8意思是最少可用8位
存一个字符。UTF-8在存储时可以存1到3个字节（通过第一个字节的信息，获取当前字符还有多少字节）

UTF-8规则 
1. 对于单字节的符号，字节的第一位设为0，后面7位为这个符号的 Unicode 码。因此对于英语字母，UTF-8 编码和 ASCII 码是相同的。
2. n个字节 第一个字节前n位为1 n+1位为0 ，后面字节的前两位一律设为10 



JavaScript使用阉割版的UFT-8,每个字符两个字节

```javascript 
'字符串' "字符串" `字符串`


//`` 是模板字面量


//使用 String 函数将其他值生成或转换成字符串

String(thing)
new String(thing)
// thing
任何可以被转换成字符串的值。



//字符串转义
//  \'   表示'
// \" 表示"
// \n 表示换行
// \r 表示回车
// \t 表示tab 制表符
// \\ 表示 \
// \uFFFF 表示对应的Unicode字符
// \xFF 表示钱256个Unicode字符


//通过是模板字面量，输入多行字符串

let s=`用模板字面量
可以轻松做到
多行输入
`
// 没有`` 的时候 多行是用+ 或/  

var longString = 'Long '
  + 'long '
  + 'long '
  + 'string';


var longString = 'Long \
long \
long \
string';

```

```javascript 
'123'.length  // 3
'\n\r\t'.length //3   转义后只有3个字符
''.length //0 空字符串length 是0
' '.length // 1 空格也是字符


// 通过下标读取字符串
let s = 'hello'
s[0] // h 
s[5] // undefind 不报错

```

## base64转码



```javascript
// 字符串转为Base64编码的字符串
window.btoa('wangshaoli18871074717@gmail.com')
// "d2FuZ3NoYW9saTE4ODcxMDc0NzE3QGdtYWlsLmNvbQ=="
// Base64编码的字符串转为原来的字符串
window.a('d2FuZ3NoYW9saTE4ODcxMDc0NzE3QGdtYWlsLmNvbQ==')// "wangshaoli18871074717@gmail.com"

```




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

是一种内置对象，它提供了一种方法来表示大于 253 - 1 的整数（这是Number表示的最大数字）。 BigInt 可以表示任意大的整数。

```javascript
//在一个整数字面量后面加 n 的方式定义一个 BigInt
const theBiggestInt = 9007199254740991n;

//调用BitInt 函数
const alsoHuge = BigInt(9007199254740991);
// ↪ 9007199254740991n

// 接收16进制数
const hugeHex = BigInt("0x1fffffffffffff");
// ↪ 9007199254740991n



// 接收二进制数
const hugeBin = BigInt("0b11111111111111111111111111111111111111111111111111111");
// ↪ 9007199254740991n


```
使用BitInt 和 Number 需要注意的地方
* 不能用于 Math 对象中的方法
* 不能和任何 Number 实例混合运算，两者必须转换成同一种类型。（BigInt 变量在转换成 Number 变量时可能会丢失精度。）





