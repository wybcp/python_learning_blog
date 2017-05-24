# JavaScript基础

## JavaScript代码块存在形式

存在HTML文档内

```HTML
<script>
  alert("asdasd");
</script>
```

以文件的方式进行引入 

```HTML
<script src="s1.js"></script>
```

`JavaScript`代码块尽量放在文档的尾部，这样等`HTML`文档上面的代码和`CSS`样式加载完成之后才会去加载`JavaScript`代码，当然这也不是绝对的。

## 注释

```JavaScript
// 单行注释
/* 多行注释 */
```

## 变量

局部变量

```JavaScript
var a = 1;
```

全局变量

```JavaScript
a = 1;
```

例子

```JavaScript
// 创建一个全局变量name=as
name = "as";
// 创建一个桉树
function hello(){
    // 在函数内创建一个变量naem=ansheng
    var name = "ansheng";
    // 在函数内输出name的值
    console.log(name)
}
// 执行函数
hello();
// 输出name的值
console.log(name)
```

输出结果如下

![javascript-foundation-01](../images/2016/12/1483064156.png)

如果把函数内的`var name = "ansheng";`换成`name = "ansheng";`，那么输出如下

![javascript-foundation-02](../images/2016/12/1483064182.png)

## 数据类型

`JavaScript`中的数据类型分为原始类型和对象类型

**原始类型**

1. 数字
2. 字符串
3. 布尔值

**对象类型**

1. 数组
2. 字典
3. 日期
4. 函数

特别的，数字、布尔值、null、undefined、字符串是不可变。

null是JavaScript语言的关键字，它表示一个特殊值，常用来描述**空值**,**undefined**是一个特殊值，表示变量未定义。

```javascript
var n = null;
console.log("n is val:" + n);

var u = undefined;
console.log("u is val:" + u);

var hello;
console.log("hello is val:" + hello);

function un(){
    // 在输出word的时候这个变量还没有被定义，所以是undefined
    console.log("word is val:" + word);
    var word = "as";
}
un();
```

输出如下
![javascript-foundation-03](../images/2016/12/1483064225.png)

### 数字(Number)

数字类型在Javascript中代表了所有的数字，包括小数、负数、长整数，统称数字类型。

**创建数字类型**

```javascript
// 直接创建了一个原始值
num1 = 123;

// 通过Number类创建一个对象
num2 = new Number(456);
```

**数据类型转换**

```javascript
// 创建一个字符串
s = "123";

// 将字符串转换成数字
n = parseInt(s);
// 转换成浮点
f = parseFloat(s);

console.log(n,f);
```

**NaN非数字**

例如下面的例子中如果两个值都转成数字类型，如果转换成功就输出`success`，并把之打印出来，如果转换失败就输出`error`，并把值打印出来：

```javascript
s1 = "uuii";
s2 = "123";
n1 = parseInt(s1);
n2 = parseInt(s2);

if (isNaN(n1)){
    console.log("n1 is error, val= " + n1);
}else{
    console.log("n1 is success, val= " + n1);
};

if (isNaN(n2)){
    console.log("n2 is error, val= " + n2);
}else{
    console.log("n2 is success, val= " + n2);
};
```
输出
![javascript-foundation-04](../images/2016/12/1483064258.png)

|方法|描述|
|:--|:--|
|`Number.NEGATIVE_INFINITY`|负无穷大|
|`Number.POSITIVE_INFINITY`|正无穷大，可使用isFinite(num)来判断|
|`Number.toString()`|将—个数字转换成字符串|
|`Number.toExponential( )`|用指数计数法格式化数字|
|`Number.toFixed( )`|保留小数点后几位|
|`typeof()`|判断变量的数据类型|
|`isNaN()`|判断转换数字类型是否转换成功|

参考:
http://www.shouce.ren/api/javascript/l_Number.html

### 字符串类型

字符串是由字符组成的数组，但在JavaScript中字符串是不可变的：可以访问字符串任意位置的文本，但是JavaScript并未提供修改已知字符串内容的方法。

**常见功能：**

```javascript
// 创建一下变量用于测试
str = "hello world";
tri = " a b ";
r = "v2h";
```

|功能|描述|结果|
|:--|:--|:--|
|`str[2];`|索引取值|`"l"`|
|`str.slice(1, 3);`|切片取值|`"el"`|
|`str.length;`|获取字符串长度|`11`|
|`tri.trimLeft();`|移除左边空白|`"a b "`|
|`tri.trimRight();`|移除右边空白|`" a b"`|
|`tri.trim();`|移除两边空白|`"a b"`|
|`str.charAt(1);`|返回字符串的第一个字符|`"e"`|
|`str.concat(value, "hw");`|字符串拼接|`"hello world a b hw"`|
|`str.indexOf("o", 5);`|从前面开始查找|`7`|
|`str.lastIndexOf('o');`|从后面开始查找|`6`|
|`str.substring(4, 7);`|根据索引查找|`"o w"`|
|`str.toLowerCase();`|字符串全部变小写|`"hello world"`|
|`str.toUpperCase();`|字符串全部变大写|`"HELLO WORLD"`|
|`r.split(/2/);`|根据o进行字符串分割|`["v", "h"]`|
|`r.split(/\d+/);`|可以使用正则表达式|`["v", "h"]`|
|`str.search(/o/);`|查找字符串索引位置,从头开始匹配|`4`|
|`str.match(/o/g);`|显示查找到的所有字符串，正则后面加g|`["o", "o"]`|
|`str.replace(/o/, "m");`|把匹配到的第一个字符串替换成m|`"hellm world"`|
|`str.replace(/o/g, "m");`|匹配到的全部替换|`"hellm wmrld"`|
|`str.replace(/o/g, "$&" + "V");`|把匹配到的字符串后面都加一个V|`"helloV woVrld"`|
|`r.replace(/(\w+)\d+(\w+)/g, "$1$2");`|把两边匹配到字符串删掉|`"vh"`|
|`r.replace(/\d+/g, "$$");`|两个$$代表一个|`"v$h"`|
|`r.replace(/2/, '$`');`|$`：位于匹配子串左侧的文本|`"vvh"`|
|`r.replace(/2/, "$'");`|$'：位于匹配子串右侧的文本|`"vhh"`|

### 布尔类型(Boolean) 

布尔类型只有两种：

1. `true`
2. `false`

|符号|描述|
|:--|:--|
|`==`|值一样则为`true`|
|`====`|值一样并且类型也一样则为`true`|
|`!=`|值不一样则为`true`|
|`!==`|值不一样并且类型也不一样则为`true`|
|`俩竖线`|或|
|`&&`|且|

测试如下：
![javascript-foundation-05](../images/2016/12/1483064288.png)

### 数组(集合)

创建一下数组用于测试

```javascript
obj = ['a','b','c'];
obj_h = [1,2];
obj_s = [1,"a",2,"d","h"];
```

|功能|描述|结构|
|:--|:--|:--|
|`obj.length;`|获取数组内元素的个数|`3`|
|`obj.push("d");`|尾部追加元素|`4`|
|`obj.pop();`|删除尾部的元素|`"d"`|
|`obj.unshift("e");`|头部插入元素|`4`|
|`obj.shift()`|删除头部的元素|`"e"`|
|`obj.splice(start, deleteCount, value, ...)`|增删改数组的元素||
|`obj.splice(1,0,"h");`|第一个位置上插入"h"|`[]`|
|`obj.splice(1,1,"v");`|第一个位置上的h替换成v|`["h"]`|
|`obj.splice(1,1);`|删除第一个位置上的元素|`["v"]`|
|`obj.slice(2,3);`|切片|`["c"]`|
|`obj.reverse();`|元素反转|`["c", "b", "a"]`|
|`obj.join("_");`|将元素进行拼接|`"c_b_a"`|
|`obj.concat(obj_h);`|数组之间进行拼接|`["c", "b", "a", 1, 2]`|
|`obj_s.sort();`|对元素进行排序|`[1, 2, "a", "d", "h"]`|

### 伪字典

通过对象创建一个字典

```javascript
 dic = { "k1": 1, "k2": 2 };
```

|参数|描述|结果|
|:--|:--|:--|
|`dic["k1"];`|取值|`1`|
|`delete dic["k1"];`|删除|`true`|
|`dic['k3'] = 3;`|添加一个值|`3`|

### 数据类型检测

**typeof**

适合基本类型及function检测，遇到null则失效

```javascript
var str = "string";
typeof(str);
// "string"
n = null;
// null
typeof(n);
// "object"
```

**instanceof**

是和自定义对象检测，也可以用来检测原生对象，在不同的`window`或`iframe`间检测时失效

```javascript
// 创建两个函数Person和Student
function Person(){};
function Student(){};
Student.prototype = new Person;
Student.prototype.constructor = Student;
// 创建一个bosn对象，由Student
var bosn = new Student();
// 判断bosn是否由Student对象创建
bosn instanceof Student;
// true
var one = new Person();
one instanceof Person;
// true
one instanceof Student;
// false
bosn instanceof Person;
// true
```

**prototype**

适合`{}.toString`拿到，适合内置对象和基本元类型

```javascript
Object.prototype.toString.apply([]);
// "[object Array]"
Object.prototype.toString.apply(function(){});
// "[object Function]"
Object.prototype.toString.apply(null);
// "[object Null]"
Object.prototype.toString.apply(undefined);
// "[object Undefined]"
``

## 序列化

创建两个变量

```JavaScript
k = [11, 22, 33, 44];
v = "[11,22,33,44]";
```

|参数|描述|结果|
|:--|:--|:--|
|`JSON.stringify(k);`|将数据类型序列化成字符串|`"[11,22,33,44]"`|
|`JSON.parse(v);`|将字符串序列化成数据类型|`[11, 22, 33, 44]`|

## URL转义

```JavaScript
// 创建一个变量s
s = "http://ansheng.me/%E4%BD%A0%E5%A5%BD%E4%B8%96%E7%95%8C";
v = "http://ansheng.me/你";
```

|属性|描述|结果|
|:--|:--|:--|
|`encodeURI(v);`|把URI中的字符进行转义|`"http://ansheng.me/%E4%BD%A0"`|
|`decodeURI(s);`|显示URl中被转义的字符|`"http://ansheng.me/你好世界"`|
|`encodeURIComponent(v);`|转义URL中所有的字符||
|`decodeURIComponent(s);`|显示被转移的URL所有字符|
|`escape(v);`|对字符串转义|`"http%3A//ansheng.me/%u4F60"`|
|`unescape(s);`|给转义字符串解码|`"http://ansheng.me/ä½ å¥½ä¸ç"`|
|`URIError`|由URl的编码和解码方法抛出|

## eval

JavaScript中的eval是Python中eval和exec的合集，既可以编译代码也可以获取返回值。

|属性|描述|结果|
|:--|:--|:--|
|`eval("1+1");`|执行字符串中的JavaScript代码|`2`|
|`EvalError`|||

## 正则表达式

JavaScript中支持正则表达式，其主要提供了两个功能：

|方法|描述|
|:--|:--|
|`test(string);`|用于检测正则是否匹配|
|`exec(string);`|用于获取正则匹配的内容|

```JavaScript
// 创建一个变量a
a = 1;
// 创建一个表达式r
r = /\d/;
// 表达式测试,匹配成功则返回true
a.test(/\d/);

// 获取表达式匹配出来的结果，["1"]
r.exec(a)
```

注：定义正则表达式时，**"g"、"i"、"m"**分别表示全局匹配，忽略大小写、多行匹配。

## 时间处理

JavaScript中提供了时间相关的操作，时间操作中分为两种时间：

1. 时间统一时间
2. 本地时间（东8区）

```javascript
// 创建一个事件对象
date = new Date()
// date = Sat Aug 13 2016 16:29:59 GMT+0800 (中国标准时间)
```

更多操作参见：http://www.shouce.ren/api/javascript/main.html