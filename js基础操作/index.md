# JS基础操作

## 一、分支结构

#### 1、if语句

- if 基础语法

```js
if (条件表达式) {
    代码块;
}
// 当条件表达式结果为true，会执行代码块；反之不执行
// 条件表达式可以为普通表达式
// 0、undefined、null、""、NaN为假，其他均为真
```

- if 复杂语法

```js
// 1.双分支
if (表达式1) {
    代码块1;
} else {
    代码块2;
}

// 2.多分支
if (表达式1) {
    
} else if (表达式2) {
    
} 
...
else if (表达式2) {
    
} else {
    
}
```

- if 嵌套

```js
if (表达式1) {
    if (表达式2) {
        
    }...
}...
```

#### 2、switch语句

```js
switch (表达式) {
    case 值1: 代码块1; break;
    case 值2: 代码块2; break;
    default: 代码块3;
}
// 1.表达式可以为 整数表达式 或 字符串表达式
// 2.值可以为 整数 或 字符串
// 3.break可以省略
// 4.default为默认代码块，需要出现在所有case最下方，在所有case均未被匹配时执行
```

## 二、循环结构

#### 1、for循环

```js
for (循环变量①; 条件表达式②; 循环变量增量③) {
    代码块④;
}
// 1.循环变量可以在外、在内声明
// 2.执行逻辑 ① ②④③ ... ②④③ ②，入口为①，出口为②，②④③个数为[0, n]
```

#### 2、while循环

```js
while (条件表达式) {
    代码块;
}
```

#### 3、do...while循环

```js
do {
    代码块;
} while (条件表达式);
```

#### 4、for...in循环

```js
obj = {"name": "zero", "age": 8}
for (k in obj) {
    console.log(k, obj[k])
}
// 用于遍历对象：遍历的结果为key，通过[]语法访问对应的value
```

#### 5、for...of循环

```js
iter = ['a', 'b', 'c'];
for (i of iter) {
    console.log(iter[i])
}
// 1.用于遍历可迭代对象：遍历结果为index，通过[]语法访问对应的value
// 2.ES6新增，可迭代对象有 字符串、数组、Map、Set、Anguments、NodeList等
```

#### 6、break，continue关键词

- break：结束本层循环
- continue：结束本次循环进入下一次循环

## 三、异常处理

```js
try {
    易错代码块;
} catch (err) {
    异常处理代码块;
} finally {
    必须逻辑代码块;
}
// 1.err为存储错误信息的变量
// 2.finally分支在异常出现与否都会被执行
```

```js
throw "自定义异常"
// 必要的时候抛出自定义异常，要结合对应的try...catch使用
```

## 四、函数初级

#### 1、函数的定义

- ES5

```js
function 函数名 (参数列表) {
    函数体;
}

var 函数名 = function (参数列表) {
    函数体;
}
```

- ES6

```js
let 函数名 = (参数列表) => {
    函数体;
}
```

- 匿名函数

```js
(function (参数列表) {
    函数体;
})

// 匿名函数需要自调用
(function (参数列表) {
    函数体;
})(参数列表);
```

#### 2、函数的调用

- 函数名(参数列表)

#### 3、函数的参数

- 个数不需要统一

```js
function fn (a, b, c) {
    console.log(a, b, c);  // 100 undefined undefined
}
fn(100);

function fn (a) {
    console.log(a)  // 100
}
fn(100, 200, 300)  // 200,300被丢弃
```

- 可以任意位置具有默认值

```js
function fn (a, b=20, c, d=40) {
    console.log(a, b, c, d);  // 100 200 300 40
}
fn(100, 200, 300);
```

- 通过...语法接收多个值

```js
function fn (a, ...b) {
    console.log(a, b);  // 100 [200 300]
}
fn(100, 200, 300);
// ...变量必须出现在参数列表最后
```

#### 4、返回值

```js
function fn () {
    return 返回值;
}
// 1.可以空return操作，用来结束函数
// 2.返回值可以为任意js类型数据
// 3.函数最多只能拥有一个返回值，返回多个的时候，不报错，只返回最后一个值
```

## 五、事件初级

- onload：页面加载完毕事件，只附属于window对象
- onclick：鼠标点击时间
- onmouseover：鼠标悬浮事件
- onmouseout：鼠标移开事件
- onfocus：表单元素获取焦点
- onblur：表单元素失去焦点

## 六、JS选择器

#### 1、getElement系列

```js
// 1.通过id名获取唯一满足条件的页面元素
document.getElementById('id名');
// 该方法只能由document调用

// 2、通过class名获取所有满足条件的页面元素
document.getElementsByClassName('class名');
// 该方法可以由document及任意页面元素对象调用
// 返回值为HTMLCollection (一个类数组结果的对象，使用方式同数组)
// 没有匹配到任何结果返回空HTMLCollection对象 ([])

// 3.通过tag名获取所有满足条件的页面元素
document.getElementsByTagName('tag名');
// 该方法可以由document及任意页面元素对象调用
// 返回值为HTMLCollection (一个类数组结果的对象，使用方式同数组)
// 没有匹配到任何结果返回空HTMLCollection对象 ([])
```

#### 2、querySelect系列

```js
// 1.获取第一个匹配到的页面元素
document.querySelector('css语法选择器');
// 该方法可以由document及任意页面对象调用

// 2.获取所有匹配到的页面元素
document.querySelectorAll('css语法选择器');
// 该方法可以由document及任意页面对象调用
// 返回值为NodeList (一个类数组结果的对象，使用方式同数组)
// 没有匹配到任何结果返回空NodeList对象 ([])
```

#### 3、id名

- 可以通过id名直接获取对应的页面元素对象，但是不建议使用

## 七、JS操作页面内容

- innerText：普通标签内容（自身文本与所有子标签文本）
- innerHTML：包含标签在内的内容（自身文本及子标签的所有）
- value：表单标签的内容
- outerHTML：包含自身标签在内的内容（自身标签及往下的所有）

## 八、JS操作页面样式

- 读写 style属性 样式

```js
div.style.backgroundColor = 'red';
// 1.操作的为行间式
// 2.可读可写
// 3.具体属性名采用小驼峰命名法
```

- 只读 计算后 样式

```js
// eg: 背景颜色
// 推荐
getComputedStyle(页面元素对象, 伪类).getPropertyValue('background-color');
// 不推荐
getComputedStyle(页面元素对象, 伪类).backgroundColor;

// IE9以下
页面元素对象.currentStyle.getAttribute('background-color');
页面元素对象.currentStyle.backgroundColor;

// 1.页面元素对象由JS选择器获取
// 2.伪类没有的情况下用null填充
// 3.计算后样式为只读
// 4.该方式依旧可以获取行间式样式 (获取逻辑最后的样式)
```

- 结合 css 操作样式

```js
页面元素对象.className = "";  // 清除类名
页面元素对象.className = "类名";  // 设置类名
页面元素对象.className += " 类名";  // 添加类名 前面要注意加空格

classList[add|remove|toggle] //add添加  remove 删除 toggle有则无，无则有
eg: this.classlist.toggle('active') //如果class里没有active就加上，有就删掉
```
