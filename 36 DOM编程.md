

获取元素


```javascript
 window.idxxx
        document.getElementById('idxxx')
        document.getElementsByTagName('div')[0]
        document.getElementsByClassName('red')[0]
        document.querySelector('#idxxx')
        document.querySelectorAll('.red')[0]
```



工作一般用 querySelector, querySelectorAll

做demo用idxxx 省事

兼容IE 才用getElement(s)ByXXX



获取特定元素


```javascript
   // html 元素
        let html = document.documentElement
        // head 元素
        let head = document.head
        // body 元素
        let body = document.body
        // 获取窗口
        window
        // 获取所有元素
        document.all

        if (!document.all) {
            console.log('document.all 是第6个falsy值')
        }

```

获取的元素是什么


![div 原型链](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/94a90f0ae37e4bdd8fe1ac006ab8efca~tplv-k3u1fbpfcp-watermark.image)




# 元素Element 节点Node ？
[Node](https://developer.mozilla.org/zh-CN/docs/Web/API/Node)

[Node.nodeType](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType)

x.nodeType

1 表示元素Element  也叫标签 tag

3 文本 

Node 还能包含回程空格



# 节点的增删改查

## 增


```javascript
//创建标签节点
       let div1 =  document.createElement('div')
        document.createElement('style')
        document.createElement('script')
        document.createElement('li')


//创建文本节点

      let text1 = document.createTextNode('你好')

        // 标签里插入文本
        div1.appendChild(text1)
        div1.innerText = '你好'
        div1.textContent = '你好'

        // div1.appendChild('你好') // appendChild 只能接收 Node 参数 此为错误示例


        
        // 插入到页面中
        document.body.appendChild(div1)
        idxxx.appendChild('div1')// idxxx 为已在页面中的元素

```

插入到页面中

创建的标签默认处于JS线程中

必须插入到 head or body 里，才会生效

```javascript

        document.body.appendChild(div1)
        idxxx.appendChild('div1')// idxxx 为已在页面中的元素

```

一个元素不能出现在两个地方，除非复制一份s
```javascript
   <div id="test1"></div>
    <div id="test2"></div>

        let div = document.createElement('div')
        test1.appendChild(div) // 把div插入到test1 里
        test2.appendChild(div) // 此时 移动到test2 里

```

## 删 

```javascript


    
    parentNode.removeChild()
    childNode.remove()

    Element.prototype.hasOwnProperty('remove') // true
    Node.prototype.hasOwnProperty('removeChild')//true 


```

一个node 被移除dom树，其实还在js 线程内存中 可以被再次append 到页面里


## 改

### 写标准属性  
非标准属性 改了后不会同步到dom上 所以自定义属性要用data-*
```javascript

 let div = document.createElement('div')

    
        // 改class
        div.className = 'red bule' // 全覆盖
        div.classList.add('red')
        div.style = 'width:100px;height:100px' // style 全覆盖
        div.style.width = '100px'
        //css - 变大小写
        div.style.backgroundColor = 'while'
        // 改 data-*  
        div.dataset.x = 'wsl'
```

### 读标准属性 

两种方法读出来的值可能稍有不同
```javascript
  <a id="a" href="/xxx"></a>

        div.classList
        div.getAttribute('class')

        // 两种不同的值
        a.href // "http://localhost:1234/xxx"
        a.getAttribute('href') //xxx
```

### 改事件 
```javascript

  let div = document.createElement('div')

        let fn = function (event) { }
        div.onclick = fn 
        fn.call(div, event) // div 被当做this event 包含点击事件所有的信息，如坐标
        
        
        // div.addEventListener  
        
```

### 改内容
```javascript
let div = document.createElement('div')
        //改文本内容
        div.innerText = 'xxx'
        div.textContent = 'xxx'

        //改html内容
        div.innerHTML = '<strong>加粗</strong>'

        // 改标签
        let div2 = document.createElement('div2')

        div.innerHTML = ''//先清空
        div.appendChild(div2) // 再加内容
```

### 改爸爸

```javascript
        newParent.appendChild(div)//会直接移动
```

## 查

```javascript

  let node = document.createElement('div')


        // Node 提供的的API child数量会算上element text 和 空格
        Node.prototype.hasOwnProperty('childNodes')// true 
        //后面不 不标注特殊的 都是Node提供的属性



        // 查爸爸
        node.parentNode
        node.parentElement

        // 查爷爷
        node.parentNode.parentNode
        // 查子代 子代变化  长度也跟着实时变化
        node.childNodes
        node.children
        Element.prototype.hasOwnProperty('children')// true 

        // 查兄弟姐妹  
        node.parentNode.childNodes // 要排除自己  算空格了
        node.parentNode.children // 要排除自己  Element 提供的

        node.firstChild
        node.lastChild
        node.previousSibling
        node.nextSibling // 下一个兄弟节点


        // 遍历div里的所有元素  （遍历树）
     let div = document.createElement('div')

        let travel = (node, fn) => {
            fn(node)
            if (node.children) {
                for (let i = 0; i < node.children.length; i++) {
                    travel(node.children[i], fn)
                }
            }
        }

        let callback = (node) => {
            console.log(nod)
        }
        travel(div, callback)     
```

# 跨线程操作
js引擎智能操作js

渲染引擎只能操作页面

跨线程通信 

* 当浏览器发现js在body 添加了个div对象
* 浏览器通知渲染引擎新增一个div元素
* 新增div元素 所有属性照抄div对象




# 属性同步
![跨线程操作](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f4b818ba338546639c986b9cde58b85e~tplv-k3u1fbpfcp-watermark.image)

## 标准属性
对div1的标准属性新改，会被浏览器同步到页面中
 如 id  className title

data-* 同上


非标准属性 只会停留在JS线程中，不会同步到页面里

如果想自定义属性  用 data- 做前缀

如 div1 上 xx属性  用div1.dataset.xx   访问

![属性同步]](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aca04426e6b943a39a062a6c169a6452~tplv-k3u1fbpfcp-watermark.image)



# Property vs Attribute 

js线程 div对象所有属性 叫 property

渲染引擎中div 对应标签的属性，叫attribute

* 大部分时候 同名的 property 和 attribute 相等
* 不是标准属性 只会在一开始相等
* attribute 只支持字符串
* property 支持字符串 布尔


# 扩展

[为什么说DOM操作很慢](https://segmentfault.com/a/1190000004114594)
[为什么经过10年的努力DOM操作还是这么慢](https://stackoverflow.com/questions/6817093/but-whys-the-browser-dom-still-so-slow-after-10-years-of-effort)