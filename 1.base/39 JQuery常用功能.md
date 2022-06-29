# jQuery 如何获取元素

```javascript
　$(document) //选择整个文档对象



    $('selector') 
　　$('#myId') //选择ID为myId的网页元素
　　$('div.myClass') // 选择class为myClass的div元素
　　$('input[name=first]') // 选择name属性等于first的input元素



    // jquery 特有表达式
　  $('a:first') //选择网页中第一个a元素

　　$('tr:odd') //选择表格的奇数行

　　$('#myForm :input') // 选择表单中的input元素

　　$('div:visible') //选择可见的div元素

　　$('div:gt(2)') // 选择所有的div元素，除了前三个

　　$('div:animated') // 选择当前处于动画状态的div元素


    // 改变结果集
    $('div').has('p'); // 选择包含p元素的div元素

　　$('div').not('.myClass'); //选择class不等于myClass的div元素

　　$('div').filter('.myClass'); //选择class等于myClass的div元素

　　$('div').first(); //选择第1个div元素

　　$('div').eq(5); //选择第6个div元素


    // 选择父子 兄弟元素
    $('div').parent(); //选择div元素的父元素
    　$('div').children(); //选择div的所有子元素
    　$('div').siblings(); //选择div的兄弟（同级）元素
```
# jQuery 的链式操作是怎样的

```javascript
　$('div').find('h3').eq(2).html('Hello');
    

    $('div') //找到div元素

　　　.find('h3') //选择其中的h3元素

　　　.eq(2) //选择第3个h3元素

　　　.html('Hello'); //将它的内容改为Hello


// end 
　$('div')

　　　.find('h3')

　　　.eq(2)

　　　.html('Hello')

　　　.end() //退回到选中所有的h3元素的那一步

　　　.eq(0) //选中第一个h3元素

　　　.html('World'); //将它的内容改为World
```

# jQuery 如何创建元素
复制元素使用.clone()。


删除元素使用.remove()和.detach()。两者的区别在于，前者不保留被删除元素的事件，后者保留，有利于重新插入文档时使用。

清空元素内容（但是不删除该元素）使用.empty()。

创建元素
```javascript
　　$('<p>Hello</p>');

　　$('<li class="new">new list item</li>');

　　$('ul').append('<li>list item</li>');

```

# jQuery 如何移动元素
```javascript

// 移动当前元素
$('div').insertAfter($('p'));//div元素移动p元素后面： return div  当前元素为div

// 移动目标元素 
$('p').after($('div'));//把div元素加到p元素后面   return p   目标元素为div

 

```
使用这种模式的操作方法，一共有四对：

　.insertAfter()和.after()：在现存元素的外部，从后面插入元素

　　.insertBefore()和.before()：在现存元素的外部，从前面插入元素

　　.appendTo()和.append()：在现存元素的内部，从后面插入元素

　　.prependTo()和.prepend()：在现存元素的内部，从前面插入元素

* jQuery 如何修改元素的属性

```javascript

　　$('h1').html(); //html()没有参数，表示取出h1的值

　　$('h1').html('Hello'); //html()有参数Hello，表示对h1进行赋值

```

　.html() 取出或设置html内容

　　.text() 取出或设置text内容

　　.attr() 取出或设置某个属性的值

　　.width() 取出或设置某个元素的宽度

　　.height() 取出或设置某个元素的高度

　　.val() 取出某个表单元素的值