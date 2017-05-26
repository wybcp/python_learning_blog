# 利用Emmet快速编写HTML标签

> Emmet is a web-developer’s toolkit that can greatly improve your HTML & CSS workflow

`Emmet`是一个介于标记性语言和规范之外的一种逻辑方式，它的前身是`zen coding`，`Emmet`可以大幅度提高前端开发的一个小工具，并不是开发工具，在其他IDE里面称之为插件，他可以根据你提供的逻辑快速生成HTML代码片段。

官网：http://emmet.io/

## 引示符号

|符号|描述|
|:--|:--|
|`html:5 or !`|快速生成一段HTML标准文档标签|
|`.`|Class Name|
|`#`|ID Name|
|`[]`|标签的内部属性|
|`{}`|标签的内容|
|`()`|分组标签|

快速生成一段HTML标准文档标签

```html
html:5
```
OR
```html
!
```

生成的HTML标签

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

</body>
</html>
```

Class

```html
ul>li.ClassName
```

生成的HTML标签

```html
<ul>
    <li class="ClassName"></li>
</ul>
```

 ID

```html
ul>li#ClassName
```

生成的HTML标签

```html
<ul>
    <li id="ClassName"></li>
</ul>
```

标签的内部属性

```html
ul>li>a[href="https://www.ansheng.me"]
```

生成的HTML标签

```html
<ul>
    <li><a href="https://www.ansheng.me"></a></li>
</ul>
```

标签的内容

```html
div>{Hello World}
```

生成的HTML标签

```html
<div>Hello World</div>
```

分组标签

```html
(h1>p>a[href="https://www.ansheng.me/"])+(h2>p{Hello World})
```

生成的HTML标签

```html
<h1>
    <p><a href="https://www.ansheng.me/"></a></p>
</h1>
<h2>
    <p>Hello World</p>
</h2>
```

## 关系符号

|符号|描述|
|:--|:--|
|`>`|嵌套元素|
|`+`|同级元素|
|`^`|上级元素|
|`*`|同级多少个元素|
|`$`|数字通配符正序|
|`@`|数字通配符倒序|

嵌套元素

```html
ul>li>a{Hello World!}
```

生成的HTML标签

```html
<ul>
    <li><a href="">Hello World!</a></li>
</ul>
```

同级元素

```html
h2+h3
```

生成的HTML标签

```html
<h2></h2>
<h3></h3>
```

上级元素

```html
ul>li^p
```

生成的HTML标签

```html
<ul>
    <li></li>
</ul>
<p></p>
```

```html
div>ul>li^^p
```

生成的HTML标签

```html
<div>
    <ul>
        <li></li>
    </ul>
</div>
<p></p>
```

```html
div>ul>li^ul>li
```

生成的HTML标签

```html
<div>
    <ul>
        <li></li>
    </ul>
    <ul>
        <li></li>
    </ul>
</div>
```

同级多少个元素

```html
ul>li*5
```

生成的HTML标签

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

数字通配符

```html
ul>li.a$*5
```

生成的HTML标签

```html
<ul>
    <li class="a1"></li>
    <li class="a2"></li>
    <li class="a3"></li>
    <li class="a4"></li>
    <li class="a5"></li>
</ul>
```

```html
ul>li.a$$$*5
```

生成的HTML标签

```html
<ul>
    <li class="a001"></li>
    <li class="a002"></li>
    <li class="a003"></li>
    <li class="a004"></li>
    <li class="a005"></li>
</ul>
```

```html
ul>li.a$@-*5
```

生成的HTML标签

```html
<ul>
    <li class="a5"></li>
    <li class="a4"></li>
    <li class="a3"></li>
    <li class="a2"></li>
    <li class="a1"></li>
</ul>
```

```html
ul>li.a$@3*5
```

生成的HTML标签

```html
<ul>
    <li class="a3"></li>
    <li class="a4"></li>
    <li class="a5"></li>
    <li class="a6"></li>
    <li class="a7"></li>
</ul>
```

## 占位标记

|符号|描述|
|:--|:--|
|`lorem`|生成多端文字|

实例

```html
p*2>lorem
```

生成的HTML标签

```html
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Labore molestias, neque. Ad atque commodi deleniti,
    doloribus fugiat hic in libero nam nisi non omnis quas recusandae, rem repellendus soluta vel.</p>
<p>Ad alias amet aut autem debitis dolore doloribus eos esse, hic itaque iusto labore laborum modi nesciunt officiis
    quia reiciendis sed sequi vitae voluptas. A beatae enim mollitia necessitatibus perspiciatis.</p>
```

```html
p*2>lorem2
```

生成的HTML标签

```html
<p>Lorem ipsum.</p>
<p>Exercitationem, sint?</p>
```