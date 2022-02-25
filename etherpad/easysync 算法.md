# easysync 算法是干嘛的

在多人写作文档中 easysync 算法是用来解决多人编辑的冲突的问题

首先，科普下专利的重要性，easysync 这套算法，作者是申请了专利的。

专利地址：http://www.freepatentsonline.com/y2012/0110445.html

作者的 LinkedIn 页面：http://www.linkedin.com/in/aaroniba/
作者当年从 Google 离职之后创业开创了 etherpad 公司，核心算法就是这套算法，之后，被 Google 收购为开源项目。而 etherpad 是在 Apache 协议下开源的，所以我们能够使用相关的专利。

## etherpad-litter

etherpad-litter 基于 nodejs 实现 用于代替原 etherpad（ scala，java，js 实现）
repo https://github.com/ether/etherpad-lite

[参考 在线文档中 Easysync2 算法介绍](https://slix.rocks/%E5%9C%A8%E7%BA%BF%E6%96%87%E6%A1%A3%E4%B8%AD-easysync2-%E7%AE%97%E6%B3%95%E4%BB%8B%E7%BB%8D/)

# AText

AText = text + attribs 用于描述完整的文档内容

![](./img/AText.png)

内容对应的 atext (atext 使用 36 进制， 需要结合下边的 apool 看)

```
atext= { text: '\n\nxx\nyyy\n', attribs: '*0|2+2*0*7+2*0|1+1*0*i+3|1+1' }
```

\*0 是用户属性

\*0|2+2 表示 用两个字节换 2 行，带有用户属性 （表示\n\n 两个换行符，且有用户属性）

\*7 是 ib_bold 属性

\*0\*7+2 两个字节（xx）带有 author 和 ib_bold 属性

\*0|1+1 xx 后换行 不多说了

\*i 对应 ibStrikethrough 属性 (十进制 18)

\*0\*i+3 对应 yyy 有 author 和 ibStrikethrough

|1+1 表示换行 （代表一个换行符 \n）

## Apool

属性池， 文档中出现过的所有格式

```

apool= AttributePool {
  numToAttrib: {
    '0': [ 'author', 'a.nQtf9mShqVxHbbPS' ],
    '1': [ 'ibBreakLine', 'ibBreakLine' ],
    '2': [ 'insertorder', 'first' ],
    '3': [ 'lmkr', '1' ],
    '4': [ 'ibAbstractBreak', 'true' ],
    '5': [ 'bold', 'true' ],
    '6': [ 'color', 'rgb(47,48,51)' ],
    '7': [ 'ib_bold', 'true' ],
    '8': [ 'color', 'rgb(0,0,0)' ],
    '9': [ 'bold', '' ],
    '10': [ 'ib_bold', '' ],
    '11': [ 'heading', 'h1' ],
    '12': [ 'heading', 'h2' ],
    '13': [ 'heading', 'h3' ],
    '14': [ 'color', 'var(--text-color)' ],
    '15': [ 'ibUnderline', 'true' ],
    '16': [ 'ibUnderline', '' ],
    '17': [ 'underline', '' ],
    '18': [ 'ibStrikethrough', 'true' ]
  },
  attribToNum: {
    'author,a.nQtf9mShqVxHbbPS': 0,
    'ibBreakLine,ibBreakLine': 1,
    'insertorder,first': 2,
    'lmkr,1': 3,
    'ibAbstractBreak,true': 4,
    'bold,true': 5,
    'color,rgb(47,48,51)': 6,
    'ib_bold,true': 7,
    'color,rgb(0,0,0)': 8,
    'bold,': 9,
    'ib_bold,': 10,
    'heading,h1': 11,
    'heading,h2': 12,
    'heading,h3': 13,
    'color,var(--text-color)': 14,
    'ibUnderline,true': 15,
    'ibUnderline,': 16,
    'underline,': 17,
    'ibStrikethrough,true': 18
  },
  nextNum: 19
}

```
