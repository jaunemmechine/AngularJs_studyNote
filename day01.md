# AngularJS简介
## 技术栈简介
 1. html+CSS  编写前端HTML页面
 2. JavaScript脚本程序  运行在客户端浏览器中
 3. NodeJS(也是使用JS代码)服务端 js代码是运行在服务器端
 4. DOM树 -- JQuery框架  + BootStrap框架页面的美化 响应式 兼容性
 5. Html5+CSS3.0 移动端(微信+APP)
 6. 前端的框架技术 -- 脚手架

    a.AngularJS  google公司  1.X  2.0

    b.Vue.js(尤雨溪) 文档齐全(阿里Weex)

    c.ReactJS(ReactNactive)推特

## AngularJS的特点
### (1)MVC框架
M-->Model模型

V-->View 视图

C-->Controller控制器

### (2)MVVM架构的前端框架 降低了各个模块之间的耦合程度 高内聚低耦合
VM-->View Model 视图模型($scope)
### (3)实现了数据的双向绑定
### (4)依赖注入
### (5)一般使用AngularJS快速构建CRUD(Create Read Update Delete)操作的SPA(Single Page Appliction)应用
### (6)在编写js代码时不再将精力放在DOM对象的操作上,而是聚焦在数据的组织和绑定,以及控制器代码的业务逻辑编写上

## AngularJS如何实现数据的双向绑定
  AngularJS内部使用事件驱动的脏数据检查(dirty-check)机制实现双向的数据绑定
## AngularJS的使用
#### 引入AngularJS的js文件
#### 在需要AngularJS起作用的节点中使用ng-app指令 声明范围
1. 目的是为了防止与其他框架在同一个页面共存时产生冲突
2. 一般我们也不会在使用ng后在页面里混用其他框架,所以通常
    将ng-app定义在html标签上,使得ng在整个页面起作用
      
## AngluarJS中的常用指令
1. ng-app 声明AngularJS在页面中起作用的范围
2. ng-model 定义一个数据model 它的值就是你给这个数据模型起的名字
   如果你给标签定义了数据模型,那么标签原本的value属性就会失效
3. ng-bind  数据绑定指令 将某一个数据模型绑定到所在标签
   它的值是数据模型的名字、会替换标签内的原有内容,所以可以实现预加载图标显示
4. ng-init 初始化数据模型的值,使用时应该放在父元素节点上
   ng-init如果重复定义以最后一次为准
5. ng-if 条件判断标签,复合条件所在标签会加入到文档树,否则不加入,就不会显示出来

##### 注意:angularJS中不能混杂普通的js代码,反之亦然

## AngularJS的模板
 用法 {{表达式}}
 
## AngularJS中的事件
1. ng-click事件
2. ng-mousedown事件等等

## AngularJS中的数组操作
1. 数据模型的值可以是数组,或者对象数组
2. 循环显示数据用ng-repeat指令
    
 