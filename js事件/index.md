# JS事件

## 一、事件的两种绑定方式 *******

#### 1、on事件绑定方式

```js
document.onclick = function() {
    console.log("文档点击");
}
// on事件只能绑定一个方法，重复绑定保留最后一次绑定的方法
```

```js
document.onclick = function() {
    console.log("文档点击");
}
// 事件的移除
document.onclick = null;
```

#### 2、非on事件绑定方式

```js
document.addEventListener('click', function() {
     console.log("点击1");
})
document.addEventListener('click', function() {
     console.log("点击2");
})
// 非on事件可以同时绑定多个方法，被绑定的方法依次被执行
// addEventListener第三个参数(true|false)决定冒泡的方式
```

```js
function fn () {}
document.addEventListener('click', fn);
// 事件的移除
document.removeEventListener('click', fn);
```

## 二、事件参数event *********

- 存放事件信息的回调参数

## 三、事件的冒泡与默认事件 *********

- 事件的冒泡：父子都具有点击事件，不处理的话，点击子级也会出发父级的点击事件

```html
<body id="body">
   	<div id="sup">
        <div id="sub"></div>
    </div>
</body>

<script>
sub.onclick = function (ev) {
    // 方式一
    ev.stopPropagation();
	console.log("sub click");
}
sup.onclick = function (ev) {
    // 方式二
    ev.cancelBubble = true;
	console.log("sup click");
}
body.onclick = function (ev) {
	console.log("body click");
}
</script>
```

- 默认事件：取消默认的事件动作，如鼠标右键菜单
- ev.preventDefault(); | return false;

```html
<body id="body">
   	<div id="sup">
        <div id="sub"></div>
    </div>
</body>

<script>
    //默认事件:鼠标右键oncontextmenu
    sub.oncontextmenu = function (ev) {
        ev.preventDefault();
        console.log("sub menu click");
    }

    //父级取消了默认事件，子级都被取消掉了
    body.oncontextmenu = function(ev){
        console.log("body menu click");
        return false;
    }
</script>
```

## 四、鼠标事件 *********

- 鼠标事件

```html
onclick：鼠标点击
ondblclick：鼠标双击
onmousedown：鼠标按下
onmousemove：鼠标移动
onmouseup：鼠标抬起
onmouseover：鼠标悬浮 onmouseenter
onmouseout：鼠标移开 onmouseleave
oncontextmenu：鼠标右键
```

- 事件参数ev

```html
ev.clientX：点击点X坐标
ev.clientY：点击点Y坐标
```

## 五、键盘事件 *******

- 键盘事件

```html
onkeydown：键盘按下
onkeyup：键盘抬起
// 绑定的对象: 对象自身不录入文本,绑给document,自身录入文本(表单标签),绑给自身
ev.keyCode
```

- 事件参数ev

```html
ev.keyCode：按键编号
ev.altKey：alt特殊按键
ev.ctrlKey：ctrl特殊按键
ev.shiftKey：shift特殊按键
```

- 键盘控制平滑运动

```js
<div class="div"></div>
<script>
    var div =document.querySelector('.div');

    var r_able =false;
    var l_able =false;
    var t_able =false;
    var b_able =false;
    setInterval(function () {
        //l_able为假，则后者短路，可以实现if的简写
        l_able&&(div.style.left=div.offsetLeft -3 + 'px');
        t_able&&(div.style.top=div.offsetTop -3 + 'px');
        
        if(r_able){
            div.style.left = div.offsetLeft + 3 + 'px';
        }
        b_able&&(div.style.top=div.offsetTop +3 + 'px');
    },16);

    document.onkeydown = function (ev) {
        switch(ev.keyCode){
            case 39: r_able=true; break;
            case 37: l_able=true; break;
            case 38: t_able=true; break;
            case 40: b_able=true; break;
        }
    }
    document.onkeyup = function (ev) {
        switch(ev.keyCode){
            case 39: r_able=false; break;
            case 37: l_able=false; break;
            case 38: t_able=false; break;
            case 40: b_able=false; break;
        }
    }
</script>
```



## 六、表单事件 *******

```js
onfocus：获取焦点
onblur：失去焦点
onselect：文本被选中
oninput：值改变
onchange：值改变，且需要在失去焦点后才能触发
onsubmit：表单默认提交事件
```

```html
<form action="">
    <input type="text" name="usr">
    <button type="submit">提交</button>
</form>
<script>
    var form = document.querySelector('form');
    var ipt=document.querySelector('input');
    var btn =document.querySelector('button');

    ipt.onselect = function () {
        console.log("文本被选中了");
    }
    //值改变就触发
    ipt.oninput=function(){
        console.log("值在改变");
    }
    //键盘抬起就触发
    ipt.onkeyup =function () {
        console.log("值在改变");
    }

    //丢失焦点触发
    ipt.change =function () {
        console.log("值在改变");
    }

    //form的专有事件
    form.onsubmit = function () {
        console.log("提交");
        return false;//取消默认事件
    }
</script>
```



## 七、文档事件 *

- 文档事件由window调用

```html
onload：页面加载成功
onbeforeunload：页面退出或刷新警告，需要设置回调函数返回值，返回值随意
```

## 八、图片事件 *

```
onerror：图片加载失败
```

## 九、页面事件 *********

```html
onscroll：页面滚动
onresize：页面尺寸调整
```

```html
window.scrollY：页面下滚距离
```
