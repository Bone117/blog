# 高级布局

## 一、文档流(normal flow)

### 1.概念

```text
将窗体自上而下分成一行一行，块级元素从上至下，行内元素在每行中从左至右顺序排放元素
```

```text
本质为normal flow(普通流，常规流)，文档流就是一个连续具有逻辑上下的页面整体，也可以片面的说，出现在页面中的显示内容都可以理解为在文档流中。
```

### 2.BFC(Block formatting context)

```text
块级格式化上下文，它是一个独立的渲染区域，只有Block-level box参与，它规定了内部的Block-level box如何布局，并且这个区域与外部毫不相干。
```

### 3.BFC规则

- 内部的Box会在垂直方向，一个接一个的放置
- **根据BFC布局方向**，Box自身的水平方向位置，由margin左或右控制，相邻Box的margin会叠加
- Box的垂直位置，由margin-top控制位置，相邻Box会发生重叠(margin塌陷)
- 同一容器(区域)中，box相互影响，不对对区域外产生影响。

## 二、浮动布局

### 1.目的

```text
让block同行显示(1.在父级规定的宽度中，2.不完全脱离文档流)
```

### 2.基本语法

```css
float:left | right
```

- float：left--->设置BFC横向布局规则为从左至右，且block box同行显示之间没有间隔
- float：right--->BFC规则为从右至左，且block box同行显示之间没有间隔

### 3.浮动布局实例

```css
.box{
    width: 1000px;
    margin: 0 auto;
}
div div{
    font: 900 30px/100px 'STSong';
    text-align: center;
    width: 200px;
    height: 100px;
    background-color: green;
    float: left;
    color: white;
}
.b6{
    width: 600px;
    background-color: yellow;
}
.b7{
    width: 400px;
    height: 200px;
    background-color: yellowgreen;
    float: right;
}
.b8{
    width: 300px;
    height: 150px;
    background-color: brown;
    float: left;
}
.b9{
    width: 300px;
    height: 150px;
    background-color: black;
    float: left;
}
.b10{
    width: 400px;
    height: 150px;
    background-color: cyan;
    float: right;
}
.b11{
    width: 600px;

    background-color: red;
    float: left;
}
.b12{
    width: 1000px;
    background-color: blue;
}
```

```html
<div class="box">
    <div class="b1">1</div>
    <div class="b2">2</div>
    <div class="b3">3</div>
    <div class="b4">4</div>
    <div class="b5">5</div>
    <div class="b6">6</div>
    <div class="b7">7</div>
    <div class="b8">8</div>
    <div class="b9">9</div>
    <div class="b10">10</div>
    <div class="b11">11</div>
    <div class="b12">12</div>
</div>
```

效果图：

![](https://img2018.cnblogs.com/blog/1285461/201809/1285461-20180927195411801-38168528.png)


### 4.浮动布局的问题

**通常文档流中**，子标签在父级标签未设置高度的情况下，会撑开父级的高度，父级的高度决定于逻辑最后位置的子级的低端。

**脱离文档流**后的子级标签，不再撑开父级高度。

**不完全脱离文档流**(浮动后的结果)：当目标标签的内部有浮动的子级，目标标签的兄弟标签的布局会出现显示异常！在不做清浮动的情况下，父级不会获取子级的高度。

### 5.清浮动

**清浮动的对象：拥有浮动子级的父级**

这里只介绍四种清浮动的方法

```css
1.浮动的父级设置高度
super {
    height: npx;
}
2.浮动的父级设置overflow
super {
    overflow: hidden;
}
3.浮动的父级兄弟设置clear
brother {
    clear: left | right | both;
}
4.浮动的父级伪类清浮动
super:after {
    content: "";
    display: block;
    clear: left | right | both;
}
```

原理：在浮动布局的情况下，让父级获得合适的高度。

## 三、流式布局

### 1.目的

让布局脱离固定值限制，可以根据页面情况的改变发生相应的变化

### 2.相关设置

```css
百分比设置 %  参考为最近的父级
窗口设置 vw | vh	50vw代表占据窗口宽度50%
字体控制 em rem		em为最近设置字体大小的父级规定的字体大小 rem为html字体大小
```

## 四、定位布局

可以根据需求给相应的块设定相应设置。

### 1.语法

position属性指定一个元素的定位类型

| 值       | 描述           |
| -------- | -------------- |
| relative | 相对定位       |
| absolute | 绝对定位       |
| fixed    | 固定定位       |
| static   | 默认，没有定位 |

设置完定位类型就可以设置top,bottom,left,right四个属性进行布局。，如果同时设置了top和bottom，top生效，同时设置了left和right，left生效。简单说就是 **左右取左，上下取上**

### 2.相对定位

- 未脱离文档流
- 以自身原有位置作为参考坐标系
- 方位布局只改变盒子显示区域，不会改变盒子原有位置(相当于灵魂出窍)
- left=-right，top=-bottom。

**优点：**父级不会脱离文档流，满足所有的盒模型布局

### 3.绝对定位

- 完全脱离文档流，不在文档流中占位，不会撑开父级高度，不会影响兄弟布局，显示层也永远高于文档流
- 以最近定位父级作为参考坐标系，没有就找html

**优点：**如果自身已经采用绝对定位布局，那么子级一定参考自身进行定位。

**注：**如果父级只是辅助子级进行绝对定位,那么一定优选相对定位,因为绝对定位会产生新的BFC,导致盒模型布局会受影响

### 4.固定定位

- 完全脱离文档流
- 参考系为文档窗口，不随页面滚动而改变位置

### 5.z-index

脱离文档流的标签，具有z-index属性，可以用来控制显示层次的优先级，值为任意正整数

## 五、Flex布局

### 1.概念

Flex是Flexible Box的缩写，意为“弹性布局”，用来为盒状模型提供最大灵活性。

- 采用Flex布局的元素，称为Flex容器(flex container)，简称“容器”。它的所有子元素自动成为容器成员，称为flex项目(flex item),简称“项目”。
- 水平为主轴(main axis),主轴开始的位置叫做main start，结束位置叫做main end。
- 垂直为交叉轴(cross axis),交叉轴的开始位置叫做cross start，结束位置叫做cross end
- 项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size

### 2.容器属性

```css
1.flex-direction属性 决定主轴的方向(即项目的排列方向)
flex-direction: row | row-reverse | column | column-reverse;
-- row（默认值）：主轴为水平方向，起点在左端。
-- row-reverse：主轴为水平方向，起点在右端。
-- column：主轴为垂直方向，起点在上沿。
-- column-reverse：主轴为垂直方向，起点在下沿。

2.flex-wrap属性 定义一条轴线排不下的情况下如何换行
flex-wrap: nowrap | wrap | wrap-reverse;
-- nowrap（默认）：不换行。
-- wrap：换行，第一行在上方。
-- wrap-reverse：换行，第一行在下方。

3.flex-flow属性 是flex-direction属性和flex-wrap属性的简写形式，默认为row nowrap
flex-flow: <flex-direction> <flex-wrap>;

4.justify-content属性 定义了项目在主轴上的对齐方式。
justify-content: flex-start | flex-end | center | space-between | space-around;

5.align-items属性 定义项目在交叉轴上如何对齐
align-items: flex-start | flex-end | center | baseline | stretch;

6.align-content属性 定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
```

### 3.项目属性

```css
1.order 属性 定义项目的排列顺序。数值越小，排列越靠前，默认为0。
order: <integer>;

2.flex-grow 属性 定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
flex-grow: <number>; /* default 0 */

3.flex-shrink 属性 定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
flex-shrink: <number>; /* default 1 */

4.flex-basis 属性 定义了在分配多余空间之前，项目占据的主轴空间（main size）。
flex-basis: <length> | auto; /* default auto */

5.flex 属性 是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
flex: <flex-grow> <flex-shrink> <flex-basis>

6.align-self 属性 允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```

## 六、响应式布局

### 1.概念

响应式布局就是一个网站能够兼容多个终端。

**原则：**采用响应式布局的样式块，基本样式块只做共性设置，需要根据页面尺寸进行适应变化的样式均有响应式布局处理

### 2.页面宽度

- 小于\<integer\>宽度

```css
@media only screen and (max-width: <integer>) {
    selector {
        
    }
}
```

- 大于<integer\>宽度小于<integer\>宽度

```css
@media only screen and (min-width: <integer>) and (max-width: <integer>) {
    selector {
        
    }
}
```

- 大于<integer\>宽度

```css
@media only screen and (min-width: <integer>) {
    selector {
        
    }
}
```

### 3.注

- 在响应式布局内，css语法同正常样式表语法一样
- 响应式布局之间存在不同屏幕尺寸的限制，使用样式相互不影响。满足当前屏幕尺寸时，该样式块起作用，不满足时则失效
- 当响应式布局中样式快起作用时，会与正常样式快设置一起协同布局，遵循选择器的优先级规则
