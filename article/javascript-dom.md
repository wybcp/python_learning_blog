# JavaScript DOM

文档对象模型**（Document Object Model，DOM）**是一种用于HTML和XML文档的编程接口。它给文档提供了一种结构化的表示方法，可以改变文档的内容和呈现方式。我们最为关心的是，DOM把网页和脚本以及其他的编程语言联系了起来。DOM属于浏览器，而不是JavaScript语言规范里的规定的核心内容。

## 查找元素

直接查找

找到的结果一类是元素，一类是元素集合，如果要找到元素集合内的某一个元素可以用索引来进行取值

|方法|描述|
|:--|:--|
|`document.getElementById`|根据ID获取一个标签|
|`document.getElementsByName`|根据name属性获取标签集合|
|`document.getElementsByClassName`|根据class属性获取标签集合|
|`document.getElementsByTagName`|根据标签名获取标签集合|

间接查找

只包含标签不包含文本

|方法|描述|
|:--|:--|
|`parentNode`|父节点|
|`childNodes`|所有子节点|
|`firstChild`|第一个子节点|
|`lastChild `|最后一个子节点|
|`nextSibling`|下一个兄弟节点|
|`previousSibling`|上一个兄弟节点|
|`nodeType`|用于区别是文本还是标签|

既包含文本又包含标签

|方法|描述|
|:--|:--|
|`parentElement`|父节点标签元素|
|`children`|所有子标签|
|`firstElementChild`|第一个子标签元素|
|`lastElementChild`|最后一个子标签元素|
|`nextElementSibling`|下一个兄弟标签元素|
|`previousElementSibling`|上一个兄弟标签元素|

## 操作

内容

|方法|描述|
|:--|:--|
|`innerText`|纯文本内容不包含标签|
|`outerText`|文本内容|
|`innerHTML`|文本内容和HTML|
|`value`|获取文本框，下拉框的值|

属性

|方法|描述|
|:--|:--|
|`attributes`|获取所有标签属性|
|`setAttribute(key,value)`|设置标签属性|
|`getAttribute(key)`|获取指定标签属性|

class操作

|方法|描述|
|:--|:--|
|`className`|获取所有类名|
|`classList`|获取所有类名,以数组显示|
|`classList.remove(cls)`|删除指定类|
|`classList.add(cls)`|添加类|

标签操作

创建标签

```JavaScript
// 方式一
var tag = document.createElement('a')
tag.innerText = "wupeiqi"
tag.className = "c1"
tag.href = "http://www.cnblogs.com/wupeiqi"
 
// 方式二
var tag = "<a class='c1' href='http://www.cnblogs.com/wupeiqi'>wupeiqi</a>"
```

操作标签

```JavaScript
// 方式一
var obj = "<input type='text' />";
xxx.insertAdjacentHTML("beforeEnd",obj);
xxx.insertAdjacentElement('afterBegin',document.createElement('p'))
 
//注意：第一个参数只能是'beforeBegin'、 'afterBegin'、 'beforeEnd'、 'afterEnd'
 
// 方式二
var tag = document.createElement('a')
xxx.appendChild(tag)
xxx.insertBefore(tag,xxx[1])
```

样式操作

```JavaScript
var obj = document.getElementById('i1')
 
obj.style.fontSize = "32px";
obj.style.backgroundColor = "red";
```

位置操作

```JavaScript
// 总文档高度
document.documentElement.offsetHeight
  
// 当前文档占屏幕高度
document.documentElement.clientHeight
  
// 自身高度
tag.offsetHeight
  
// 距离上级定位高度
tag.offsetTop
  
// 父定位标签
tag.offsetParent
  
// 滚动高度
tag.scrollTop
 

clientHeight  // 可见区域：height + padding
clientTop     // border高度
offsetHeight  // 可见区域：height + padding + border
offsetTop     // 上级定位标签的高度
scrollHeight  // 全文高：height + padding
scrollTop     // 滚动高度
特别的：document.documentElement代指文档根节点
```

提交表单

```JavaScript
document.geElementById('form').submit()
```

其他操作

```JavaScript
// 输出框
console.log
// 弹出框
alert
确认框
confirm
  
// URL和刷新
location.href               // 获取URL
location.href = "url"       // 重定向
location.reload()           // 重新加载
  
// 定时器
setInterval                 // 多次定时器
clearInterval               // 清除多次定时器
setTimeout                  // 单次定时器
clearTimeout                // 清除单次定时器
```

## 事件

|属性|动作|
|:--|:--|
|`onabort`|图像的加载被中断|
|`onblur`|元素失去焦点|
|`onchange`|域的内容被改变|
|`onclick`|当用户点击某个对象时调用的事件句柄|
|`ondblclick`|当用户双击某个对象时调用的事件句柄|
|`onerror`|在加载文档或图像时发生错误|
|`onfocus`|元素获得焦点|
|`onkeydown`|某个键盘按键被按下|
|`onkeypress`|某个键盘按键被按下并松开|
|`onkeyup`|某个键盘按键被松开|
|`onload`|一张页面或一幅图像完成加载|
|`onmousedown`|鼠标按钮被按下|
|`onmousemove`|鼠标被移动|
|`onmouseout`|鼠标从某元素移开|
|`onmouseover`|鼠标移到某元素之上|
|`onmouseup`|鼠标按键被松开|
|`onreset`|重置按钮被点击|
|`onresize`|窗口或框架被重新调整大小|
|`onselect`|文本被选中|
|`onsubmit`|确认按钮被点击|
|`onunload`|用户退出页面|

参考： http://www.w3school.com.cn/jsref/dom_obj_event.asp