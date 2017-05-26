# Python全栈之路系列之Django Cookie与Sessi

## Cookies

`cookies`是浏览器为Web服务器存储的一小段信息,每次浏览器从某个服务器请求页面时，它向服务器回送之前收到的`cookies`.

### 存取Cookies

创建**Cookies**

```python
def set_cookie(request):
    # 创建HttpResponse对象
    Response = HttpResponse()
    # 创建cookie
    Response.set_cookie("CookieKey", "CookieValue")
    # 删除cookie
    # Response.delete_cookie("CookieKey")
    return Response
```

获取**Cookies**

```python
def show_cookie(request):
    # 如果cookie存在
    if "CookieKey" in request.COOKIES:
        # cookie的值并赋值给VAL
        VAL = request.COOKIES["CookieKey"]
    # 如果不存在
    else:
        # VAL=Cookie不存在
        VAL = "Cookie不存在"
    return HttpResponse(VAL)
```

测试Cookies

Django提供了一个简单的方法来测试用户的浏览器是否接受cookie：

```python
# urls.py
from django.conf.urls import url
from django.contrib import admin
from DjangoProjects import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^test_cookie/', views.test_cookie),
    url(r'^hello/', views.hello),
]

# views.py
from django.http import HttpResponse

def test_cookie(request):
    # 植入测试的cookie
    request.session.set_test_cookie()
    return HttpResponse("a")

def hello(request):
    # 获取测试的cookie
    ret = request.session.test_cookie_worked()
    print(ret)
    if ret:
        VAL = "验证成功"
        # 删除测试的cookie
        request.session.delete_test_cookie()
    else:
        VAL = "验证失败"
    return HttpResponse(VAL)
```

创建**Cookies**时的参数

|参数|默认值|描述|
|:--|:--|:--|
|`max_age`|`None`|cookie生存时间，如果参数是None，这个cookie会延续到浏览器关闭为止|
|`expires`|`None`|cookie失效的实际日期/时间，格式必须是：`Wdy, DD-Mth-YY HH:MM:SS GMT`，这个参数会覆盖max_age参数|
|`path`|`"/"`|cookie生效的路径前缀|
|`domain`|`None`|这个cookie在那个站点生效，如果为`None`，就是当前站点，如果设为`domain=".example.com"`，则所有带`example.com`的二级域名站点都可读到cookie|
|`False`|`False`|如果设置为**True**，浏览器将通过HTTPS来回传cookie|

## Session

你可以用session框架来存取每个访问者任意数据，这些数据在服务器端存储，并对cookie的收发进行了抽象，Cookies只存储数据的哈希会话ID，而不是数据本身，从而避免了大部分的常见cookie问题。

### 开启Django内的Session功能

编辑**settings.py**配置文件，修改配置如下：

1. 找到`MIDDLEWARE`段，确保列表里有**'django.contrib.sessions.middleware.SessionMiddleware',**字段
2. 找到`INSTALLED_APPS`段，确保列表里有**'django.contrib.sessions',**字段

编辑**settings.py**配置文件，找到**DATABASES**字段进行数据库的配置：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mydatabase',
        'USER': 'root',
        'PASSWORD': 'as',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```
然后再项目的`__init__.py`文件加入以下两行配置：
```python
import pymysql
pymysql.install_as_MySQLdb()
```
生成数据表
```bash
E:\DjangoProjects>python manage.py migrate
```

### 在视图内使用Session

`SessionMiddleware`激活后，每个传给视图(view)函数的第一个参数`HttpRequest`对象都有一个`session`属性，这是一个字典型的对象，你可以象用普通字典一样来用它。

```python
from django.http import HttpResponse

def set_session(request):
    # 设置一个session
    request.session["SessionKey"] = "SessionValue"
    # 返回页面一个字符串
    return HttpResponse("session")

def show_session(request):
    # 判断session是否存在
    if "SessionKey" in request.session:
        # 获取session的值
        ret = request.session["SessionKey"]
        # 删除session
        del request.session["SessionKey"]
    else:
        ret = None
    return HttpResponse(ret)
```

其他的映射方法，如**keys()**和**items()**对**request.session**同样有效。

**session**的数据存放在**django_session**表内

```bash
mysql> use mydatabase
Database changed
mysql> select * from django_session;
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+----------------------------+
| session_key                      | session_data                                                                                                                             | expire_date                |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+----------------------------+
| bvthtagcl257hiv8v2wfqnf03m5268sg | ZjY4ZGM1MzRiZGExOGNhMjI0ODBlMjZjM2JhYjU5ODU2MzU5MjM1Mzp7IjAiOjAsIjEiOjEsIjIiOjIsIjMiOjMsIjQiOjQsIjUiOjUsIjYiOjYsIjciOjcsIjgiOjgsIjkiOjl9 | 2016-08-25 08:33:31.281157 |
+----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------+----------------------------+
1 row in set (0.00 sec)
```

### 在视图外使用Session

进入带`django`环境变量的Python解释器
```bash
E:\DjangoProjects>python manage.py shell
Python 3.5.2 (v3.5.2:4def2a2901a5, Jun 25 2016, 22:18:55) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>>
```
```python
>>> from django.contrib.sessions.models import Session
# pk后面的字符是从django_session表内获取的session_key
>>> s = Session.objects.get(pk='bvthtagcl257hiv8v2wfqnf03m5268sg')
>>> s.expire_date
datetime.datetime(2016, 8, 25, 8, 40, 56, 709091, tzinfo=<UTC>)
# 这是经过加密的数据
>>> s.session_data
'NjdiZGYyODljOTNlMTQ3NmFjZTc0YzRlMmVjNmExYTc1NjZkNzUzNDp7IjAiOjAsIjMiOjMsIjkiOjksIjQiOjQsIjEiOjEsIjciOjcsIjIiOjIsIjYiOjYsIjUiOjUsIjgiOjh9'
# 使用get_decoded()来读取实际的session数据
>>> s.get_decoded()
{'8': 8, '9': 9, '2': 2, '5': 5, '1': 1, '6': 6, '4': 4, '7': 7, '3': 3, '0': 0}
```

默认情况下，session只会在发生变化的时候才会存入数据库，比如说，字典赋值或删除，你可以设置`SESSION_SAVE_EVERY_REQUEST`为`True`来改变这一缺省行为，如果置为True的话，会话cookie在每次请求的时候都会送出，同时，每次会话cookie送出的时候，其`expires`参数都会更新。


### 设置Session

**SESSION_EXPIRE_AT_BROWSER_CLOSE**

如果cookie没有设置过期时间，当用户关闭浏览器的时候，cookie就自动过期了，你可以改变`SESSION_EXPIRE_AT_BROWSER_CLOSE`的设置来控制session框架的这一行为。

缺省情况下，`SESSION_EXPIRE_AT_BROWSER_CLOSE`设置为`False`，这样，会话cookie可以在用户浏览器中保持有效达`SESSION_COOKIE_AGE`秒（缺省设置是两周，即1,209,600 秒），如果你不想用户每次打开浏览器都必须重新登陆的话，用这个参数来帮你。

如果`SESSION_EXPIRE_AT_BROWSER_CLOSE`设置为`True`，当浏览器关闭时，Django会使cookie失效

**SESSION_COOKIE_DOMAIN**

使用会话cookie（session cookies）的站点，将它设成一个字符串，就好象`“.example.com”`以用于跨站点（cross-domain）的cookie，或`None`以用于单个站点，默认为**None**

**SESSION_COOKIE_NAME**

会话中使用的cookie的名字。 它可以是任意的字符串。默认为**sessionid**

**SESSION_COOKIE_SECURE**

是否在session中使用安全cookie，如果设置True , cookie就会标记为安全，这意味着cookie只会通过HTTPS来传输，默认为**False**