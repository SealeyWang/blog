
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


[MIME（Multipurpose Internet Mail Extensions）"多用途互联网邮件扩展"](http://www.ruanyifeng.com/blog/2008/06/mime.html)

[Base64](http://www.ruanyifeng.com/blog/2008/06/base64.html)

早期电子邮件的传统格式不支持非ASCII编码和二进制数据，所以规定了内容传输编码 Content-transfer-encoding  可选值有"7bit"、"8bit"、"binary"、"quoted-printable"和"base64"----其中"7bit"是缺省值，即不用转化的ASCII字符


base64是编码转换方式中的一种。选出64个字符----小写字母a-z、大写字母A-Z、数字0-9、符号"+"、"/"（再加上作为垫字的"="，实际上是65个字符）----作为一个基本字符集。然后，其他所有符号都转换成这个字符集中的字符。

转换步骤

 1. 将每三个字节作为一组，一共是24个二进制位。
 2. 将这24个二进制位分为四组，每个组有6个二进制位。
 3. 在每组前面加两个00，扩展成32个二进制位，即四个字节。
 4. 根据下表，得到扩展后的每个字节的对应符号，这就是Base64的编码值。

![base64编码](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/75b90a7ee9c94fd59f4b157e713491a8~tplv-k3u1fbpfcp-watermark.image)


Base64将三个字节转化成四个字节，因此Base64编码后的文本，会比原文本大出三分之一左右。

两个字节处理方式 16位 分成三组（  6位 6位 4位）6位前面补2个0  4位前后各补2个0 转成base64编码后补一个 =

一个字节处理方式 8位 分成2组（6位  2位） 6位前面加2个0，2位前面加2个0后面加4个0。转成base64编码后补一个 =



```javascript
// 字符串转为Base64编码的字符串
window.btoa('wangshaoli18871074717@gmail.com')
// "d2FuZ3NoYW9saTE4ODcxMDc0NzE3QGdtYWlsLmNvbQ1"
// Base64编码的字符串转为原来的字符串
window.atob('d2FuZ3NoYW9saTE4ODcxMDc0NzE3QGdtYWlsLmNvbQ==')// "wangshaoli18871074717@gmail.com"

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


## falsy 值

falsy 值 (虚值) 是在 Boolean 上下文中认定为 false 的值。
JavaScript 在需要用到布尔类型值的上下文中使用强制类型转换(Type Conversion )将值转换为布尔值。 if() while()


1. undefind 
2. null 
3. 0 
4. NaN 
5. ''  

空字符串有 '' ""  ``

0 有 -0  0n(BigInt的0)

他们都被认为是false

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



# 数据类型转换 

[数据类型转换 ](https://www.jianshu.com/p/ca95ab518635)


## 转换为string


### 使用toString 

```javascript
// 1. number
var n = 100
n.toString() // "100"
// 2. boolean
var b = true
b.toString() // "true"
// 3. null
null.toString() // 报错：Cannot read property 'toString' of null
// 4. undefined
undefined.toString() // 报错：Cannot read property 'toString' of undefined
// 5. object
var obj = {name: 'xxx'}
obj.toString() // "[object Object]" 无法得到想要的结果


```

```javascript

//console.log 等函数实际上就是使用了 toString，如果发现不是字符串，会自动调用 toString 这个API来转换成字符串
console.log(100) 
// 实际上是
console.log((100).toString())
obj[1]
// 实际上是
obj[(1).toString()]
```

### 通过 + ''  转换成String
```javascript


// 1. number
var n = 100
n + '' // "100"
// 2. boolean
var b = true
b + '' // "true"
// 3. null
null + '' // "null"
// 4. undefined
undefined + '' // "undefined"
// 5. object
var obj = {name: 'xxx'}
obj + '' // "[object Object]"

//  + 的左右两边如果发现任意一边有字符串，它就会尝试把另外一边也变成字符串
1 + "1" // "11"
```


### 通过全局函数 String   转换成String类型

```javascript
// 1. number
var n = 100
String(n) // "100"
// 2. boolean
var b = true
String(b) // "true"
// 3. null
String(null) // "null"
// 4. undefined
String(undefined) // "undefined"
// 5. object
var obj = {name: 'xxx'}
String(obj) // "[object Object]"


```


## 转化为boolean


### 通过全局函数 Boolean
```javascript
// 5 个falsy 是false  除此之外别的都是true
Boolean(0) // false
Boolean(null) // false
Boolean(undefined)//false
Boolean('')//false
Boolean(NaN)// false 


//number
Boolean(1) // true

//string
Boolean('1') // true
Boolean('0') // true

//object
var obj = {name: 'xxx'}
Boolean(obj) // true
Boolean([]) // true
Boolean({}) // true 
```


### 通过 !(逻辑非) 运算符

```javascript
// 5个falsy 值
!!0 //false
!!NaN //false
!!null //false
!!undefined //false
!!'' //false


//number
!!1 // true

//string
!!'1' // true
!!'0' // true

//object
var obj = {name: 'xxx'}
!!obj // true
!![] // true
!!{} // true 

```

## 转换为number


### 通过全局函数Number


[科学计数法](https://zh.wikipedia.org/wiki/%E7%A7%91%E5%AD%A6%E8%AE%B0%E6%95%B0%E6%B3%95)
在电脑或计算器中一般用EXP或E（Exponential）来表示10的幂：

7.823E5=782300
1.2e−4=0.00012



```javascript 
// 1. number
Number(1) // 1
Number(NaN) // NaN

// 2. string
Number('1') // 1
Number('NaN') // NaN
Number() // 0
Number('') // 0
Number('123abc') // NaN
Number('0.1') // 0.1
Number('1.2e+2') // 123
Number(' 123  ') // 123
Number(' 12 3  ') // NaN

// 3. boolean
Number(true) // 1
Number(false) // 0


// 4. null
Number(null) // 0

// 5. undefined
Number(undefined) // NaN


// 6. object
Number({}) // NaN
Number([]) // 0
Number([1,2]) // NaN
Number([1]) // 1
Number(['1']) // 1
Number([true])// NaN
Number([undefined]) // 0
Number([null]) // 0
Number(['']) // 0
```

### 通过全局函数[parseInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)



转换为整数，parseInt 的参数都是字符串，如果不是则会先被转化成字符串



```javascript
// 1. number
parseInt(1) // 1
parseInt(NaN) // NaN
parseInt(Infinity) // NaN
parseInt(-1.23) // -1
parseInt(0x100) // 256，里面参数都是字符串，等同于 parseInt(String(0x100))

// 2. string
parseInt('1') // 1
parseInt('NaN') // NaN
parseInt('') // NaN
parseInt('123abc') // 123
parseInt('0.1') // 0
parseInt('1.2e+2') // 1 科学计数法视为字符串
parseInt(0.0000000000001) // 1 科学计数法视为字符串
parseInt(10000.5) // 1000
parseInt(' 123  ') // 123
parseInt(' 12 3  ') // 12
parseInt('+1') // 1
parseInt('.1') // NaN
parseInt('0x100') // 256，0x或者0X开头按照16进制解析
parseInt('0100') // 100，0开头仍然按照10进制解析
parseInt('011') // 11，0开头的数字不当做8进制，忽略0
// 字符串只转换为头部可以转换为数字的部分，不然返回NaN


// 3. boolean
parseInt(true) // NaN
parseInt(false) // NaN

// 4. null
parseInt(null) // NaN

// 5. undefined
parseInt(undefined) // NaN

// 6. object
parseInt({}) // NaN
parseInt([]) // NaN
parseInt([1,2]) // 1
parseInt([1]) // 1
parseInt(['1']) // 1
parseInt([true]) // NaN
parseInt([undefined]) // NaN
parseInt([null]) // NaN
parseInt(['']) // NaN

```

`parseInt(string, radix);`

string 要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用 [ToString](https://262.ecma-international.org/6.0/#sec-tostring)  抽象操作)。字符串开头的空白符将会被忽略。

radix 从 2 到 36，表示字符串的基数。例如指定 16 表示被解析值是十六进制数。请注意，10不是默认值！

radix取值范围为2-36的整数，超出这个范围的整数，则返NaN。如果不是数值或者整数，会被自动转化成一个整数，如果是 
0，null，undefined，NaN 和 Infinity

```javascript

//radix 默认值不是 10 

// 以0x 0X 开头 radix被假定为16   0x 后面的部分当做16进制数解析
parseInt('0x10') // 16  

// radix被假定为8（八进制）或10（十进制）。具体选择哪一个radix取决于实现。ECMAScript 5 规定应该使用 10 (十进制) 但不是所有的浏览器都支持。因此，在使用 parseInt 时，一定要指定一个 radix。
parseInt('010') // 10   IE chorme firefox 都是10 

//如果输入的 string 以任何其他值开头， radix 是 10 (十进制)。
parseInt('100') // 100

// 如果第一个字符不能转换为数字，parseInt会返回 NaN。
parseInt('A1') // NaN  10进制解析不了A
parseInt('A1',16) //161  16进制才能解析A


//参数范围为2-36的整数，超出这个范围的整数, 则返NaN
parseInt('100', 40)// NaN  
parseInt('100', 1) // NaN

// 如果不是数值或者整数，会被自动转化成一个整数
parseInt('100', 2.8) // 4  
parseInt('100', '2.8') // 4   

// 如果是 0，null，undefined，NaN 和 Infinity，则radix默认为10。
parseInt('100', 0) // 100
parseInt('100', null) // 100
parseInt('100', undefined) // 100
parseInt('100', NaN) // 100
parseInt('100', Infinity) // 100

```


### 全局函数 parseFloat

```javascript

parseFloat('1.23') // 1.23
parseFloat('123e-2') // 1.23 可正确转换科学计数法
parseFloat('1.23abc') // 1.23
parseFloat('abc1.23') // NaN
parseFloat (' 1.23 ') // 1.23
parseFloat (' 1.23 4.56') // 1.23
```


### 快捷的方法

#### -0 
```javascript

'1.23' - 0 // 1.23
'a123' - 0 // NaN
'123a' - 0 // NaN
' 123 ' - 0 // 123
'Infinity' - 0 // Infinity
true - 0 // 1
false - 0 // 0
null - 0 // 0
undefined - 0 // NaN
[1] - 0 // 1
[1,2] - 0 // NaN

```

#### + 
```javascript
+ '1.23' // 1.23
+ 'a123' // NaN
+ '123a' // NaN
+ ' 123 ' // 123
+ 'Infinity' // Infinity
+ true // 1
+ false // 0
+ null // 0
+ undefined // NaN
+ [1] // 1
+ [1,2] // NaN
```

#### Number.toString

[Number.prototype.toString()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toString)

`numObj.toString([radix])`


radix
指定要用于数字到字符串的转换的基数(从2到36)。如果未指定 radix 参数，则默认值为 10。

如果对象是负数，则会保留负号。即使radix是2时也是如此：返回的字符串包含一个负号（-）前缀和正数的二进制表示，不是 数值的二进制补码。

进行数字到字符串的转换时，建议用小括号将要转换的目标括起来，防止出错。

```javascript


var count = 10;

console.log(count.toString());    // 输出 '10'
console.log((17).toString());     // 输出 '17'
console.log((17.2).toString());   // 输出 '17.2'

var x = 6;

console.log(x.toString(2));       // 输出 '110'
console.log((254).toString(16));  // 输出 'fe'

console.log((-10).toString(2));   // 输出 '-1010'
console.log((-0xff).toString(2)); // 输出 '-11111111'

```





