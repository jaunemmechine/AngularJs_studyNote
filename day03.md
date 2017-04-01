## 双向数据绑定复习
### 双向数据绑定的原理
  1. 事件驱动的脏数据检查机制
  2. 当发生ng认为的会改变模型数据的事件时,就会触发ng的脏数据检查机制
  3. 我们所定义的每一个控制器都拥有一个监控范围(控制器所控制的页面区域内的所有表达式及绑定指令),还会给每一个监控的对象生成一个监控处理的回调函数,整个过程涉及到两个函数$apply及$digest。其中$digest这个函数专门负责检查所有的受监控对象。当有某个事件发生,而ng认为这个事件有可能会修改模型数据时,$digest函数就会自动执行,它会将所有的受控对象全部遍历一遍,数据发生变动的就会执行处理的回调函数。$apply函数会调用$digest发起脏数据检查
  4. 由于会对所有的受控对象做脏数据检查,当受控对象很多时,会影响页面的渲染速度,这也是AngularJS的一个缺点。解决方式:不要整个页面都只用一个模块一个控制器控制,而是划分多个模块多个控制器,使得一个控制器的控制范围尽可能合理,这样就可以提高效率。


## 依赖注入DI  Dependency Inject

### 什么叫做依赖注入
   什么是依赖Dependency 某个函数调用的时候需要其他的对象传入,那么就说这个函数依赖于其他对象

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


