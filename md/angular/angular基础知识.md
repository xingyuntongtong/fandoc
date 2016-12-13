## AngularJS是什么？
- 它是完全使用js编写的客户端框架 它不兼容IE8以下
- AngularJS  诞生于2009年，由Misko Hevery 等人创建，后为Google所收购。是一款优秀的前端JS框架，已经被用于Google的多款产品当中。AngularJS有着诸多特性
## 框架和库的区别
- 库：提供了很多方法，让我们来调用(主动调用)，比如：vue,react
- 框架：我们按照他的规范去写，人家调用我们(被动)(强约束)
> 框架的好处，提供完整的解决方案

## MVC
- Model: 数据模型层 存在数据的
- View: 视图层,负责展示 大部分的业务逻辑放在了这里，很厚重
- Controller: 控制器，控制逻辑和业务逻辑展示层(主要实现了路由的功能路由就相当于路径 接口等)很单薄,在backbone中把该层去掉，只是用路由替代了
## MVVM
- M: model: 用来存放数据的(数据模型层)
- V: view: 视图展示 很单薄(视图层)
- VM: viewModel: 视图模型层 在angular里面可以认为是$scope，大部分又业务逻辑放在了这里 很厚重
> `view` 和 `viewModel`是双向数据绑定  view通过viewModel把数据存放到model里面，MVVM它是经过MVC的历史演变过来的

## 下载angular
- 下载nodejs会提供一个叫npm的东西
- node package manager
```
npm install angular
```
> 通过npm来下载，别人上传上去的'包'，默认安装到当前目录的node_modules文件夹中，如果上级目录有node_modules就会安装到上级中

## 本文讲述Angular中的指令、控制器、服务的应用场景。
- 指令：涉及到DOM操作的时候使用
- 控制器：涉及到业务逻辑的时候使用
- 服务：需要跨域共享数据的时候使用
## 双向数据绑定
- 引入angular后 提供一个angular对象
- 实现最基础的双向数据绑定，视图怎么能影响数据变化
- 只有"表单元素"才能实现双向数据绑定
## {{}}
- {{}}快捷方式 按 ngb+tab键 : 实现了angularJS重要的特性之一，数据绑定,两个花括号{{}}组成，可以把数据绑定到HTML
    - 1.{{}}可以放文字，变量，运算符(加减乘除，三元运算符) 比如{{name}}用来做表达式结果 {{1+2*3}}
        `<input type="text" ng-model="value">`
        {{value?value:'zfpx'}}  // value里面有值就显示里面的值没有就是zfpx 当输入的时候会跟着改变
    - 2.变量可以存储字符串，数组，对象等
## 内制指令
### ng-app  在ng-app下的内容都归angular所管理
- 它是angularJS读取页面的起始位置
- 会形成$rootScope的跟作用域
- 只有最外层的一个ng-app是有效的，也就是说只能有一个跟作用域$rootScope
- 写在哪里就是哪一部分归angularJS管理，一般一个页面都归angularJS管理，写在html标签中，可以放在页面中任何元素标签中，一般写在html标签中
> 让angular用哪个模块来运行，会指定一个模块的名字，会产生一个$rootScope

### ng-init 在当前作用域下定义初始值。
- 它就是初始化对象 和js中的var 代表的是一个意思
- 它就是初始化数据的作用  多组值用分号;隔开 通常情况下,不使用 ng-init。使用一个控制器或模块来代替它。
- 使用场景，只是演示一些小案例，真实项目中是不会使用的
### ng-model
- 使用的场景 input select textarea 和自定义文本框 这些表单标签里
- ng-model定义的属性名 它存储的是这些文本框里的value的值，只能放值
- 定义的属性name存放在当前作用域下的数据模型上
- {{name}} 其实是取出数据模型上的name所对应的值，绑定到页面中
- 它实现了数据的双向绑定
### ng-repeat
- 也叫迭代就是循环遍历 和for in循环很像
- 可以迭代的内容有字符串，数组，对象
    - 对象: key in name 中的key 为value的值 表示的属性值而不是属性名
  (key,value) in name 其中的key,value代表的是一个键值对的属性名和属性值
    - 数组和字符串: (key,value) in name 其中的key,value代表的是 索引和对应的值
  `<li ng-repeat="(key,obj) in objs">{{key}}:{{obj}}</li>`
- 当页面中数据的数组和字符串出现重复项，页面加载的时候会报错，解决方法 track by $index 是按照索引来循环
     - $first: 判断当前项是不是第一项；
     - $last:判断当前项是不是最后一项
     - $odd:判断当前项是不是奇数索引
     - $even:判断当前项是不是偶数索引  前面4个都是布尔类型
     - $index: 就是当前项的索引
> 如果想让它取到上一级的索引就要在上一级{{parent=$index}}在下一级中直接用{{parent}};也可以在上一级的ng-repeat="(key,arr) in arrs",在下一级中直接用{{key}},其中的key就是索引,比如

```
<div ng-init="phones=['苹果','苹果','苹果']">
   <div ng-repeat="phone in phones track by $index" >
       {{phone}}
   </div>
</div>
```
- 它能形成私有的作用域 它形成的每一个DOM元素都有一个自己的作用域
- 我们想让哪一个标签创建出新的DOM元素，我们就把ng-repeat放在哪个标签上
- 能够循环嵌套数组  可以把最外层的索引保存一份让里面的用
### 解决页面刚加载时的闪烁问题: 因为angular.js加载在下面了，它是后加载所以有闪烁 可以放在head里
- 1.ng-bind：绑定 其实{{}}就是ng-bind的缩写，但是ng-bind不会出现闪烁的问题 防止用户看到被渲染之前的样子
    `<div ng-bind='name'>`这里不能加内容`</div>`  正常情况下是 {{name}}
> 属性是不会展示到页面上的，当angular加载后，在将数据插入到div中

- 2.ng-bind-template 它后面可以绑定多个数据  解决 ng-bind 中只能绑定一个的问题
     `<div ng-bind-template='{{name}}{{age}}'></div>`
- 3.ng-cloak 得先给它一个隐藏的样式当angular加载完后在移除;用的是属性选择器[ng-cloak]{display:none}，如果有路由的话可以不用加
     `<div ng-cloak>`{{name}}{{age}}`</div>` 这是项目中最常用的解决方法

## angular有自己的模块化，一切从模块开始
单例模式：名字不能保证重名，调用过长

- 1.angular.module(); 定义一个模块 它放在ng-app='';里面
        参数1：这个模块的名字(字符串类型)
        参数2：是数组可以注入依赖的模块 如果没有可依赖的模块写空数组，必须写，得用它占位 字符串的数组
        返回值: 是module对象
        var myMod=angular.module('myMod1',[]); 在ng-app='myMod1'; 这样写
        angular.module('myMod');//调用这个模块
> myMod代表的是当前的模块

- 2.定义一个控制器 它是分块管理的(相当于每个部门一样) 可以有很多控制器
        myMod.controller('myCtrl',function($scope){ //创建一个控制器
            $scope.name = 'tongtong';
            console.log($scope);//它可以打印出详细的信息，上面有方法原型链 ChildScope
        });
        myMod.run(function ($rootScope) { //将公有的代码放在run中
            $rootScope.name='tongtong';
        });
        使用这个控制器 可管理的范围是ng-controller="myCtrl"的标签里的范围
        在这个控制器里首先形成自己的作用域 定义的变量和方法，都是定义在$scope上的属性 $scope.name='tongtong';
        `<div ng-controller="myCtrl">`{{name}}`</div>`  //使用控制器
> $scope是一个作用域，相当于把属性挂在作用域上，{{ }}可以通过它取值

- 控制器的特点
    - 1.控制器可以嵌套，只能子找父(子控制器可以继承父控制器)，不能父找子
    - 2.控制器会产生一个独立作用域
    - 3.作用范围，与DOM平齐
    - 4.不要复用控制器，而且不要操作DOM
- 控制器的另一种写法，在项目中常用这种写法
  代码压缩的时候会把作为形参的服务压缩成一个字母，angularJS会找不到这个服务，
  解决办法: 以数组的形式进行传参(传递服务)
      前面的字符串和回调函数里的参数是一一对应的表示的是一个意思
      myMod.controller('myCtrl',['$scope','uppercaseFilter',function($scope,uppercaseFilter){
          $scope.name='tong';
          $scope.name=uppercaseFilter($scope.name);
      }]);
> myMod.run(['$rootScope',function($rootScope)]{});也是一样的




