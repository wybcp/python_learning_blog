# [Linux基础正则表达式

## 什么是正则表达式？

正则表达式是为了处理大量的字符串而定义的一套规则和方法，例如：@代表123456，当我查找123455这几个字符的时候我就可以用@代替，这样速度更快，Linux正则一般为行为单位处理的。

## 为什么要用正则表达式？

在企业里，我们每天的Linux运维工作中，时刻都会面对大量带有字符串的文本配置，程序、命令及日志文件等， 而我们经常会有迫切的需要，从大量的字符串内容中查找符合的字符串，

## 容易混淆的主事项

a.正则表达式应用非常广泛，存在各种语言中，列如：php,prl,java等，但是，我们今天讲的是系统运维工作中的正则表达式，即Linux表达式，最常应用的命令就是grep(egrep),sed,awk，换句话说Linux三剑客要像能工作的更高效，那一定离不开正则表达式的配合。

b.正则表达式和我们常用的通配符特殊字符有着本质区别；

**注意事项：**

a.LInux正则一般以行为单位处理的

b.alias grep='grep --color=auto'，讲课是grep为咧。

c.注意字符集 LC_ALL=C

## Linux基础正则表达式

基于grep来讲正则表达式，基础正则表达式.BRE(Basrc Regular Expression)

测试文件ansheng.txt内容：

```bash
[root@centos6 ~]# cat ansheng.txt 
My name is quiet!
I am a Linux Operation and Maintenance Engineer
My youth will stay at eighteen
I Hexo + Github made a static blog, blog address is: https://blog.ansheng.me
My mail: as@ansheng.me
QQ: 6087414
Thank you!
```

| 符号 | 说明 |
| :-------- | :----- |
| ^My | 搜索以My开头的 |

```bash
[root@ansheng ~]# grep --color=auto "^My" ansheng.txt 
My name is quiet!
My youth will stay at eighteen
My mail: as@ansheng.me
```

| 符号 | 说明 |
| :-------- | :----- |
| me$ | 搜索以me结尾的 |

```bash
[root@ansheng ~]# grep --color=auto "me$" ansheng.txt 
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
My mail: as@ansheng.me
```

| 符号 | 说明 |
| :-------- | :----- |
| ^$ | 表示空格 |

```bash
[root@ansheng ~]# grep -vn --color=auto "^$" ansheng.txt 
1:My name is quiet!
2:I am a Linux Operation and Maintenance Engineer
3:My youth will stay at eighteen
4:I Hexo + Github made a static blog, blog address is: https: //ansheng.me
5:My mail: as@ansheng.me
6:QQ: 6087414
7:Thank you!
```

| 符号 | 说明 |
| :-------- | :----- |
| . | 代表且只能代表任意一个字符 |

```bash
[root@ansheng ~]# grep --color=auto "." ansheng.txt 
My name is quiet!
I am a Linux Operation and Maintenance Engineer
My youth will stay at eighteen
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
My mail: as@ansheng.me
QQ: 6087414
Thank you!
```

> 全部都过滤出来了！

| 符号 | 说明 |
| :-------- | :----- |
| \ | 转义符，让有着特殊意义的字符变成没有任何意义的字符 |

```bash
[root@ansheng ~]# grep --color=auto "\." ansheng.txt 
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
My mail: as@ansheng.me
```

> 只过滤包含.的行

| 符号 | 说明 |
| :-------- | :----- |
| * | 重复0个或者多个前面的一个字符 |

```bash
[root@ansheng ~]# grep --color=auto "0*" ansheng.txt 
My name is quiet!
I am a Linux Operation and Maintenance Engineer
My youth will stay at eighteen
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
My mail: as@ansheng.me
QQ: 6087414
Thank you!
```

| 符号 | 说明 |
| :-------- | :----- |
| .* | 匹配任意字符 |

```bash
[root@ansheng ~]# grep --color=auto ".*" ansheng.txt 
My name is quiet!
I am a Linux Operation and Maintenance Engineer
My youth will stay at eighteen
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
My mail: as@ansheng.me
QQ: 6087414
Thank you!
```

> .匹配的是任意一个字符，.*匹配的是任意0个或者个多个，所以才会有空行

| 符号 | 说明 |
| :-------- | :----- |
| [abc] | 匹配字符集合内的任意一个字符 |

```bash
[root@ansheng ~]# grep --color=auto "[abc]" ansheng.txt 
My name is quiet!
I am a Linux Operation and Maintenance Engineer
My youth will stay at eighteen
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
My mail: as@ansheng.me
Thank you!
```

| 符号 | 说明 |
| :-------- | :----- |
| [^abc] | 匹配不包含^后的任意字符的内容 |

```bash
[root@ansheng ~]# grep --color=auto "[^abc]" ansheng.txt 
My name is quiet!
I am a Linux Operation and Maintenance Engineer
My youth will stay at eighteen
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
My mail: as@ansheng.me
QQ: 6087414
Thank you!
```

| 符号 | 说明 |
| :-------- | :----- |
| a\{n.m\} | 前面的字符重复N到M次 |

```bash
[root@ansheng ~]# grep --color=auto "4\{1,3\}" ansheng.txt  
QQ: 6087414
```

> 重复4一到三次

| 符号 | 说明 |
| :-------- | :----- |
| \{n,\} | 前面的字符重复至少N次 |

```bash
[root@ansheng ~]# grep --color=auto "4\{1,\}" ansheng.txt  
QQ: 6087414
```

| 符号 | 说明 |
| :-------- | :----- |
| \{n\} | 前面的字符重复N次 |

```bash
[root@ansheng ~]# grep --color=auto "d\{2\}" ansheng.txt 
I Hexo + Github made a static blog, blog address is: https: //ansheng.me
```
## 扩展的正则表达式（egrep或者grep -E）

扩展正则表达式 ERE Extended Regular Expressions

测试文件内容：

```bash
[root@ansheng ~]# cat linux.txt
Linux is a good 
god assdxw bcvnbvbjk
greatttttt  wexcvxc
operaaaating  dhfghfvx
gooodfs awrerdxxhkl
gdsystem awxxxx
glad
good
```

| 符号 | 说明 |
| :-------- | :----- |
| + | 重复前一个字符一个或一个以上 |

```bash
[root@ansheng ~]# egrep --color=auto "go?d" linux.txt  
god assdxw bcvnbvbjk
gdsystem awxxxx
```

| 符号 | 说明 |
| :-------- | :----- |
| ？ | 0个或者一个字符 |

```bash
[root@ansheng ~]# egrep --color=auto "go?d" linux.txt   
god assdxw bcvnbvbjk
gdsystem awxxxx
```

| 符号 | 说明 |
| :-------- | :----- |
| | | 表示或，查找多个字符串 |

```bash
[root@ansheng ~]# egrep --color=auto "gd|good" linux.txt 
Linux is a good 
gdsystem awxxxx
good
```

| 符号 | 说明 |
| :-------- | :----- |
| （） | 分组过滤匹配 |

```bash
[root@ansheng ~]# egrep --color=auto "g(la|oo)d" linux.txt 
Linux is a good 
glad
good
```

**批注：**上面的命令请自行加参数"--color=auto"，可以把过滤出来的字符串用颜色显示出来，我这里写的文档没有颜色的提示，如果要需要自己动手操作哦~