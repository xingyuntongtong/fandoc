## 什么是路由?
- AngularJS 路由允许我们通过不同的 URL 访问不同的内容。
- 通过 AngularJS 可以实现多视图的单页Web应用（single page web application，SPA）。
- 通常我们的URL形式为 http://runoob.com/first/page 但在单页Web应用中 AngularJS 通过 # + 标记 实现，例如：
```
http://runoob.com/#/first
http://runoob.com/#/second
http://runoob.com/#/third
```
当我们点击以上的任意一个链接时，向服务端请的地址都是一样的 (http://runoob.com/) 因为 # 号之后的内容在向服务端请求时会被浏览器忽略掉。 所以我们就需要在客户端实现 # 号后面内容的功能实现。 AngularJS 路由 就通过 #, + 标记 帮助我们区分不同的逻辑页面并将不同的页面绑定到对应的控制器上。

## 1.ng-route路由
属性单页面应用 angular-route它是angular自带的路由模块
### 1.1 安装路由
```
npm install angular-route
```
### 1.2 依赖模块
- 让当前模块依赖于ngRoute
```
var myMod = angular.module('myMod',['ngRoute']);
```
### 1.3 使用 ngView 指令
- ng-view必须有，所有的页面都是渲染到该标签中的,只有它里面的内容是变化的
- 该 div 内的 HTML 内容会根据路由的变化而变化。
```
<div ng-view></div>
```
### 1.4 配置路由
- when: 就是当配置路由的路径是哪个的时候要做的事情；参数1：path 配置路由的路径 参数2：是一个对象；它可以实现链式操作,可以链接多个when
- AngularJS 模块的 config 函数用于配置路由规则。通过使用 configAPI，我们请求把$routeProvider注入到我们的配置函数并且使用$routeProvider.whenAPI来定义我们的路由规则。
- $routeProvider 为我们提供了 when(path,object) 和 otherwise(object) 函数按顺序定义所有路由
```
myMod.config(function($routeProvider){
        $routeProvider.when('/',{
            template:'<h1>index</h1>',//定义需要的模板
        }).when('/users',{
            templateUrl:'usesr.html',
            controller:['$scope','add',function($scope,add){
                //控制器:它控制的是同一个路由下的模板，这里有控制器了在模板里就不用写了
                //$scope.users=['tongtong','haha','hehe'];
                //这里可以是一个字符串也可以是一个对象
                $scope.users=[{id:1,name:'tongtong'},{id:2,name:'haha'},{id:3,name:'hehe'}];
                $scope.result=add(1,2);
            }],
            resolve:{//它是一个对象 相当于factory 可以定义服务,这里面的add就相当于一个服务
                add:function(){//指定当前controller所依赖的其他模块。
                    return function(a,b){
                        return a+b;
                    }
                }
            }
        }).when('/user/:id',{
              // /user/1   /user/2    /user/3
              template:'我的ID号是{{id}}',
              controller: function ($scope, $routeParams) {
            //$routeParams 路由参数（是一个对象） 能够获取到冒号后面的值
                  $scope.id = $routeParams.id
              }
          }).otherwise({
              redirectTo:'/';//意思是重新定义路径，当输入错误时直接回到 / 它的页面上 两种写法
          })
    });
```
### 1.5 创建路由链接
```
<a href="#/home">首页</a>
<a href="#/user/{{user.id}}">用户管理 或者{{user.name}}</a>
<a href="#/contact">联系</a>
<a href="#/settings">设置</a>
<div ng-view></div>
```
实现点击可以跳转相应的页面，它刷新的只是ng-view这一块，其它的不改变 必须有# 防止页面的跳转

## 2.angular-ui-route  可以实现多应用它更强大
### 2.1 安装路由
```
npm install angular-ui-router
```
### 2.2 依赖模块
- 让当前模块依赖于uiRouter
```
var myMod = angular.module('myMod', ['ui.router']);
```
### 2.3 使用 uiView 指令
- ui-view必须有，所有的页面都是渲染到该标签中的,只有它里面的内容是变化的
- 该 div 内的 HTML 内容会根据路由的变化而变化。
```
<div ui-view></div>
```
### 2.4 配置路由
- state 和 when 差不多
```
myMod.config(function($stateProvider,$urlRouterProvider){
    $urlRouterProvider.otherwise('/');
    //可以在这个服务下调用otherwise,不然没有这个属性，可以实现刚打开页面就显示内容
    $stateProvider.state('index',{
        //参数1：是状态的名字  参数2： 配置的对象
        url:'/',//代表路由 必须写,每个里面都得写不然在搜索栏里不显示
        templateUrl:'index.html'
    }).state('about',{
        url:'/about',//一个状态配置一个路径，路径不要重复
        templateUrl:'about.html',
        controller:function($scope){
            $scope.books = [{id: 1, name: 'js高程3'},{id: 2, name: 'node'},{id: 3, name: 'html5'}];
        }
    }).state('contact',{
        url:'/contact',
        views:{//配置多视图，一个页面里面有多个路由
            '':{
              //默认状态下渲染该模板
               templateUrl:'templs/contact.html'
            } ,
            'tel@contact':{
               //tel@contact
               //contact状态对应的templs/contact.html页面中的 ui-view = ‘tel’视图 ，渲染的是templateUrl:'templs/tel.html'
                templateUrl:'templs/tel.html',
                controller: function ($scope) {
                    $scope.tel='12345'
                }
            },
            'address@contact':{
                templateUrl:'templs/address.html'
            }
        }
    }).state('book',{
          url:'/book/:name',
          template:'{{a}}',
          controller: function ($scope,$stateParams) {
              //$stateParams对象 获取冒号后面的值
              $scope.a=$stateParams.name
          }
      })
}).controller('myCtrl',function($scope){
    $scope.flag='index';
})
```
### 2.5 创建路由链接
-   ```
    <a ui-sref="home">首页</a>
    <a ui-sref="book({name:book.name})">用户 或者{{book.name}} </a>
    <a ui-sref="contact">联系</a>
    <div ui-view></div>
    ```








