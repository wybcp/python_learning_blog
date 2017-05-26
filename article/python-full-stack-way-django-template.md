# Python全栈之路系列之Django模板

模板是一个文本，用于分离文档的表现形式和内容，模板定义了占位符以及各种用于规范文档该如何显示的各部分基本逻辑（模板标签）。模板通常用于产生HTML，但是Django的模板也能产生任何基于文本格式的文档。

## 如何使用模板系统

在Python代码中使用Django模板的最基本方式如下：

1. 可以用原始的模板代码字符串创建一个`Template`对象，Django同样支持用指定模板文件路径的方式来创建`Template`对象;
2. 调用模板对象的`render`方法，并且传入一套变量`context`。它将返回一个基于模板的展现字符串，模板中的变量和标签会被`context`值替换。

## 模板渲染

模板渲染

一旦你创建一个`Template`对象，你可以用`context`来传递数据给它。一个context是一系列变量和它们值的集合。

`context`在Django里表现为`Context`类，在`django.template`模块里,她的构造函数带有一个可选的参数： 一个字典映射变量和它们的值。调用`Template`对象的`render()`方法并传递context来填充模板：

```bash
# manage.py shell命令会在启动解释器之前告诉Django使用哪个配置文件
E:\mysite>python manage.py shell
Python 3.5.2 (v3.5.2:4def2a2901a5, Jun 25 2016, 22:18:55) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from django.template import Context, Template
>>> t = Template('My name is {{ name }}.')
>>> c = Context({'name': 'Stephane'})
>>> t.render(c)
'My name is Stephane.'
```

这就是使用Django模板系统的基本规则：写模板，创建`Template`对象，创建`Context`，调用`render()`方法。

## 深度变量的查找

我们通过`context`传递的简单参数值主要是字符串，还有一个`datetime.date`范例。然而，模板系统能够非常简洁地处理更加复杂的数据结构，例如list、dictionary和自定义的对象。

在Django模板中遍历复杂数据结构的关键是句点字符`(.)`。

最好是用几个例子来说明一下。 比如，假设你要向模板传递一个Python字典,要通过字典键访问该字典的值，可使用一个句点：

```python
>>> from django.template import Template, Context
>>> person = {'name': 'Sally', 'age': '43'}
>>> t = Template('{{ person.name }} is {{ person.age }} years old.')
>>> c = Context({'person': person})
>>> t.render(c)
'Sally is 43 years old.'
```

点语法也可以用来引用对象的`方法`。 例如，每个Python字符串都有`upper()`和`isdigit()`方法，你在模板中可以使用同样的句点语法来调用它们：

```python
>>> from django.template import Template, Context
>>> t = Template('{{ var }} -- {{ var.upper }} -- {{ var.isdigit }}')
>>> t.render(Context({'var': 'hello'}))
'hello -- HELLO -- False'
>>> t.render(Context({'var': '123'}))
'123 -- 123 -- True'
```

句点也可用于访问列表索引，例如：

```python
>>> from django.template import Template, Context
>>> t = Template('Item 2 is {{ items.2 }}.')
>>> c = Context({'items': ['apples', 'bananas', 'carrots']})
>>> t.render(c)
'Item 2 is carrots.'
```

**当模板系统在变量名中遇到点时，按照以下顺序尝试进行查找：**

1. 字典类型查找(比如`foo["bar"]`)
2. 属性查找(比如`foo.bar`)
3. 方法调用(比如`foo.bar()`)
4. 列表类型索引查找(比如`foo[bar]`)

## 模板标签

**`if/else`**

`{ %` `if` `% }`标签检查一个变量的值是否为真或者等于另外一个值，如果为真，系统会执行`{ %` `if` `% }`和`{ %` `endif` `% }`之间的代码块，例如：

```html
{% if today_is_weekend %}
    <p>Welcome to the weekend!</p>
{% endif %}
```

`{ %` `else` `% }`标签是可选的：

如果不为真则执行`{ %` `else` `% }`和`{ %` `endif` `% }`之间的代码块

`{ %` `if` `% }`标签不允许在同一个标签中同时使用`and`和`or`，因为逻辑上可能模糊的,比如这样的代码是不合法的：

```html
{% if athlete_list and coach_list or cheerleader_list %}
```

系统不支持用圆括号来组合比较操作，如果你确实需要用到圆括号来组合表达你的逻辑式，考虑将它移到模板之外处理，然后以模板变量的形式传入结果吧，或者，仅仅用嵌套的`{ %` `if` `% }`标签替换吧，就像这样：


```html
{% if athlete_list %}
    {% if coach_list or cheerleader_list %}
        We have athletes, and either coaches or cheerleaders!
    {% endif %}
{% endif %}
```
多次使用同一个逻辑操作符是没有问题的，但是我们不能把不同的操作符组合起来。 例如，这是合法的：
```html
{% if athlete_list or coach_list or parent_list or teacher_list %}
```
并没有`{ %` `elif` `% }`标签，使用嵌套的`{ %` `if` `% }`标签来达成同样的效果:
```html
{% if athlete_list %}
    <p>Here are the athletes: {{ athlete_list }}.</p>
{% else %}
    <p>No athletes are available.</p>
    {% if coach_list %}
        <p>Here are the coaches: {{ coach_list }}.</p>
    {% endif %}
{% endif %}
```
一定要用`{ %` `endif` `% }`关闭每一个`{ %` `if` `% }`标签

- **`for`**

`{ %` `for` `% }`允许我们在一个序列上迭代,与Python的 for 语句的情形类似，循环语法是`for X in Y`，`Y`是要迭代的序列而`X`是在每一个特定的循环中使用的变量名称。每一次循环中，模板系统会渲染在`{ %` `for` `% }` 和`{ %` `endfor` `% }`之间的所有内容。

例如创建一个运动员列表`athlete_list`变量，给标签增加一个`reversed`使得该列表被反向迭代,显示这个列表

```html
<ul>
	{% for athlete in athlete_list reversed  %}
		<li>{{ athlete.name }}</li>
	{% endfor %}
</ul>
```

在执行循环之前先检测列表的大小为空时输出一些特别的提示:
```html
{% for athlete in athlete_list %}
    <p>{{ athlete.name }}</p>
\{\% else %\}\或者\{\% empty \%\}
    <p>There are no athletes. Only computer programmers.</p>
{% endfor %}
```

Django不支持退出循环操作，也不支持continue语句。

在每个`{ %` `for` `% }`循环里有一个称为`forloop`的模板变量。这个变量有一些提示循环进度信息的属性。

|属性|描述|
|:--|:--|
|`forloop.counter`|表示当前循环的执行次数的整数计数器，从1开始计数|
|`forloop.counter0`|从0开始计数|
|`forloop.revcounter`|循环中剩余项的整型变量,开始执行时值为列表中的总数，结束时为1|
|`forloop.revcounter0`|以0做为结束索引|
|`forloop.first`|是一个布尔值，如果该迭代是第一次执行，那么它被置为False|
|`forloop.last`|是一个布尔值；在最后一次执行循环时被置为True|
|`forloop.parentloop`|指向当前循环的上一级循环的 forloop 对象的引用(在嵌套循环的情况下)|
|`forloop`|仅仅能够在循环中使用，在模板解析器碰到`{ %` `endfor` `% }`标签后，forloop就不可访问了|

**`ifequal/ifnotequal`**

`{ %` `ifequal` `% }`标签比较两个值，当他们相等时，显示在`{ %` `ifequal` `% }`和`{ %` `endifequal` `% }` 之中所有的值。

下面的例子比较两个模板变量`user`和`currentuser`:

```html
\{\% ifequal user currentuser %\}\
    <h1>Welcome!</h1>
\{\% endifequal %\}\

```

和`{ %` `if` `% }`类似，`{ %` `ifequal` `% }`支持可选的`{ %` `else` `% }` 标签，其他任何类型，例如Python的字典类型、列表类型、布尔类型，不能用在`{ %` `ifequal` `% }`中。

**`注释`**

就像HTML或者Python，Django模板语言同样提供代码注释，注释使用`{# #}`：

```html
{# This is a comment #}
```

注释的内容不会在模板渲染时输出。

**`过滤器`**

模板过滤器是在变量被显示前修改它的值的一个简单方法，过滤管道可以被`套接`，既是说，一个过滤器管道的输出又可以作为下一个管道的输入，如此下去，下面的例子实现查找列表的第一个元素并将其转化为大写:

```html
{{ my_list|first|upper }}
```

有些过滤器有参数。 过滤器的参数跟随冒号之后并且总是以双引号包含。 例如：
```html
{{ bio|truncatewords:"30" }}
```
这个将显示变量 bio 的前30个词。

|方法|描述|
|:--|:--|
|`addslashes`|添加反斜杠到任何反斜杠、单引号或者双引号前面|
|`date`|按指定的格式字符串参数格式化`date`或者`datetime`对象|
|`length`|返回变量的长度|

`date`范例：
```html
{{ pub_date|date:"F j, Y" }}
```

## 在视图中使用模板

加载模板

为了减少模板加载调用过程及模板本身的冗余代码，Django提供了一种使用方便且功能强大的`API`，用于从磁盘中加载模板，要使用此模板加载API，首先你必须将模板的保存位置告诉框架。

打开`settings.py`配置文件，找到`TEMPLATES`这项设置，设置一个目录，用于模板加载机制在哪里查找模板

```python
# 首先你要导入os模块
'DIRS': [os.path.join(BASE_DIR, 'templates')],
```

然后再`templates`目录下面创建一个名为`current_datetime.html`的文件，内容为：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
It is now {{ current_date }}.
</body>
</html>
```
`urls.py`文件内容
```python
from django.conf.urls import url
from django.contrib import admin
from DarkerProjects.views import current_datetime

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^time/', current_datetime)
]
```
`views.py`文件内容
```python
from django.template.loader import get_template
from django.template import Context
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
	# 该get_template()函数以模板名称为参数，在文件系统中找出模块的位置，打开文件并返回一个编译好的Template对象
    t = get_template('current_datetime.html')
    html = t.render(Context({'current_date': now}))
    return HttpResponse(html)
```

`get_template()`方法会自动为你连接已经设置的`TEMPLATES`配置的`DIRS`目录和你传入该法的模板名称参数。比如，你的`DIRS`目录设置为`'/home/django/mysite/templates'`，上面的`get_template()`调用就会为你找到`/home/django/mysite/templates/current_datetime.html`这样一个位置。

如果`get_template()`找不到给定名称的模板，将会引发一个`TemplateDoesNotExist`异常。

然后我们就可以通过`python manage.py runserver`来运行Django了，通过访问`http://127.0.0.1:8000/time/`会返回当前时间的字符串。

## render_to_response()

Django提供了一个捷径，让你一次性地载入某个模板文件，渲染它，然后将此作为`HttpResponse`返回。

该捷径就是位于`django.shortcuts`模块中名为`render_to_response()`的函数。

下面就是使用`render_to_response()`重新编写过的`current_datetime`范例:

```python
from django.shortcuts import render_to_response
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    return render_to_response('current_datetime.html', {'current_date': now})
```

在上面的代码中我们不再需要导入`get_template`、`Template`、`Context`和`HttpResponse`。相反，我们导入 `django.shortcuts.render_to_response`。`import datetime`继续保留.

在`current_datetime`函数中，我们仍然进行`now`计算，但模板加载、上下文创建、模板解析和`HttpResponse`创建工作均在对`render_to_response()`的调用中完成了。由于`render_to_response()`返回`HttpResponse`对象，因此我们仅需在视图中`return`该值。

`render_to_response()`的第一个参数必须是要使用的模板名称,如果要给定第二个参数，那么该参数必须是为该模板创建`Context`时所使用的字典。如果不提供第二个参数，`render_to_response()`使用一个空字典.

## locals()技巧

思考一下我们对 current_datetime 的最后一次赋值:
```python
def current_datetime(request):
    now = datetime.datetime.now()
    return render_to_response('current_datetime.html', {'current_date': now})
```
很多时候，就像在这个范例中那样，你发现自己一直在计算某个变量，保存结果到变量中（比如前面代码中的now），然后将这些变量发送给模板。尤其喜欢偷懒的程序员应该注意到了，不断地为临时变量和临时模板命名有那么一点点多余。不仅多余，而且需要额外的输入。

如果你是个喜欢偷懒的程序员并想让代码看起来更加简明，可以利用 Python 的内建函数`locals()`。它返回的字典对所有局部变量的名称与值进行映射。因此，前面的视图可以重写成下面这个样子：
```python
def current_datetime(request):
    current_date = datetime.datetime.now()
    return render_to_response('current_datetime.html', locals())
```
在此，我们没有像之前那样手工指定`context`字典，而是传入了`locals()`的值，它囊括了函数执行到该时间点时所定义的一切变量。因此，我们将`now`变量重命名为`current_date`，因为那才是模板所预期的变量名称。在本例中，`locals()`并没有带来多大的改进，但是如果有多个模板变量要界定而你又想偷懒，这种技术可以减少一些键盘输入。

使用`locals()`时要注意是它将包括所有的局部变量，它们可能比你想让模板访问的要多。在前例中，`locals()`还包含了`request`。对此如何取舍取决你的应用程序。

## get_template()中使用子目录

把模板存放于模板目录的子目录中是件很轻松的事情，只需在调用`get_template()`时，把子目录名和一条斜杠添加到模板名称之前，如：
```pthon
t = get_template('dateapp/current_datetime.html')
```
由于`render_to_response()`只是对`get_template()`的简单封装，你可以对`render_to_response()`的第一个参数做相同处理。
```python
return render_to_response('dateapp/current_datetime.html', {'current_date': now})
```
对子目录树的深度没有限制，你想要多少层都可以，只要你喜欢，用多少层的子目录都无所谓。

## include模板标签

内建模板标签`{ %` `include` `% }`，该标签允许在(模板中)包含其它的模板的内容，标签的参数是所要包含的模板名称，可以是一个变量，也可以是用`单/双引号`硬编码的字符串，每当在多个模板中出现相同的代码时，就应该考虑是否要使用`{ %` `include` `% }`来减少重复。

下面这两个例子都包含了`nav.html`模板

```html
{% include 'nav.html' %}
{% include "nav.html" %}
```

下面的例子包含了`includes/nav.html`模板的内容:

```html
{% include 'includes/nav.html' %}
```

下面的例子包含了以变量`template_nam`的值为名称的模板内容：

```html
{% include template_name %}
```

如果`{ %` `include` `% }`标签指定的模板没找到，Django将会在下面两个处理方法中选择一个：

1. 如果`DEBUG`设置为`True`,你将会在Django错误信息页面看到`TemplateDoesNotExist`异常;
2. 如果`DEBUG`设置为`False`,该标签不会引发错误信息，在标签位置不显示任何东西;

## 模板继承

本质上来说，模板继承就是先构建一个基础模板框架，而后在其子模板中对它所包含站点公用部分和定义块进行重载。

定义`基础模板`，该框架之后将由`子模板`所继承:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>\{\% block title %\}\{\% endblock %\}\</title>
</head>
<body>
    <h1>My helpful timestamp site</h1>
    \{\% block content %\}\{\% endblock %\}\
    \{\% block footer %\}\
    <hr>
    <p>Thanks for visiting my site.</p>
    \{\% endblock %\}\
</body>
</html>
```

这个叫做`base.html`的模板定义了一个简单的HTML框架文档，我们将在本站点的所有页面中使用,子模板的作用就是重载、添加或保留那些块的内容。

每个`{ %` `block` `% }`标签所要做的是告诉模板引擎，该模板下的这一块内容将有可能被子模板覆盖。

修改`current_datetime.html`文件内容为

```html
\{\% extends "includes/base.html" %\}\

\{\% block title %\}\The current time\{\% endblock %\}\

\{\% block content %\}\
<p>It is now {{ current_date }}.</p>
\{\% endblock %\}\
```

**以下是其工作方式:**

在加载`current_datetime.html`模板时，模板引擎发现了`{ %` `extends` `% }`标签，注意到该模板是一个子模板，模板引擎立即装载其父模板，即本例中的`base.html`

此时，模板引擎注意到`base.html`中的三个`{ %` `block` `% }`标签，并用子模板的内容替换这些`block`因此，引擎将会使用我们在`{ %` `block title` `% }`中定义的标题，对`{ %` `block content` `% }`也是如此，所以，网页标题一块将由`{ %` `block title` `% }`替换，同样地，网页的内容一块将由`{ %` `block content` `% }`替换。

注意由于子模板并没有定义`footer`块，模板系统将使用在父模板中定义的值。父模板`{ %` `block` `% }`标签中的内容总是被当作一条退路。

继承并不会影响到模板的上下文，换句话说，任何处在继承树上的模板都可以访问到你传到模板中的每一个模板变量。

你可以根据需要使用任意多的继承次数，使用继承的一种常见方式是下面的**三层法**：

1. 创建`base.html`模板，在其中定义站点的主要外观感受,这些都是不常修改甚至从不修改的部分。
2. 为网站的每个区域创建`base_SECTION.html`模板(例如,`base_photos.html`和`base_forum.html`)。这些模板对`base.html`进行拓展，并包含区域特定的风格与设计;
3. 为每种类型的页面创建独立的模板，例如论坛页面或者图片库，这些模板拓展相应的区域模板。

这个方法可最大限度地重用代码，并使得向公共区域（如区域级的导航）添加内容成为一件轻松的工作，以下是使用**模板继承的一些诀窍**：

1. 如果在模板中使用`{ %` `extends` `% }` ，必须保证其为模板中的第一个模板标记，否则，模板继承将不起作用;
2. 一般来说，基础模板中的`{ %` `block` `% }`标签越多越好，记住，子模板不必定义父模板中所有的代码块，因此你可以用合理的缺省值对一些代码块进行填充，然后只对子模板所需的代码块进行（重）定义。 俗话说，钩子越多越好;
3. 如果发觉自己在多个模板之间拷贝代码，你应该考虑将该代码段放置到父模板的某个`{ %` `block` `% }`中
4. 如果你需要访问父模板中的块的内容，使用`{{ block.super }}`这个标签吧，这一个魔法变量将会表现出父模板中的内容，如果只想在上级代码块基础上添加内容，而不是全部重载，该变量就显得非常有用了;
5. 不允许在同一个模板中定义多个同名的`{ %` `block` `% }` ,存在这样的限制是因为block标签的工作方式是双向的，也就是说，block标签不仅挖了一个要填的坑，也定义了在父模板中这个坑所填充的内容。如果模板中出现了两个相同名称的`{ %` `block` `% }`标签，父模板将无从得知要使用哪个块的内容;

6. `{ %` `extends` `% }`对所传入模板名称使用的加载方法和`get_template()`相同。 也就是说，会将模板名称被添加到`TEMPLATE_DIRS`设置之后;

7. 多数情况下，`{ %` `extends` `% }`的参数应该是字符串，但是如果直到运行时方能确定父模板名，这个参数也可以是个变量，这使得你能够实现一些很酷的动态功能;
