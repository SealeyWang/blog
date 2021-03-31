
# 事件捕获 与 事件冒泡

从外到内找监听函数，叫事件捕获

从内到外找监听函数，叫事件冒泡

有函数监听函数就调用，并提供事件信息



先执行捕获阶段的监听函数

后执行冒泡阶段的监听函数
![eventflow](https://www.w3.org/TR/DOM-Level-3-Events/images/eventflow.svg)


```javascript

fn(e){
    
}

el.addEventListener('click',fn,bool){
    //..........
}

// bool 不填  默认为false  设置的是事件冒泡

// bool 为true 则设置的是事件捕获



```


# target currentTarget  取消冒泡

[event mdn](https://developer.mozilla.org/zh-CN/docs/Web/API/Event#properties)

e 代指  event 

e.bubbles （boolean值） 表示事件是否会在DOM中冒泡

e.cancelable （boolean值） 表示事件是否可以取消


捕获不可取消

 使用e.stopPropagation() 可以取消冒泡

```javascript

fn(e){
    e.target // 用户操作的元素
    e.currentTarget // 程序员设置监听的元素

    this // 这里的this 是 e.currentTarget 
}

el.addEventListener('click',fn,bool){
    e.stopPropagation() // 取消冒泡
}


```


## 特例 

* 当只有一个div被监听（不考虑父子同时被监听）
* 分别监听了 捕获阶段和冒泡阶段  
* 用户点击的元素就是开发者监听的元素

此时 谁先监听就执行谁()


不过chrome 有特殊处理 ， 在chrome（89.0.4389.90（正式版本）） 总是 先执行捕获，后执行冒泡（我感觉更好）
[例子](https://jsbin.com/minawomayi/edit?html,css,js,console,output)


# 事件委托

委托一个元素监听 其他元素的事件

## 节省监听数
如果要给100个按钮添加点击事件  怎么办？ 

可以给100个元素的上级元素添加事件，冒泡的时候判断target是不是100个按钮中的一个， 这样可以减少监听数量，省内存

```html

<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>test</title>
</head>

<style>
</style>
<body>
<div id="parent">
    <button>click 1</button>
    <button>click 2</button>
    <button>click 3</button>
    <!-- ......省略一部分 -->
    <button>click 98</button>
    <button>click 99</button>
    <button>click 100</button>

</div>

<script>

    let parent = document.querySelector('#parent')

    parent.addEventListener('click',function (e) {
        const t = e.target
        if(t.tagName.toLowerCase() === 'button') {
            console.log('button被点击了' + t.textContent)
        }


    })

</script>
</body>
</html>

```







## 监听动态元素
监听 目前不存在元素的点击事件

通过监听上级元素，等点击的时候看是不是想要监听的


```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>test</title>
</head>

<style>
</style>
<body>
<div id="parent">

</div>

<script>

    let parent = document.querySelector('#parent')

    parent.addEventListener('click',function (e) {
        const t = e.target
        if(t.tagName.toLowerCase() === 'button') {
            console.log('button被点击了' + t.textContent)

        }
    })

    setTimeout( ()=> {
        const button = document.createElement('button')
        button.textContent = 'click 1'
        parent.appendChild(button);
    },1000)

</script>
</body>
</html>

```





