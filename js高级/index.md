# JS高级

## 一、函数高级

### 1.函数回调

函数回调的本质：在一个函数中，满足特定条件下，调用另一个函数

```
//<span style="color: #000000;"> 回调的函数
function callback(data) {}
</span>//<span style="color: #000000;"> 逻辑函数
function func(callback) {
    </span>//<span style="color: #000000;"> 函数回调
    </span><span style="color: #0000ff;">if</span><span style="color: #000000;"> (callback) callback(data);
}</span>
```

![](http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
<span style="color: #000000;">function a_fn(data) {
        console.log(</span><span style="color: #800000;">'</span><span style="color: #800000;">a_fn</span><span style="color: #800000;">'</span><span style="color: #000000;">);
        </span>//<span style="color: #000000;"> 如果要使用数据,那么定义形参接收参数
        console.log(data);
    }

    function b_fn(a_fn) {
        var data </span>= <span style="color: #800000;">"</span><span style="color: #800000;">b_fn 的 数据</span><span style="color: #800000;">"</span><span style="color: #000000;">;
        console.log(</span><span style="color: #800000;">'</span><span style="color: #800000;">b_fn</span><span style="color: #800000;">'</span><span style="color: #000000;">);
        </span>//<span style="color: #000000;"> 条件: a_fn有值(有函数实现体)
        </span><span style="color: #0000ff;">if</span><span style="color: #000000;"> (a_fn) {
            </span>//<span style="color: #000000;"> 调用参数函数
            a_fn(data);
        }
    }
    b_fn(a_fn);</span>
```

test

总结：
　　 1.一个函数(函数名)作为另外一个函数的参数  
　　 2.满足一定的条件,调用参数函数  
　　 3.形成了函数回调,回调的函数可以获取调用函数的局部变量(将数据携带出去)

### 2.闭包

闭包本质：函数的嵌套定义，内层函数称之为闭包
闭包目的：不允许提升变量作用域时，该函数的局部变量需要被其他函数使用  
闭包的解决案例：①影响局部变量的生命周期，持久化局部变量；②解决变量污染

```
<span style="color: #000000;">function outer() {
    var data </span>=<span style="color: #000000;"> {}
    </span>//<span style="color: #000000;">闭包
    function inner() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> data;
    }
    </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> inner;
}</span>
```

注：外部使用局部变量的方法: 返回值 | 函数回调 | 闭包 | 提升作用域

#### 2.1局部变量持久化

```
<span style="color: #000000;"> function outer() {
        </span>//<span style="color: #000000;">eg：请求得到的数据，如果不持久化，方法执行完毕后，数据就会被销毁
        var data</span>=[1,2,3,4,5<span style="color: #000000;">];
        console.log(data);
        </span>//<span style="color: #000000;">通过闭包解决该类问题，所以闭包所有代码均可以随意自定义
        function inner(){
            </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> data;
        }
        </span>//<span style="color: #000000;">数据被inner操作返回，inner属于outer，所以需要将inner返回出去(跟外界建立联系)
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> inner;
    }
    </span>//<span style="color: #000000;">将局部变量生命周期提升于inner函数相同，inner存在，局部变量data就一直存在
    var inner </span>= outer();
```

#### 2.2闭包解决变量污染

现有一个列表，点击某个时，弹出下标

```
<script type=<span style="color: #800000;">"</span><span style="color: #800000;">text/javascript</span><span style="color: #800000;">"</span>>
    // 需求:点击li,打印li在ul中的索引 => 0, 1, 2, 3, 4
    // 1<span style="color: #000000;">.lis 
    var lis </span>= document.querySelectorAll(<span style="color: #800000;">'</span><span style="color: #800000;">ul li</span><span style="color: #800000;">'</span><span style="color: #000000;">);
    </span>// 2<span style="color: #000000;">.循环绑定
    </span><span style="color: #0000ff;">for</span> (var i = 0; i < lis.length; i++<span style="color: #000000;">) {
        </span>// 解决的原理:一共产生了5个外层函数,存储的形参i的值分别是0, 1, 2, 3, 4
        //<span style="color: #000000;"> 内层函数也产生了5个,且和外层函数一一对应,打印的i就是外层函数的形参i

        </span>//<span style="color: #000000;"> 外层函数
        (function (i) {
            </span>//<span style="color: #000000;"> 内层函数:闭包
            lis[i].onclick </span>=<span style="color: #000000;"> function () {
                alert(i)
            }
        })(i)    
    }
    console.log(i);  </span>//<span style="color: #000000;"> 点击事件触发一定晚于该逻辑语句
    </span>//<span style="color: #000000;"> 所以再去触发点击,弹出i的值,永远是5

    </span>//<span style="color: #000000;"> 该类问题就称之为变量污染
</span></script>
```

ES6的块级作用域的语法也可以解决循环绑定变量污染的问题

```
let lis = document.querySelectorAll(<span style="color: #800000;">'</span><span style="color: #800000;">li</span><span style="color: #800000;">'</span><span style="color: #000000;">);
    </span><span style="color: #0000ff;">for</span> (let i = 0; i < lis.length; i++<span style="color: #000000;">) {
        lis[i].onclick </span>=<span style="color: #000000;"> function () {
            alert(i)
        }
    }</span>
```

## 二、JS的面向对象

### 1.面向对象初始

```
var obj = {}; | var obj =<span style="color: #000000;"> new Object();
</span>//<span style="color: #000000;"> 属性
obj.prop </span>= <span style="color: #800000;">""</span>;|obj[<span style="color: #800000;">'</span><span style="color: #800000;">name</span><span style="color: #800000;">'</span>]=<span style="color: #800000;">""</span><span style="color: #000000;">;
</span>//<span style="color: #000000;"> 方法
obj.func </span>=<span style="color: #000000;"> function () {}
</span>//<span style="color: #000000;"> 删除属性与方法
delete obj.prop
delete obj.func</span>
```

### 2.类字典结构使用

js中没有字典(键值对存储数据的方式),但可以通过对象实现字典方式存储数据,并使用数据

```
var dict = {name: <span style="color: #800000;">"</span><span style="color: #800000;">zero</span><span style="color: #800000;">"</span>, age: 18<span style="color: #000000;">}
var dict </span>= {<span style="color: #800000;">"</span><span style="color: #800000;">my-name</span><span style="color: #800000;">"</span>: <span style="color: #800000;">"</span><span style="color: #800000;">zero</span><span style="color: #800000;">"</span><span style="color: #000000;">, fn: function () {}, fun () {}}
</span>//<span style="color: #000000;">使用
dict.name </span>| dict[<span style="color: #800000;">"</span><span style="color: #800000;">my-name</span><span style="color: #800000;">"</span>] | dict.fn()
```

```
总结：<br/>1.key全为字符串形式，但存在两种书写方式<br/>2.key在js语法标识符支持情况下，可以省略引号，但key为js非法标识符的情况下，必须添加引号。<br/>3.value可以为任意类型<br/>4.访问值可以通过字典名(对象名).key语法与[“key”]语法来访问value<br/>5.可以随意添加key与value完成增删改查
```

```
//<span style="color: #000000;"> 增
dict.key4 </span>=<span style="color: #000000;"> true;
console.log(dict);

</span>//<span style="color: #000000;"> 删
delete dict.key4;
console.log(dict);

</span>//<span style="color: #000000;"> 改
dict[</span><span style="color: #800000;">"</span><span style="color: #800000;">my-key3</span><span style="color: #800000;">"</span>] = [5, 4, 3, 2, 1<span style="color: #000000;">];
console.log(dict);

</span>//<span style="color: #000000;"> 查
console.log(dict.key1);
console.log(dict[</span><span style="color: #800000;">"</span><span style="color: #800000;">key1</span><span style="color: #800000;">"</span>]);
```

### 3.构造函数

构造函数其实就是普通函数  
特定的语法与规定:  
1.一般采用首字母大写(大驼峰)  
2.内部可以构建属性与方法,通过this关键词

```
//<span style="color: #000000;"> 普通函数
function fn() {
    var num </span>= 10<span style="color: #000000;">;
    console.log(</span><span style="color: #800000;">'</span><span style="color: #800000;">普通函数</span><span style="color: #800000;">'</span><span style="color: #000000;">);
}
fn();

</span>//<span style="color: #000000;">构造函数
function People(name) {
    this.name </span>=<span style="color: #000000;"> name;
    this.eat </span>=<span style="color: #000000;"> function (agr) {
        console.log(this.name </span>+ <span style="color: #800000;">"</span><span style="color: #800000;">吃</span><span style="color: #800000;">"</span> +<span style="color: #000000;"> agr);
    }
}</span>
```

构造函数可以创建多个对象,使用{}构建出的对象是唯一的

```
<span style="color: #0000ff;">var</span> p1 = <span style="color: #0000ff;">new</span> People("zero");  <span style="color: #008000;">//</span><span style="color: #008000;"> new关键词, 创建对象并实例化</span>
<span style="color: #0000ff;">var</span> p2 = <span style="color: #0000ff;">new</span> People("seven"<span style="color: #000000;">);

</span><span style="color: #0000ff;">var</span> obj = {} <span style="color: #008000;">//</span><span style="color: #008000;">对象唯一</span>
```

构造函数的属性变量与{}的语法规则不一样，构造函数内部通过this，二普通对象直接通过对象名.的方式

```
<span style="color: #0000ff;">function</span><span style="color: #000000;"> People(name) {
    </span><span style="color: #0000ff;">this</span>.name =<span style="color: #000000;"> name;
    </span><span style="color: #0000ff;">this</span>.eat = <span style="color: #0000ff;">function</span><span style="color: #000000;"> (agr) {
        console.log(</span><span style="color: #0000ff;">this</span>.name + "吃" +<span style="color: #000000;"> agr);
    }
}

</span><span style="color: #008000;">//</span><span style="color: #008000;">普通对象</span>
<span style="color: #0000ff;">var</span> obj =<span style="color: #000000;"> {
    index: </span>"obj对象"<span style="color: #000000;">,
    fn: </span><span style="color: #0000ff;">function</span><span style="color: #000000;"> () {
        console.log(</span>"obj方法"<span style="color: #000000;">);
    }
};
</span><span style="color: #008000;">//</span><span style="color: #008000;"> 使用属性与方法</span>
<span style="color: #000000;">console.log(obj.index);
obj.fn();</span>
```

### 4.继承

 1.完成继承,必须拥有两个构造函数  
2.一个函数要使用另外一个函数的属性与方法,需要对其进行继承(属性与方法的复用)

#### 4.1 ES5的继承

属性的继承用call方法,在继承函数中由被继承函数调用,传入继承函数的this与被继承函数创建属性对属性进行赋值的所有需要的数据  
方法的继承prototype原型

```
    <span style="color: #008000;">//</span><span style="color: #008000;"> 类似于父级</span>
    <span style="color: #0000ff;">function</span><span style="color: #000000;"> Sup(name) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 属性存放某个值</span>
        <span style="color: #0000ff;">this</span>.name =<span style="color: #000000;"> name;
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 方法完成某项功能</span>
        <span style="color: #0000ff;">this</span>.func = <span style="color: #0000ff;">function</span><span style="color: #000000;"> () {
            console.log(</span>"func"<span style="color: #000000;">);
        }
    }
    </span><span style="color: #0000ff;">var</span> sup = <span style="color: #0000ff;">new</span> Sup("supClass"<span style="color: #000000;">);
    console.log(sup.name);
    sup.func();

    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 类似于子级</span>
    <span style="color: #0000ff;">function</span><span style="color: #000000;"> Sub(name) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 属性的继承</span>
        Sup.call(<span style="color: #0000ff;">this</span><span style="color: #000000;">, name);//Sup有name属性,那么可以通过call将Sub的name传给Sup
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 方法的继承</span>
    Sub.prototype = <span style="color: #0000ff;">new</span><span style="color: #000000;"> Sup; //将new Sup赋值给Sub.prototype

    </span><span style="color: #0000ff;">var</span> sub = <span style="color: #0000ff;">new</span> Sub("subClass"<span style="color: #000000;">);
    console.log(sub.name);
    sub.func();

    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 原型</span>
    console.log(sup.__proto__);
```

#### 4.2 ES6继承

```
<span style="color: #008000;">//</span><span style="color: #008000;"> 多对象 => 创建类</span>
<span style="color: #000000;">    class Person {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 构造器:创建对象完成初始化操作</span>
<span style="color: #000000;">        constructor (name, age) {
            </span><span style="color: #0000ff;">this</span>.name =<span style="color: #000000;"> name;
            </span><span style="color: #0000ff;">this</span>.age =<span style="color: #000000;"> age;
        }
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 实例方法:只能由对象调用</span>
<span style="color: #000000;">        eat () {
            console.log(</span><span style="color: #0000ff;">this</span>.name + '吃吃吃'<span style="color: #000000;">);
        }
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 类方法:只能由类调用</span>
<span style="color: #000000;">        static create () {
            console.log(</span>'诞生'<span style="color: #000000;">);
        }
    }
    let p1 </span>= <span style="color: #0000ff;">new</span> Person('zero', 8<span style="color: #000000;">);
    let p2 </span>= <span style="color: #0000ff;">new</span> Person('seven', 58<span style="color: #000000;">);

    console.log(p1.age);
    p2.eat();
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> Person.eat();</span>
<span style="color: #000000;">    Person.create();
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> p2.create();</span>

    <span style="color: #008000;">//</span><span style="color: #008000;"> 继承</span>
<span style="color: #000000;">    class Student extends Person {
        constructor (name, age, sex) {
            </span><span style="color: #008000;">//</span><span style="color: #008000;"> super指向父级</span>
<span style="color: #000000;">            super(name, age);
            </span><span style="color: #0000ff;">this</span>.sex =<span style="color: #000000;"> sex;
        }
    }

    let s1 </span>= <span style="color: #0000ff;">new</span> Student("张三", 18, "男"<span style="color: #000000;">);
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 属性的继承</span>
<span style="color: #000000;">    console.log(s1.name, s1.age, s1.sex);
    console.log();
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 方法的继承</span>
<span style="color: #000000;">    s1.eat();
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 继承为全继承</span>
    Student.create();
```

补：ES6用箭头语法表示函数

```
<span style="color: #008000;">//</span><span style="color: #008000;"> 函数</span>
let f = () =><span style="color: #000000;"> {
}</span>
```

#### 5.属性解决循环绑定变量污染

还是用之前的变量污染的例子，js代码看下面

```
    <span style="color: #0000ff;">var</span> lis = document.querySelectorAll('li'<span style="color: #000000;">);

    </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">var</span> i = 0; i < lis.length; i++<span style="color: #000000;">) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> lis[i]依次代表五个li页面元素对象</span>
        <span style="color: #008000;">//</span><span style="color: #008000;"> 给每一个li设置一个(临时)属性</span>
        lis[i].index = i;  <span style="color: #008000;">//</span><span style="color: #008000;"> 第一个li的index属性存储的就是索引0,以此类推,最后一个存储的是索引4</span>
<span style="color: #000000;">
        lis[i].onclick </span>= <span style="color: #0000ff;">function</span><span style="color: #000000;"> () {
            </span><span style="color: #008000;">//</span><span style="color: #008000;"> var temp = lis[i].index; // lis[i]中的i一样存在变量污染</span>
            <span style="color: #0000ff;">var</span> temp = <span style="color: #0000ff;">this</span>.index;  <span style="color: #008000;">//</span><span style="color: #008000;"> 当前被点击的li</span>
            alert(temp)  <span style="color: #008000;">//</span><span style="color: #008000;"> temp => this.index(lis[i].index) => i(索引)</span>
<span style="color: #000000;">        }
    }</span>
```

## 三、定时器

应用场景：  
1.完成js自启(不需要手动触发)动画  
2.css完成不了的动画

+   
setInterval　　一次性定时器

+   
setTimeout　　持续性定时器

```
<span style="color: #008000;">//</span><span style="color: #008000;"> 一次性定时器</span>
    <span style="color: #008000;">/*</span><span style="color: #008000;"> 异步执行
        setTimeout(函数, 毫秒数, 函数所需参数(可以省略));
    </span><span style="color: #008000;">*/</span><span style="color: #000000;">
    console.log(</span>"开始"<span style="color: #000000;">);
    setTimeout(</span><span style="color: #0000ff;">function</span><span style="color: #000000;"> (data) {
        console.log(data);
    }, </span>1000, "数据"<span style="color: #000000;">);
    console.log(</span>"结束"<span style="color: #000000;">);

    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 持续性定时器</span>
    <span style="color: #008000;">/*</span><span style="color: #008000;">异步执行
        setInterval(函数, 毫秒数, 函数所需参数(可以省略));
    </span><span style="color: #008000;">*/</span><span style="color: #000000;">
    console.log(</span>"又开始"<span style="color: #000000;">);
    </span><span style="color: #0000ff;">var</span> timer = setInterval(<span style="color: #0000ff;">function</span><span style="color: #000000;">() {
        console.log(</span>"呵呵"<span style="color: #000000;">);
    }, </span>1000<span style="color: #000000;">)
    console.log(timer);
    console.log(</span>"又结束");
```

#### 清除定时器

1.持续性与一次性定时器通常都应该有清除定时器操作  
2.清除定时器操作可以混用 clearTimeout() | clearInterval()  
3.定时器的返回值就是定时器编号,就是普普通通的数字类型

```
    document.onclick = <span style="color: #0000ff;">function</span><span style="color: #000000;"> () {
        console.log(</span>"定时器清除了"<span style="color: #000000;">);
        clearInterval(timer);
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> clearTimeout(timer);</span>
<span style="color: #000000;">    }

    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 清除所有定时器</span>
    <span style="color: #008000;">//</span><span style="color: #008000;"> 如果上方创建过n个定时器,那么timeout值为n+1</span>
    <span style="color: #0000ff;">var</span> timeout = setTimeout(<span style="color: #0000ff;">function</span>(){}, 1<span style="color: #000000;">);
    </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">var</span> i = 0; i < timeout; i++<span style="color: #000000;">) {
        </span><span style="color: #008000;">//</span><span style="color: #008000;"> 循环将编号[0, n]所有定时器都清除一次</span>
<span style="color: #000000;">        clearTimeout(i)
    }
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 1.允许重复清除</span>
    <span style="color: #008000;">//</span><span style="color: #008000;"> 2.允许清除不存在的定时器编号</span>
```

 
