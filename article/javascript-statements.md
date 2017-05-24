# JavaScript语句

## 条件语句

JavaScript中支持两个中条件语句，分别是：`if`和`switch`

### if

```javascript
if(条件){
  代码块;
}else if(条件){
// }elif(条件){ 
  代码块;
}else{
  代码块;
};
```

### if实例

```javascript
a = 456;

if (a == 123) {
    console.log("a = 123");
} else if (a == 456) {
    console.log("a == 456");
} else {
    console.log("a is null");
}
```

输出结果为：`a == 456`

## switch

```javascript
switch(变量名){
  case '值1':
      代码块;
      break;
  case '值2':
      代码块;
      break;
  default :
      代码块;
}
```

## switch实例

```javascript
var name = "as";
switch (name) {
    case "a":
        console.log("name == a");
        break;
    case "as":
        console.log("name == as");
        break;
    default:
        console.log("name is null");
        break;
};
```
输出结果为：`name == as`

```javascript
var val = 2;
switch (val) {
    case 1:
    case 2:
    case 3:
        console.log("1,2,3");
        break;
    case 4:
        console.log("4");
        break;
    default:
        console.log("defaule");
}
```
如果`val=1,2,3`都输出`1,2,3`,结果也是`1,2,3`

## 循环语句

JavaScript中支持三种循环语句

定义一个列表和一个字典:

```javascript
var li = [1, 2, 3, 4, 5, 6];
var dic = { "k1": "v1", "k2": "v2", "k3": "v3" };
```

第一种循环列表方式

```javascript
for (var i = 0; i < li.length; i++) {
    console.log("索引是：" + i);
    console.log("索引的值是：" + li[i]);
};
```

第二种循环字典方式

```javascript
for (var item in dic) {
    console.log("Key是：" + item);
    console.log("Value是：" + dic[item]);
};
```

第三种循环语句

```javascript
while(条件){
    // 跳出循环
    break;
    // 跳出当前循环
    continue;
};
```

## 异常处理语句

```javascript
try {
    // 主动抛出异常 
    throw Error('xxxx')；
    // 首先执行这块代码，如果出错就执行catch中的代码
}
catch (e) {
    // 如果try中的代码没有抛出异常，则这里的代码将不会执行
    // e是一个局部变量，用来指向Error对象或者其他抛出的对象
}
finally {
     // 无论try或catch中代码是否有异常抛出，甚至是try代码块中有return语句，finally代码块中始终会被执行
}
```