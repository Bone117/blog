# JavaScript入门（基础）

## 一、JS语言介绍

### 1.概述

- 浏览器脚本语言
- 可以编写运行在浏览器上的代码程序
- 属于解释性、弱语言类型编程语言

### 2.组成

- ES语法：ECMAScript、主要版本有ES5和ES6
- DOM:文档对象模型(Document Object Model),是W3C组织推荐的处理可扩展标准语言的标准编程接口。
- BOM浏览器对象模型(Browser Object Model),提供了独立于内容的、可以与浏览器窗口进行互动的对象结构，且由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象。

## 二、JS的基本语法

### 1.三种存在位置(HTML中)

#### 1.1行间式：存在于行间的时间中

```html
<body id='bd' onload="body.style.backgroundColor='cyan'">
    
</body>
```

#### 1.2内联式：存在于页面的script标签中

```html
<body id ='bd'>
    <style>
        bd.style.backgroundColor='cyan';
    </style>
</body>
```

- 一般情况下，内联可以放在`</body>下 或者</body>上`，最终被解析在`</body>`上方
- 内联式也可以出现在`<head>`标签底部，一般应用于依赖性JS库

#### 1.3外联式：存在于外部JS文件，通过script标签的src属性链接

```javascript
html文件：
<script src='js/test.js'></script>

js文件：
bd.style.backgroundColor='cyan';
```

```javascript
<script src="js/02.js">
	alert(123)
</script>
拥有src的外联script标签，会自动屏蔽标签内部的代码块
```

#### 1.4总结

- 内联使用场景：某页面的特有逻辑，可以书写在该页面的script标签内
- 外联的使用场景：适用于团队开发，js代码具有复用性
- 出现在head标签底部：依赖型JS库；出现在body标签底部：功能型JS脚本

### 2.JavaScript注释

```javascript
//单行注释

/*
多行注释
*/
```

### 3.变量的定义

#### 3.1变量的命名规范

- 标识符由字母数字下划线，$组成，不能以数字开头(驼峰命名法)
- 标识符不能与关键字及保留字重名
- 区分大小写

**保留字**：

|          |           |            |           |              |
| -------- | --------- | ---------- | --------- | ------------ |
| abstract | arguments | boolean    | break     | byte         |
| case     | catch     | char       | class*    | const        |
| continue | debugger  | default    | delete    | do           |
| double   | else      | enum*      | eval      | export*      |
| extends* | false     | final      | finally   | float        |
| for      | function  | goto       | if        | implements   |
| import*  | in        | instanceof | int       | interface    |
| let      | long      | native     | new       | null         |
| package  | private   | protected  | public    | return       |
| short    | static    | super*     | switch    | synchronized |
| this     | throw     | throws     | transient | true         |
| try      | typeof    | var        | void      | volatile     |
| while    | with      | yield      |           |              |

#### 3.2ES5定义变量

```javascript
var num = 10; //无块级作用域变量
num = 10; //全局变量
```

#### 3.3ES6定义变量

```javascript
let num = 10; //局部变量
const NUM=10; //常量  常量名全大写 不允许修改
```

**注**：ES5标准下无块级作用域，只有方法可以产生实际的局部作用域

```javascript
// 方法的自调用，就会产生一个局部的作用域
(function () {
    var x=10;
    y=20;
})()
```

ES6有块级作用域

### 4.三个基本弹出框

```javascript
alert() //一个弹出框只能弹出一条消息，多个值，后面的值会被忽略
confirm() // 确认框   有返回值， true | false
prompt() //输入框  确定为输入值，取消为null
```

### 5.函数监测

```
typeof(100)
```

### 5.数据类型

#### 5.1值类型

##### 5.1.1数字类型Number

数值范围：`-5e324 ~ 1.7976931348623157e+308`超过范围，会显示为 Infinity(正无穷) 或 -Infinity(负无穷),不像其它语言，JS的数字类型包含了小数。

**特殊的Number值 NaN**:表示Not A Number，非数字类型

和任何值都不相等

与任何值运算,结果还是NaN

```javas
isFinite()    //函数判断是否在范围内
isNaN()    //函数 判读是否是 NaN
```

##### 5.1.2字符串类型String

**声明方式**：

- 双引号

- 单引号

- 模板字符串(ES6新增)

  ```js
  content = `
  打扎好，我寺${username}.
  是兄弟，就来砍我
  今晚八点，不见不散
  `
  //多行，${}方式 嵌入变量。 传统方式变量字符串连接必须用字符串连接符
  ```

##### 5.1.3布尔类型Boolean  true |false

```js
var a = true;
console.log(a, typeof a);
```

##### 5.1.4未定义类型Undefined

```js
undefined 表示'缺少值'
var c = undefined
console.log(c,typeof c);
```

#### 5.2引用类型

##### 5.2.1函数类型function

```js
var m = function(){
    console.log(m,typeof m);
}
```

##### 5.2.2对象类型：object

```js
var obj = {};
console.log(obj,typeof obj);

obj = new Object();
//console.log(obj,typeof obj); //对象类型判断一般用instanceof
console.log(obj instanceof Object);
```

#### 5.3具体的对象类型

##### 5.3.1空对象：null

```js
var o = null;
//console.log(o,typeof o);
//空的判断用==
console.log(o == null);
```

##### 5.3.2数组对象：Array

```js
o = new Array(1,2,3,4,5);
//console.log(o,typeof o);

//判断
console.log(o instanceof Array);
```

##### 5.3.3时间对象：Date

```js
o = new Date();
console.log(o,instanceof Date);
```

##### 5.3.4正则对象：ReExp

```js
o = new ReExp();
console.log(o, instanceof ReExp);
```

### 6.数据类型转换

#### 6.1显示转换

```js
//1.num,bool => str     //String|toString
var a = 10;
var b=true;

var c =String(a);
c = a.toString();

//2.bool,str => num		//Number |+?
var aa =true;
var bb = '10';

var cc = Number(aa);
cc = +aa;  //'1' number
cc = +bb;  //'10' number

//针对字符串		//parseFloat|parseInt
cc = parseFloat('3.14.15.16');
console.log(cc,typeof cc);  //3.14 number
cc = parseInt('10.35abc');
console.log(cc,typeof cc); //10 number

//字符串以非数字开头，结果为NaN
//1.非数字类型
//2.不与任何数相等，包含自己
//3.通过isNaN()进行判断
var res = parseInt('abc10def');
console.log(res,isNaN(res));
```

#### 6.2隐性转换

```js
5 + null //5
'5' +null //'5null'
'5' +1  //'51'
'5' -1  //4
```

### 7.运算符

#### 7.1算数运算符

- 加法运算符 +
- 减法运算符 -
- 乘法运算符 *
- 除法运算符 /
- 模运算符 %
- 负号运算符 -
- 正号运算符 +
- 递增运算符 ++  
- 递减运算符 --

**注：**++/--在前先自身运算，再做其他运算，++/--在后先做其他运算，再自运算

```js
//a = 5
var res = a++;
console.log('res:'+res+'a:'+a); //5  6

//a =5
var res = ++a;
console.log('res:'+res+'a:'+a); //6 6
```

#### 7.2赋值运算符

前提：x=5，y=5

| 运算符 | 例子 | 等同于 | 运算结果 |
| ------ | ---- | ------ | -------- |
| =      | x=y  |        | 5        |
| +=     | x+=y | x=x+y  | 10       |
| -=     | x-=y | x=x-y  | 0        |
| *=     | x*=y | x=x*y  | 25       |
| /=     | x/=y | x=x/y  | 1        |
| %=     | x%=y | x=x%y  | 0        |

#### 7.3比较运算符

前提：x=5

| 运算符 | 描述       | 比较    | 结果  |
| ------ | ---------- | ------- | ----- |
| ==     | 等于       | x=="5"  | true  |
| ===    | 绝对等于   | x==="5" | false |
| !=     | 不等于     | x!="5"  | fales |
| !==    | 不绝对等于 | x!=="5" | true  |
| >      | 大于       | x>5     | false |
| <      | 小于       | x<5     | false |
| >=     | 大于等于   | x>=5    | true  |
| <=     | 小于等于   | x<=5    | true  |

#### 7.4逻辑运算符

前提：n=5

| 运算符 | 描述 | 例子          | 结果                |
| ------ | ---- | ------------- | ------------------- |
| &&     | 与   | x=n>10&&++n   | x=false,n=5（短路） |
| \|\|   | 或   | x=n<10\|\|n-- | x=true,n=5（短路）  |
| !      | 非   | x=!n          | x=false,x=5         |

- `&&`运算，有假即假，前面有假，后面不会被执行，称为短路
- `||`运算，有真，即真，前面为真，后面不执行 短路

#### 7.5三目运算符(三元运算符)

```js
expression ? sentence1 : sentence2 
当expression的值为真时执行sentence1，否则执行 sentence2

var tq = prompt('天气(晴|雨)')
var res =tq =='晴'?'今天天气挺好':'请假回家';
console.log(res);
```

#### 7.6其他运算符

- 条件运算符 `?:`
- `typeof`运算符 判断操作数类型
- `delete`运算符 删除对象属性或者数组元素
- `void`运算符 忽略操作数的值
- 逗号运算符 `,`
- 字符串连接 `+`

### 补：四种调试方式

- alert()
- console.log()
- document.write()
- 浏览器断点调试
