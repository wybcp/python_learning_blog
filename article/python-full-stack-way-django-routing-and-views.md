# Python全栈之路系列之Django路由与视图

路由说白了就是与视图(函数)的对应关系，怎么说呢，一个路由对应一个视图，比如上面文章中所提到的那样，当打开`/users/`路径的时候会让`users`这个函数来进行逻辑处理，把处理的结果再返回到前端。

那么django是怎么知道从哪里找路由的配置文件入口呢？其实这在`settings.py`文件中已经被定义了：

```python
ROOT_URLCONF = 'ansheng.urls'
```

## 路由的配置

绝对地址访问

```python
# 访问地址必须是http://127.0.0.1:8000/hello/
url(r'^hello/$', views.hello),
```

使用正则与分组

在函数内需要接受year，month，day参数

```python
url(r'^(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2})/$', views.deta),
```

`deta`视图必须接收以下参数：

```python
def deta(request, year, month, day):
```

访问地址为：**http://127.0.0.1:8000/2016/11/19/**

传值

```python
url(r'^(?P<year>[0-9]{4})/$', views.id, {'foo': 'bar'}),
```

`id`函数必须接受**year**与**foo**参数

include分发，有利于解耦

```python
# 当访问二级路由是cmdb的时候转发给app01.urls处理
url(r'^cmdb/$', include('app01.urls')),
```

别名

主要用于前端的from表单验证，如下实例，URLs地址的时候，因为from表单提交的地址使用了别名，所以会自动替换：

```python
# urls.py
from django.conf.urls import url
from app01 import views
urlpatterns = [
    url(r'^index/$', views.index, name='bieming'),
]
# views.py
from django.shortcuts import render,HttpResponse
def index(request):
    if request.method=='POST':
        username=request.POST.get('username')
        password=request.POST.get('password')
        if username=='as' and password=='123':
            return HttpResponse("登陆成功")
    return render(request, 'index.html')
# index.html
<form action="{% url 'bieming' %}" method="post">
	 用户名:<input type="text" name="username">
	 密码:<input type="password" name="password">
	 <input type="submit" value="submit">
</form>
```

## 路由分解

可以使用`incloud`把很多个路由进行拆封，然后把不同的业务放到不同的urls中，首先我们创建项目及应用

```bash
# 创建DjangoProjects项目
E:\>django-admin.py startproject DjangoProjects

E:\>cd DjangoProjects
# 在项目内创建app1和app12应用
E:\DjangoProjects>python manage.py startapp app1

E:\DjangoProjects>python manage.py startapp app2

```

项目的**urls.py**文件内容

```python
# E:\DjangoProjects\DjangoProjects\urls.py
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    # 当路由匹配到一级路径为app1时，就把这个URL交给app1.urls再次进行匹配
    url(r'^app1/', include('app1.urls')),
    url(r'^app2/', include('app2.urls')),
]
```

应用的**urls.py**和**views.py**文件内容

```python
# E:\DjangoProjects\app1\urls.py
from django.conf.urls import url,include
from django.contrib import admin
from app1 import views
urlpatterns = [
    url(r'^hello/$', views.hello),
]

# E:\DjangoProjects\app1\views.py
from django.shortcuts import render,HttpResponse
def hello(request):
    return HttpResponse("Hello App1")
	
# E:\DjangoProjects\app2\urls.py
from django.conf.urls import url
from django.contrib import admin
from app2 import views
urlpatterns = [
    url(r'^hello/$', views.hello),
]

# E:\DjangoProjects\app2\views.py
from django.shortcuts import render,HttpResponse
def hello(request):
    return HttpResponse("Hello App2")
```

1. 当访问**http://127.0.0.1:8000/app1/hello/**时返回内容`Hello App1`
2. 当访问**http://127.0.0.1:8000/app2/hello/**时返回内容`Hello App2`

## 视图

http请求：**HttpRequest对象**
http响应：**HttpResponse对象**

HttpRequest对象属性

|属性|描述|
|:--|:--|
|request.path|请求页面的路径，不包括域名|
|request.get_full_path()|获取带参数的路径|
|request.method|页面的请求方式|
|request.GET|GET请求方式的数据|
|request.POST|POST请求方式的数据|

HttpResponse对象属性

|属性|描述|
|:--|:--|
|render(request, 'index.html')|返回一个模板页面|
|render_to_response('index.html')|返回一个模板页面|
|redirect('/login')|页面跳转|
|HttpResponseRedirect('/login')|页面跳转|
|HttpResponse('https://blog.ansheng.me')|给页面返回一个字符串|