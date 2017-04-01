##  MVC 设计原则
### 由于ng-init定义数据模型是直接在view视图(html标签中)定义,所以违反了MVC的 模块之间要低耦合分离的设计原则,因此不推荐使用
### 使用Controller控制器创建Model数据---复合MVC的设计原则
#### 步骤  ngApp--->Module-->Controller-->Modle
(1) 使用ngApp指令声明一个AngularJS的应用
```
   <tag ng-app="应用入口的模块名" >
```
(2) 创建一个自定义的模块. 使用 angular.module方法
```
   var myModule1=angular.module('myModule1',['ng']);
```
这个函数的第一个参数是模块名,第2个参数是你的模块要依赖的其他模块的名称列表,所以是个数组,这里需要引入Angular核心模块ng

(3) 注册模块 如果使用代码的话可以用angular.injector和angular.bootstrap.最简便的注册方式使用ngApp指令
```
   <html ng-app="模块名">
```

(4) 在模块中声明Controller函数
```
    myModule1.controller('myctr1',function ($scope) {

    }
```
  第一个参数是控制器的名称,第2个参数是控制器的实现函数,在控制其中由于需要定义数据模型,所以使用依赖注入的方式获得$scope对象.
  尽可能使用链式导航的方式创建模块和控制器

(5) 在Controller控制器中定义Model模型
```
   $scope.a=30
```
   $scope对象充当粘合剂的作用 将View和Model粘结在一起

(6)在View(html标签中)使用数据

   在需要填充数据的标签父节点上使用ng-controller指令创建控制器,指令的值是控制器的名字。然后在需要填充的地方使用{{数据}} 填充数据

##### AngularJS与JQuery的区别和关系
1. JQuery简化JS操作DOM节点的代码
2. AngularJS完全隐藏了DOM操作,使得开发者将注意力集中在业务逻辑和数据的组织上
3. AngularJS中一样也要做DOM操作,只不过做了彻底封装,使得开发人员不需要接触DOM操作
4. AngularJS中定义了两个变量jQuery对象和jqLite对象,如果用户在页面上引入了jQuery的文件,那么ng中的DOM操作就会直接使用这个jQuery来完成。
反之没有jQuery文件那么ng中会使用jqLite对象完成DOM操作,而这个对象是jQuery的简化版,所以可以说ng对jQuery做了彻底的封装。

##### 简述AngularJS的模块化
1. 一个AngularJS应用中肯定有一个也可有多个module模块
2. ngApp的入口模块只能有一个,那么当存在多个模块时,可以通过模块的依赖列表来注册其他模块。
3. 一个模块中可以声明多种组件,比如service服务、derective指令、controller控制器、filter过滤器等
4. 在前端页面比较复杂的情况下,可能会将页面划分成不同的功能区域,那么就可以针对不同的区域设置不同的模块来处理,方便编写SPA单页面应用

##### 简述$scope作用域的特点
1. 在控制器中所使用的$scope(作用域对象)都是属于各自唯一的。
2. 声明在某个$scope中的模型数据,不能被其他的控制器访问
3. 控制器是可以嵌套的,那么如果控制器中有同名的数据模型,那么起作用的是离得最近的控制器中的。

##### $scope与 $rootScope的区别
1. $rootScope是应用程序中的根作用域,其他的scope都派生自rootScope
2. 整个application中只有一个$rootScope对象,所以可以在它之中保存公共数据,任意一个控制器都可以访问
3. $scope是属于某一个特定的控制器的,其中的数据只能由这个控制器访问
