# flex布局笔记

## 一、简介：
> flex布局又叫弹性布局，只要将最外层的块级元素display设置为flex，就创建了flex布局。可以把他看成一个盒子，盒子里面有很多子项。设置了flex布局的最外层元素被称为flex container，作为容器存在，里面的子项被称为flex items。container和item分别有不同的属性可以设置。

#### 几个基础概念：
![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/blog/2022-02-22-006tNbRwly1gbfpf6jbg6j310q0fqtcw.jpeg)
（图片来自网络）

* 主轴：main axis（从main-start到main-end）子元素默认排布
* 交叉轴：cross axis(从cross-start到cross-end)
* main size：单个项目占据主轴长度大小
* cross size：子元素在交叉轴方向上代销

## 二、container的属性
#### 2.1 flex-direction：绝对主轴方向，控制子项的整体布局方向

* row：主轴从左到右
* row-reverse：主轴从右到左
* column：主轴从上到下
* column-reverse：主轴从下到上

#### 2.2 justify-content：主轴的对齐方式，控制子项的对齐方式

* flex-start:与main start对齐(默认值)
* flex-end：与main end对齐
* center：居中对齐
* space-between：item之间距离相等，main start 和main end 两端对齐(两端靠边)
* space-evenly：item之间和两端距离相等
* space-around：item之间距离相等，main start 和main end之间的距离是 items之间距离的一半。

#### 2.3 align-items：item在cross axis上的对齐方式

* normal：与stretch一致，item在cross axis方向的size为auto时，会自动拉伸至填充container(意思就是item没有设置高度，会自动填充至container高度)
* flex-start:与cross start对齐(顶部对齐)
* flex-end：与cross end对齐(底部对齐)
* center：居中对齐
* baseline：与基准线对齐

#### 2.4 flex-wrap:控制子项整体单行显示还是换行显示

* nowarp：默认nowarp，一行放不下时会压缩。
* warp：宽度不足换行显示
* wrap-reverse：宽度不足换行显示，但是从下往上开始

#### 2.5 align-content：align-content可以看成和justify-content是相似且对立的属性，justify-content指明水平方向flex子项的对齐和分布方式，而align-content则是指明垂直方向每一行flex元素的对齐和分布方式。如果所有flex子项只有一行，则align-content属性是没有任何效果的。

* stretch：默认值，与align-items的stretch类似
* flex-start：与cross start 对齐，顶部堆砌
* flex-end：与cross end 对齐
* center：居中对齐
* space-between：顶底两端对齐，items之间距离相等
* space-around：顶底两端是items距离之间的一半，items之间的距离相等
* space-evenly：全部等距。

## 三、flex-items属性
#### 3.1 order：
> 可以设置任意整数,值越小越排在前面，默认值是0

#### 3.2 align-self:
> 可以覆盖flex container设置的align-items。参数与align-items一致

#### 3.3 flex-grow：
> 决定flex items 如何扩展。当flex container 在main axis还有剩余size时，可用。可以设置任意非负数字，默认为0。

* 所有flex-grow总和超过1，每个item扩展的size为 `剩余size*flex-grow/sum`
* 所有flex-grow总和不超过1，每个item扩展的size为 `剩余size*flex-grow

注：flex items扩展后端饿size不能超过width\height

#### 3.4 flex-shrinnk：
> 不支持负值，默认值是1，也就是默认所有的flex子项都会收缩。如果设置为0，则表示不收缩，保持原始的fit-content宽度。

* 如果只有一个flex子项设置了flex-shrink：

> flex-shrink值小于1，则收缩的尺寸不完全，会有一部分内容溢出flex容器。
> flex-shrink值大于等于1，则收缩完全，正好填满flex容器。

* 如果多个flex子项设置了flex-shrink：
> flex-shrink值的总和小于1，则收缩的尺寸不完全，每个元素收缩尺寸占“完全收缩的尺寸”的比例就是设置的flex-shrink的值。
> flex-shrink值的总和大于1，则收缩完全，每个元素收缩尺寸的比例和flex-shrink值的比例一样。

#### 3.5 flex-basis：
> 用来设置item的宽度。

决定items宽度的优先级(从高到低)：
> max-width\min-width > flex-basis > width >内容的size

参考：https://www.zhangxinxu.com/wordpress/2018/10/display-flex-css3-css/

