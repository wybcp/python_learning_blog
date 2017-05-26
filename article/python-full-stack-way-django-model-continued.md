# Python全栈之路系列之Django模型续

## 连表操作一对一

在`app`的**models.py**文件内添加以下内容用户创建一对多关系的表：

```python
from django.db import models

# Create your models here.

class UserType(models.Model):
    nid = models.AutoField(primary_key=True)
    caption = models.CharField(max_length=32)

class UserInfo(models.Model):
    username = models.CharField(max_length=32)
    password = models.CharField(max_length=16)
    user_type = models.ForeignKey('UserType')
```

把`app`的名字添加到项目的`settings.py`配置文件的**INSTALLED_APPS**中，然后再数据库中生成表：

```bash
E:\DjangoProjects>python manage.py makemigrations
E:\DjangoProjects>python manage.py migrate
```

### 基本操作

进入带有**django**环境变量的项目，然后把模型导入进去，用于做下面的操作

```bash
E:\DjangoProjects>python manage.py shell
Python 3.5.2 (v3.5.2:4def2a2901a5, Jun 25 2016, 22:18:55) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from app01 import models
```

添加数据

```bash
# 通过create方式进行数据的添加
>>> models.UserType.objects.create(caption='版主')
<UserType: UserType object>
```

```bash
# 通过save保存的方式添加数据
>>> obj = models.UserType(caption='管理员')
>>> obj.save()
```

```bash
# 通过字典的方式进行数据添加
>>> UserInfoDict = {'username':'ansheng','password':'helloword','user_type': models.UserType.objects.get(nid=1)}
# 通过**UserInfoDict把数据以字典方式传给create
>>> models.UserInfo.objects.create(**UserInfoDict)
<UserInfo: UserInfo object>
>>> UserInfoDict = {'username':'hello','password':'word','user_type': models.UserType.objects.get(nid=2)}
>>> models.UserInfo.objects.create(**UserInfoDict)
<UserInfo: UserInfo object>
```

```bash
# 如果知道user_type_id代表多少，那么也可以直接写数字
>>> UserInfoDict = {'username':'ansheng','password':'helloword','user_type_id': 2}
>>> models.UserInfo.objects.create(**UserInfoDict)
<UserInfo: UserInfo object>
```

修改数据

```bash
# 指定条件更新
>>> models.UserInfo.objects.filter(password='helloword').update(password='hw')
1
```

```bash
# 获取id=1的这条数据对象
>>> obj = models.UserInfo.objects.get(id=1)
# 把username字段修改成as
>>> obj.username = 'as'
# 保存操作
>>> obj.save()
```

删除数据

```bash
>>> models.UserInfo.objects.filter(username='ansheng', user_type_id='2').delete()
(1, {'app01.UserInfo': 1})
```

查询数据

```bash
# 获取单条数据，不存在则报错
>>> models.UserInfo.objects.get(id=1)
<UserInfo: UserInfo object>
>>> models.UserInfo.objects.get(id=23)
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "C:\Python\Python35\lib\site-packages\django\db\models\manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "C:\Python\Python35\lib\site-packages\django\db\models\query.py", line 385, in get
    self.model._meta.object_name
app01.models.DoesNotExist: UserInfo matching query does not exist.
```

```bash
# 获取全部数据
>>> models.UserInfo.objects.all()
<QuerySet [<UserInfo: UserInfo object>]>
```

```bash
# 获取指定条件的数据
>>> models.UserInfo.objects.filter(username='as')
<QuerySet [<UserInfo: UserInfo object>]>
```

### 单表查询

查询出来的结果都是queryset对象

query方法

query是用来查看查询语句的，即django生成的SQL

```bash
>>> ret = models.UserType.objects.all()
>>> print(ret.query)
SELECT `app01_usertype`.`nid`, `app01_usertype`.`caption` FROM `app01_usertype`
```

values与values_list

```bash
>>> ret = models.UserType.objects.all().values('nid')
# 返回的列表,列表里面套字典
>>> print(type(ret),ret)
<class 'django.db.models.query.QuerySet'> <QuerySet [{'nid': 1}, {'nid': 2}]>
```

```bash
>>> ret = models.UserType.objects.all().values_list('nid')
# 返回一个列表，列表里面套集合
>>> print(type(ret),ret)
<class 'django.db.models.query.QuerySet'> <QuerySet [(1,), (2,)]>
```

双下划线连表操作

```bash
>>> ret = models.UserInfo.objects.all().values('username','user_type__caption')
# INNER
>>> print(ret.query)
SELECT `app01_userinfo`.`username`, `app01_usertype`.`caption` FROM `app01_userinfo` INNER JOIN `app01_usertype` ON (`app01_userinfo`.`user_type_id` = `app01_usertype`.`nid`)
```

```bash
>>> ret = models.UserInfo.objects.all()
>>> for item in ret:
...  print(item,item.id,item.user_type.nid,item.user_type.caption,item.user_type_id)
...
UserInfo object 1 1 版主 1
```

### 查询实例

获取用户类型是超级管理员的所有用户

正向查找

通过双下划线连表查询

```bash
>>> ret = models.UserInfo.objects.filter(user_type__caption = "管理员").values('username','user_type__caption')
>>> print(ret,type(ret),ret.query)
<QuerySet [{'user_type__caption': '管理员', 'username': 'hello'}]> <class 'django.db.models.query.QuerySet'> SELECT `app01_userinfo`.`username`, `app01_usertype`.`caption` FROM `app01_userinfo` INNER JOIN `app01_usertype` ON (`app01_userinfo`.`user_type_id` = `app01_usertype`.`nid`) WHERE `app01_usertype`.`caption` = 管理员
```

反向查找

先查找UserType表中数据，再把这个数据和UserInfo表中进行过滤

```bash
>>> obj = models.UserType.objects.filter(caption= '管理员').first()
>>> print(obj.nid, obj.caption)
2 管理员
>>> print(obj.userinfo_set.all())
<QuerySet [<UserInfo: UserInfo object>]>
```
把UserType表里的所有字段和userinfo表进行一个匹配，如果有匹配到就显示出来
```bash
>>> ret = models.UserType.objects.all().values('nid','caption','userinfo__username')
>>> print(ret)
<QuerySet [{'userinfo__username': 'as', 'nid': 1, 'caption': '版主'}, {'userinfo__username': 'hello', 'nid': 2, 'caption': '管理员'}]>
```

## 连表操作多对多

两种创建多对多表的方式

**手动指定第三张表进行创建**

```python
class HostGroup(models.Model):
    hgid = models.AutoField(primary_key=True)
    host_id = models.ForeignKey('Host')
    group_id = models.ForeignKey('Group')

class Host(models.Model):
    hid = models.AutoField(primary_key=True)
    hostname = models.CharField(max_length=32)
    ip = models.CharField(max_length=32)

class Group(models.Model):
    gid = models.AutoField(primary_key=True)
    name = models.CharField(max_length=16)
    # 指定第三张表
    h2g = models.ManyToManyField('Host', through='HostGroup')
```


**django帮我们创建第三张表**

创建以下表关系用于测试多对多

```bash
class Host(models.Model):
    hid = models.AutoField(primary_key=True)
    hostname = models.CharField(max_length=32)
    ip = models.CharField(max_length=32)

class Group(models.Model):
    gid = models.AutoField(primary_key=True)
    name = models.CharField(max_length=16)
    # 任意一个字段，会自动生成第三张表，且第三张表会自动的添加联合唯一索引，Unique
    h2g = models.ManyToManyField('Host')
```

插入以下数据用于测试

```bash
# Host插入数据
>>> models.Host.objects.create(hostname='localhost', ip='192.168.1.1')
<Host: Host object>
>>> models.Host.objects.create(hostname='linux-node1', ip='192.168.1.2')
<Host: Host object>
>>> models.Host.objects.create(hostname='linux-node2', ip='192.168.1.3')
<Host: Host object>
>>> models.Host.objects.create(hostname='web-node1', ip='192.168.1.4')
<Host: Host object>
```

```bash
# Group插入数据
>>> models.Group.objects.create(name='市场部')
<Group: Group object>
>>> models.Group.objects.create(name='技术部')
<Group: Group object>
>>> models.Group.objects.create(name='财务部')
<Group: Group object>
>>> models.Group.objects.create(name='运维部')
<Group: Group object>
>>> models.Group.objects.create(name='销售部')
<Group: Group object>
```

单个添加数据

```bash
# 获取组中gid=1的这条数据
>>> obj = models.Group.objects.get(gid=1)
# 获取表中的内容
>>> obj.gid, obj.name
(1, '市场部')
# 获取第三张表的内容
>>> obj.h2g.all()
<QuerySet []>
# 获取一个主机
>>> h1 = models.Host.objects.get(hid=2)
# 获取主机的IP
>>> h1.ip
'192.168.1.2'
# 把主机hid=2和组gid=1的这两条数据做一个对应关系，放入第三章比哦中
>>> obj.h2g.add(h1)
# 查看输入
>>> obj.h2g.all()
<QuerySet [<Host: Host object>]>
```

批量添加

```bash
# 获取GID大于2的所有主机
>>> h = models.Host.objects.filter(hid__gt=2)
# 获取到两条数据
>>> h
<QuerySet [<Host: Host object>, <Host: Host object>]>
# 把上面的主机添加到第三张表内
>>> obj.h2g.add(*h)
# 添加的数据总和为三条
>>> obj.h2g.all()
<QuerySet [<Host: Host object>, <Host: Host object>, <Host: Host object>]>
```

反向操作

上面的例子中我们往一个部门中添加了多台机器，那么现在我们将一个机器添加到多个部门中

```bash
# 获取ip='192.168.1.3'的这台机器
>>> host = models.Host.objects.get(ip='192.168.1.3')
# 把上面的那台机器添加到gid大于2的所有部门内
>>> host.group_set.add(*models.Group.objects.filter(gid__gt=2))
```

### 多对多操作的一些奇葩方法

remove()

````bash
# 删除在关系表中gid=3的这条数据
>>> host.group_set.remove(*models.Group.objects.filter(gid=3))
```

delete()

```bash
# 原始数据和关系表中的数据都删除
>>> host.group_set.all().delete()
# 总影响行数8，在app01.Group内删除了3条，在app01.Group_h2g删除了5条
(8, {'app01.Group': 3, 'app01.Group_h2g': 5})
```

为了不影响后面的测试，请重新恢复以下数据

```bash
models.Group.objects.create(name='市场部')
models.Group.objects.create(name='技术部')
models.Group.objects.create(name='财务部')
models.Group.objects.create(name='运维部')
models.Group.objects.create(name='销售部')
```

set()

```bash
如果传入过来的数据在表中没有，那么就添加进来，如果有那么就进行移除
接收一个参数clear，如果clear=true，那么就先全部清除，然后再添加，默认clear=false
>>> host.group_set.set(models.Group.objects.filter(gid=6))
```

create()

```bash
# gid大于8的全部加入进来，相当于add()
>>> host.group_set.add(*models.Group.objects.filter(gid__gt=8))
```

get_or_create()

```bash
# 如果有就获取，没有就添加，元数据也会被添加
>>> host =models.Host.objects.get(ip='192.168.1.3')
>>> r = host.group_set.get_or_create(name='技术部')
# 如果没有，返回True，并添加数据
>>> r
(<Group: Group object>, True)
# 已经有了，返回False，不执行任何操作
>>> r = host.group_set.get_or_create(name='技术部')
>>> r
(<Group: Group object>, False)
```

update_or_create()

与上述方法一致

### 自定义第三张表特性

1. 自定义的第三张表不会创建联合唯一索引，Unique
2. 在对第三张表操作的时候，只能使用第三张表明进行操作

第三张表添加约束

```bash
# 在第三张表内添加下面的类
class Meta:
	unique_together = [
		('host_id', 'group_id')
	]
```

插入一条数据

```bash
# 两个外键都会加上_id
>>> models.HostGroup.objects.create(host_id_id=1, group_id_id=1)
<HostGroup: HostGroup object>
```