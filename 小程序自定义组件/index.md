# 小程序自定义组件

### 一、自定义组件的步骤

1. 需要在json文件中进行自定义组件声明(将component字段设置为true)可以将这组文件设置为自定义组件
2. 在wxml中编写属于我们组件自己的模板
3. 在wxss中编写相关组件的样式
4. 在js文件中，可以定义数据或组件内部的相关逻辑

注：WXML节点标签名只能是 小写字母、中划线、下划线组合。所以自定义组件标签名只能是这些。不能以”wx-“为前缀。在app.json的usingComponents声明某个组件，就是全局注册，那么所有页面和组件都可以直接使用该组件

### 二、自定义组件的样式细节

1. 自定义组件的样式不会干扰引用组件的page的样式
2. 组件内不能使用id选择器、属性选择器、标签选择器
3. 外部使用class的样式只对外部wxml的class生效，对组件不生效
4. 外部使用了id选择器、属性选择器不会对组件内产生影响
5. 外部使用了标签选择器，会对组件内产生影响
6. 组件内的class和组件外的class是有隔离的，官方不推荐使用属性、id、标签选择器

### 三、向组件传递数据和样式
1.给组件传递数据：
> 在组件的js文件中使用properties属性，有两种定义方式，一种是变量名后面加类型；一种是变量名里面详细参数

![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/blog/2020-02-01-083724.png)
> 在组件中通过`{{title}}`进行接收，在使用组件的地方通过变量名进行传值。

![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/blog/2020-02-01-084734.png)
2.给组件传递样式：
> 在组件的js文件中使用externalClasses属性，在组件内的wxml中使用externalClass属性中的class，在页面中传入对应的class，并且给这个class设置样式

![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/blog/2020-02-01-085359.png)
### 四、组件向外传递事件
> 1.在组件中绑定事件 2.在组件的js中将监听事件发射出去 3.page中接收到事件，执行对应的方法 4.在page的js中定义相关的处理方法

![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/blog/2020-02-01-093857.png)

页面直接调用组件方法(`this.selectComponent`)
![](https://blogimg-1256896917.cos.ap-shanghai.myqcloud.com/blog/2020-02-02-092329.png)

### 五、Component构造器

1. properties：定义传入的属性
    ```JavaScript
        properties:{
          title:String,
          content:{
            type:String,
            value:""
          }
        }
    ```
2. data：定义内部属性
    ```JavaScript
        data:{
          currentIndex:0,
          info:{
            name:"haha",
            age:18
          }
        }
    ```
3. methods：定义方法
    ```JavaScript
        methods:{
          onBtnClick(){
            this.setData({
              "info.name":"kobe"
            })
          }
        }
    ```
4. options：额外配置选项
    ```JavaScript
        options:{
          styleIsolation:"shared",
          multipleSlots:true
        }
    ```
5. externalClasses：引用外部样式
    ```JavaScript
        externnalClasses:['title'],
    ```
6. observers：属性和数据监听
    ```JavaScript
        observers:{
          title:function(newVal){
            console.log(newVal)
          }
        }
    ```
7. pageLifetimes：页面生命周期
    ```JavaScript
        pageLifetimes:{
          show(){
            console.log("页面显示出来")
          }
          ...
        }
    ```
8. lefetimes：组件生命周期
    ```JavaScript
        lifeimes:{
          created(){
            console.log("组件被创建")
          }
          ...
        }
    ```

