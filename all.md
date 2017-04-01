# AngularJs 学习笔记
## 什么是Angular
#### jQuery，它属于一种类库(一系列函数的集合)，以DOM为驱动核心；
#### 而Angular是一种 MVC 的前端框架，则是前端框架，以数据和逻辑为驱动核心，
#### 它有着诸多特性，最重要的是：模块化，双向数据绑定，语义化标签，依赖注入等。
#### MVC 开发模式：M->model(模型) V->view(视图) C->controller(控制器)
## 模块化
#### 最大程度实现代码的复用
##### 定义应用：
为html标签添加ng-app属性，表明整个文档都是应用，也可以在指定标签添加，该标签包裹的所有内容都是应用的一部分
##### 定义模块：
提供一个全局对象Angular，用angular.moduel(参数1，参数2)方法定义模块，输出一个返回值。
```
var myModule1=angular.module('myModule1',['ng']);
```
第一个参数是模块名,也就是跟ng-app的值一致；

第二个参数是该模块依赖其他模块的名称列表,是个数组,这里需要引入Angular核心模块ng,依赖不存在则写[]
##### 定义控制器
定义模块输出的返回值

```
    //myModel是一个模块实例化对象
    //通过这个实例化对象定义控制器
    //需要两个参数，第一个参数表示控制器名称
    //第二个参数表示控制器的实现函数
    var myMode1=angular.module('myModel',['ng']);
    myMode1.controller('myctr1',function ($scope) {

    });
  ```
## 指令
#### 常用内置指令
1. ng-app 指定应用根源素，至少有一个元数指定了此属性
2. ng-controller 指定控制器
3. ng-show 控制器元素是否显示，true表示显示，false表示不显示（通过display：none/block来控制）
4. ng-hide 控制元素是否隐藏，true表示隐藏，false表示不隐藏
5. ng-if 控制元素是否"存在",true表示存在，false表示不存在（与ng-show区别：当为false的时候，连同整个DOM节点都不存在）
6. ng-src 增强图片路径
7. ng-href 增强地址
8. ng-class 控制类名（后面可接对象,如ng-class="{done: true}"）
9. ng-include 引入模块
10. ng-disabled 表单禁用
11. ng-readonly 表示只读
12. ng-checked 单选或复选表单选中
13. ng-selected 下拉框表单选中

#### 自定义指令
directive
```
//自定义指令
    var app=angular.module('myapp',[]);
    app.directive('tag',function () {
        return{
            //自定义指令类的型 E、A、C、M
            restrict:'EA',//是否替换原有标签
            replace:true,//指令模板
            template:'<h1>hello angluarjs</h1>',//指令外部模板
            //templateUrl:'header.html'
        }
    })
```
## 数据绑定
#### 单向绑定
只能是模型数据向视图传递（类似artTemplate模板引擎的工作方式）相关指令：

1. 通过{{}}或者ng-bind来实现模型数据向视图模板的绑定，模型数据是由一个内置服务$scope提供，它是个空对象，通过为这个对象添加属性或方法，便可以在相应的视图模板里被被访问。
2. 这里的{{}}是ng-bind的简写形式，区别在于通过{{}}绑定数据时会出现闪屏，添加ng-cloak 也可以解决“闪烁”现象。

#### 双向绑定

1. 方向一： 模型--->View视图 一旦View绑定了某个数据模型,那么模型数据的改变会自动更新View页面中的内容
   能做方向一绑定的指令 ng-bind ng-if ng-show ng-hide ng-repeat ng-src
2. 方向二： View视图--->模型数据 一旦绑定,用户在页面中修改了数据,那么会直接影响到控制器中的模型数据
（1）能够使用双向数据绑定的指令只有ng-model一个
（2）可以使用$watch函数观察指定的数据是否改变,第一个参数是被观察的数据名称,第2个参数是数据改变后要执行的回调函数。

#### 双向数据绑定的原理
1. 事件驱动的脏数据检查(dirty-check)机制
2. 当发生ng认为的会改变模型数据的事件时,就会触发ng的脏数据检查机制
3. 我们所定义的每一个控制器都拥有一个监控范围(控制器所控制的页面区域内的所有表达式及绑定指令),
还会给每一个监控的对象生成一个监控处理的回调函数,整个过程涉及到两个函数$apply及$digest。
其中$digest这个函数专门负责检查所有的受监控对象。当有某个事件发生,而ng认为这个事件有可能会修改模型数据时,
$digest函数就会自动执行,它会将所有的受控对象全部遍历一遍,数据发生变动的就会执行处理的回调函数。
$apply函数会调用$digest发起脏数据检查。
4. 由于会对所有的受控对象做脏数据检查,当受控对象很多时,会影响页面的渲染速度,这也是AngularJS的一个缺点。
解决方式:不要整个页面都只用一个模块一个控制器控制,而是划分多个模块多个控制器,使得一个控制器的控制范围尽可能合理,
这样就可以提高效率。

## 作用域

每个controller下的$scope产生不同的作用域
根作用域：$rootScope

### $scope作用域的特点：
1. 在控制器中所使用的$scope(作用域对象)都是属于各自唯一的。
2. 声明在某个$scope中的模型数据,不能被其他的控制器访问
3. 控制器是可以嵌套的,那么如果控制器中有同名的数据模型,那么起作用的是离得最近的控制器中的。

### $scope与 $rootScope的区别

1. $rootScope是应用程序中的根作用域,其他的scope都派生自rootScope
2. 整个application中只有一个$rootScope对象,所以可以在它之中保存公共数据,任意一个控制器都可以访问
3. $scope是属于某一个特定的控制器的,其中的数据只能由这个控制器访问

## 过滤器
### 内置过滤器
1. currency [货币] 将数值格式化为货币格式
2. date 日期格式化，年（y）、月（M）、日（d）、星期（EEEE/EEE）、时（H/h）、分（m）、秒（s）、毫秒（.sss），也可以组合到一起使用。
3. filter 在给定数组中选择满足条件的一个子集，并返回一个新数组，其条件可以是一个字符串、对象、函数
4. json 将Javascrip对象转成JSON字符串。
5. limitTo 取出字符串或数组的前（正数）几位或后（负数）几位
6. lowercase 将文本转换成小写格式
7. uppercase 将文本转换成大写格式
8. number 数字格式化，可控制小位位数
10. orderBy 对数组进行排序，第2个参数是布尔值可控制方向(正序或倒序)

### 自定义过滤器
通过模块对象实例提供的filter方法自定义过滤器

#### 总结
竖线调用，冒号传值。

## 依赖注入
### 什么叫做依赖注入
   1. 什么是依赖Dependency 某个函数调用的时候需要其他的对象传入,那么就说这个函数依赖于其他对象
### 如何解决依赖关系
   1. 主动创建
```
        var stu = new Object();
        stu.name="tom";
        stu.score1=75;
        stu.score2=80;
        stu.score3=90;
        function totalScore(stu)
        {
          var sum = stu.score1+stu.score2+stu.score3;
          return sum;
        }
        totalScore(stu);
```
   2. 被动注入(inject)方式

     依赖对象不需要你主动创建,而是由别人自动帮你创建,并传入到函数中,这种方式就是依赖注入

   3. AngularJS中,很多情况下都可以使用依赖注入。典型的如控制器创建时,需要用到$scope对象,这个对象不需要我们创建,而是由ng创建,并注入到控制器中。
   4. ng中,使用依赖注入的方式传入ng的需要对象时,要保证对象的名字一定是ng所规定的名字,否则会错误
   5. 可以被注入的对象---所有的service/provider对象都是可被注入的
   
     a.$rootScope $scope  数据模型

     b.$interval $timeout 定时器

     c.$log    日志服务

     d.$http   http的异步请求服务(AJAX)

     ```
      //使用$http服务
         App.controller('myconCtrl',['$scope','$http',function($scope,$http){
             //发起异步请求
             $http({
                 method:'post',//请求方式
                 url:"./example.php",//请求地址
                 data:{name:'yangyang',age:18},//请求主体
                 headers:{//请求头消息
                     'content-Type':'application/x-www-form-urlencoded'
                 }
             }).success(function(data,status,headers,config){
                 //成功回调
             }).error(function (data,status,headers,config) {
                 //失败回调
             });
         }]);
       ```

     e.$location 位置服务

     f.$animate  动画

   6. 若项目中使用了类似yuicompressor.jar这样的js压缩程序,那么会把函数的参数名精简成一个字母的形式,这样会导致ng的依赖注入失败。解决方式如下:
   ```
    angular.module("mymd2",['ng'])
       .controller('myctrl3',['$scope','$interval',function (a12,asdbc) {
           a12.num=2001;
           asdbc(function(){
               a12.num++;
           },1000);
       }])
   ```

   定义控制器时,第2个参数是个N+1个单元的数组,数组的前N个单元就是需要注入的字符串格式的对象名,最后一个单元是控制器的实现函数,其参数列表就是需要注入的N个对象,要注意这个n个参数的顺序要和前N个单元的顺序一样

## 模块加载

### 配置块

通过config来实现对服务的配置（也可以更改一些服务的默认设置）,AngularJS绝大多数服务都有对应的provider。

例如：$route 对应的$routeProvider(配置路由)

### 运行块

特殊：run方法 还是最先执行的。

案例：比如验证用户是否登录，未登录则不允许进行任何其它操作

## 路由

### 功能：一个应用由若干个视图组成，然后,根据不同的业务逻辑展示不同的视图给用户。

### 理解

#### SPA：Single Page Application 单页面应用

#### 链接使用锚点

#### 单一页面原理：单页面通过hashchange事件监听锚点的变化，实现不同锚点准备找到对应的视图

#### 路由: AngularJS基于单一页面原理进行封装，将锚点变化封装成路由，这也是与后端路由的根本区别

#### 路由的使用： 需引入angular-route.js文件

#### 实例化模块，传入依赖（路由名称为：ngRoute）

#### 配置路由（config、$routeProvider、when（条件））

```
App.config(['$routeProvider',function($routeProvider){
        //配置路由
        $routeProvider.when('/',{
            template:'首页'
        });
    }]);
```

#### 布局模板（用ng-view指令，路由匹配的视图会渲染到该区域）

```
<header>头部</header>
<div class="container">
    <!--视图会被加载并渲染到此处-->
    <div ng-view></div>
</div>
<footer>底部</footer>
```

#### 路由的参数

#####  两种方法匹配路由:when和otherwise，when可以调用多次。otherwise作为when的补充，参数只有一个。

#####  when有两个参数

1. 参数1：是个字符串，代表当前url的hash值；例如："/：type"
2. 参数2：是一个对象，配置当前路由参数，如视图、控制器

#### template：字符串形式视图模板

#### templateUrl：引入外部视图模板

#### controller：视图模板所属控制器，作用之一：通过http请求向后台要数据

#### redirectTo：跳转到其他路由 例如：“/2”；

#### 获取路由参数，在控制器中注入$routeParams，可以传递参数给后台或其他。

```
//url 地址
// http://localhost/AngularJs/myng.html#/index/10
//得到的结果为{id:10}
.when('/index/:id',{//路由规则
    template:'index page',
    controller:'indexController'
})
```
