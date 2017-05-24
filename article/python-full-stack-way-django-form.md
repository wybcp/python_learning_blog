# Python全栈之路系列之Django表单

## 从Request对象中获取数据

HttpRequest对象包含当前请求URL的一些信息：

|熟悉/方法|描述|例如|
|:--|:--|:--|
|`request.path`|除域名以外的请求路径|/hello/|
|`request.get_host()`|访问的域名|127.0.0.1:8000" or www.example.com|
|`request.get_full_path()`|请求路径，可能包含查询字符串|/hello/?print=true|
|`request.is_secure()`|是否通过HTTPS访问|True或者False|
|`request.get_host()`|访问的主机端口|80/8080|
|`request.META`|返回一个字典，包含了所有的请求头信息|如果访问一个不存在的建会触发KeyError异常|

一个返回所有`request.META`信息的页面函数：

```python
def display_meta(request):
    values = request.META.items()
    html = []
    for k, v in values:
        html.append('<tr><td>%s</td><td>%s</td></tr>' % (k, v))
    return HttpResponse('<table>%s</table>' % '\n'.join(html))
```

## 提交的数据信息

除了基本的元数据，HttpRequest对象还有两个属性包含了用户所提交的信息：`request.GET`和`request.POST`,二者都是类字典对象，你可以通过它们来访问GET和POST数据。

我们说`request.GET和request.POST是类字典对象`，意思是他们的行为像Python里标准的字典对象，但在技术底层上他们不是标准字典对象。比如说，`request.GET和request.POST`都有`get()、keys()和values()方法`，你可以用用`for key in request.GET`获取所有的键。

## 一个简单的表单处理示例

通常，表单开发分为两个部分：前端HTML页面用户接口和后台view函数对所提交数据的处理过程，第一部分很简单；现在我们来建立个视图来显示一个用户搜索表单，这个页面的功能如下：

1. 浏览器打开**http://127.0.0.1:8000/search-form/**，让用户输入一个用户名，然后搜索
2. 如果用户搜索的用户名在数据库中存在，那么就返回这个用户的信息信息
3. 如果用户不存在就返回和上面相同的页面，但是没有用户的信息信息
4. 如果输入为空就提示用户**输入为空**

首先我们需要在`darker`这个app目录下面的`views.py`文件中加入以下内容：

```python
from django.shortcuts import render_to_response
from darker.models import UserInfo

# Create your views here.

def search(request):
    # 定义一个变量error，默认为False
    error = False
	# 如果user在request.GET内
    if 'user' in request.GET:
	    # 获取request.GET['user']的值并赋值给username
        username = request.GET['user']
		# 如果username为空
        if len(username) == 0:
		    # 那么就error = True
            error = True
        # 如果不为空
		else:
		    # 在数据库中获取name=username的详细信息
            userinfo = UserInfo.objects.filter(name=username)
			# 跳转到页面search_results.html，传入两个值username和userinfo
            return render_to_response('search_results.html', {'username': username, 'userinfo': userinfo})
    
	# 如果user不存在在request.GET内或者如果username为空，就跳转到search_form.html页面并且传入一个值error
    return render_to_response('search_form.html', {'error': error})
```
下面是一个搜索页面的html文件内容存在于`templates/search_form.html`内：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    # 如果传入过来的error=True
    {% if error %}
	    # 那么就显示错误，否则就不显示
        <p style="color: red;">输入为空，请重新输入</p>
    {% endif %}

    <form action="" method="get">
        <input type="text" name="user">
        <input type="submit" value="搜索">
    </form>
</body>
</html>
```

然后在`urls.py`内添加视图，代码如下：

```python
from django.conf.urls import url
from darker.views import search

urlpatterns = [
    url(r'^search/', search),
]
```

`templates`下面的`search_results.html`文件内容如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<p>你搜索的用户名是： {{ username }}</p>

<p>用户信息如下</p>
{{ userinfo }}

</body>
</html>
```

打开浏览器，然后访问**http://127.0.0.1:8000/search/**，输入搜索的用户名，如果搜索到用户的信息就会返回错误信息

## 一个简单的验证

上述的实例中我们是否让用户填写一个用户名来判断用户是否存在亦或者填写的数据是否为空，那么现在我们再来做一个限制，就是如果用户输入为空，那么我们就提示**输入为空，请重新输入**，如果搜索的用户名大于10个字符那么就提示用户**输入的用户名长度大于10，请重新输入**。

修改`search`函数为：

```python
def search(request):
    errors = []
    if 'user' in request.GET:
        username = request.GET['user']
        if len(username) == 0:
            errors.append("输入为空，请重新输入")
        elif len(username) > 10:
            errors.append("输入的用户名长度大于10，请重新输入")
        else:
            userinfo = UserInfo.objects.filter(name=username)
            return render_to_response('search_results.html', {'username': username, 'userinfo': userinfo})
    return render_to_response('search_form.html', {'errors': errors})
```
修改`search_form.html`文件内容为：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    {% if errors %}
        <ul>
            {% for error in errors %}
            <li>{{ error }}</li>
            {% endfor %}
        </ul>
    {% endif %}

    <form action="" method="get">
        <input type="text" name="user">
        <input type="submit" value="搜索">
    </form>
</body>
</html>
```
最后就可以启动django然后重新访问**http://127.0.0.1:8000/search/**，在搜索框内输入`空字段`或者输入的字符`大于10`看看是什么结果。

## Form类

Django带有一个form库，称为`django.forms`或者`django.newforms`，这个库可以处理HTML表单显示以及验证.

**为什么有`django.forms`和`django.newforms`？**

改名其实有历史原因的，当Django第一次向公众发行时，它有一个复杂难懂的表单系统：`django.forms`，后来它被完全重写了，新的版本改叫作：`django.newforms`，这样人们还可以通过名称，使用旧版本，当`Django 1.0`发布时，旧版本`django.forms`就不再使用了，而`django.newforms`也终于可以名正言顺的叫做：`django.forms`。

**`darker`app下创建**forms.py**文件，内容如下**

```python
from django import forms

class ContactForm(forms.Form):
    subject = forms.CharField()
    email = forms.EmailField(required=False)
    message = forms.CharField()
```

上述代码中，表单中的每一个字段作为Form类的属性，被展现成Field类，这里只用到`CharField`和`EmailField`类型，每一个字段都默认是必填，要使email成为可选项，我们需要指定`required=False`

**Form对象做的第一件事是将自己显示成HTML**

进入带django环境变量的解释器

```bash
E:\DarkerProjects>python manage.py shell
```

```python
>>> from darker.forms import ContactForm
>>> f = ContactForm()
>>> f
<ContactForm bound=False, valid=Unknown, fields=(subject;email;message)>
>>> f.as_ul()
'<li><label for="id_subject">Subject:</label> <input id="id_subject" name="subject" type="text" required /></li>\n<li><label for="id_email">Email:</label> <input id="id_email" name="email" type="email" /></li>\n<li><label for="id_message">Message:</label> <input id="id_message" name="message" type="text" required /></li>'
>>> f.as_p()
'<p><label for="id_subject">Subject:</label> <input id="id_subject" name="subject" type="text" required /></p>\n<p><label for="id_email">Email:</label> <input id="id_email" name="email" type="email" /></p>\n<p><label for="id_message">Message:</label> <input id="id_message" name="message" type="text" required /></p>'
```

**Form对象做的第二件事是校验数据**

创建一个新的对Form象，并且传入一个与定义匹配的字典类型数据：

```python
>>> f = ContactForm({'subject': 'Hello', 'email': 'adrian@example.com', 'message': 'Nice site!'})
# 一旦你对一个Form实体赋值，你就得到了一个绑定form
>>> f.is_bound
True
```

**`is_valid()`方法用来判断它的数据是否合法**

```python
# 如果数据合法则返回True
>>> f.is_valid()
True
# 否则返回False，这里只是指定了subject的值
>>> f = ContactForm({'subject': 'Hello'})
>>> f.is_valid()
False
```

查看每个字段的出错消息

```python
>>> f = ContactForm({'subject': 'Hello', 'message': ''})
>>> f['message'].errors
['This field is required.']
>>> f['subject'].errors
[]
>>> f['email'].errors
[]
```

每个Form实体都有一个errors属性，它提供了一个字段与错误消息相映射的字典表

```python
>>> f = ContactForm({'subject': 'Hello', 'message': ''})
>>> f.errors
{'message': ['This field is required.']}
```

更改改字段显示

当你在本地显示这个表单的时，message字段被显示成

```html
<input id="id_message" name="message" type="text" required />
```

而它应该被显示成

```html
<textarea cols="40" id="id_message" name="message" rows="10" required></textarea>
```

我们可以通过设置`widget`来修改它：

```python
message = forms.CharField(widget=forms.Textarea)
```

设置主题最大长度

使subject限制在100个字符以内

```python
subject = forms.CharField(max_length=100)
```

选项`min_length`参数设置最小长度