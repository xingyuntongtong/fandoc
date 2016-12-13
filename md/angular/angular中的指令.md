### 什么是指令
AngularJS 指令是扩展的 HTML 属性 就是操作DOM元素的
### 什么是自定义指令
就是封装一些方法，供外面使用
## 指令 directive
- 组件式指令：有模板的 `<ul><li></li></ul>`，restrict一般采用E
- 装饰型指令：一般增加属性的 `<div red-color></div>`，restrict一般采用A
> 指令默认是不依赖控制器的；一般我们用指令封装功能而不是封装方法的

## 定义一个指令
```
<div my-dire>wold</div>
var myMod=angular.module('myMod',[]);
myMod.directive('myDire',function(){
        //myDire需要转成驼峰命名法,声明指令后默认返回一个对象
    return{返回值: 是一个对象
        //定义一个模板 它指的是html结构 把它嵌套到html里面 <div my-dire><div>hello</div></div>
        //template:'<div>hello</div>', 会把原有的标签里的内容也替换掉
        templateUrl:'url.html',//引入外界的模板只能是模板片段 上面的就不用了，它是ajax将文件读取过来插入到模板中
        replace:true,  //替换原有的标签 就是把上面的div替换掉 只剩下这一个div
        restrict:'ECMA', //定义指令可以使用的位置 用它的时候必须得有 replace:true
        transclude:true, //保存原有标签里的内容 和ng-transclude(会形成自己的作用域)结合使用，想把内容放在哪个标签上就在哪个标签上加ng-transclude 标签和外面的一样
        scope:true, //指令所有的元素 会形成自己的作用域,它也可以是个对象,{}和true是有区别的，指定它以后就不会出现
        controller:function($scope,$element,$attrs){//可被外界定义指令调用的公用部分
        }//在最前面
        compile:function(tElement,tAttrs){//编译阶段 写在link前面
        },
        link:function(scope,element,attrs){ //链接阶段,是装饰指令
        },
    }
}
});
```
### restrict
```
restrict:'ECMA'; 不写默认值是 EA 可以不写 可以单个写 restrict:'E';
    <my-dire></my-dire><!--用于标签--> E: element 用于标签
    <div class="my-dire"></div><!--用于样式类名--> C: class 用于样式类名
    <!-- directive:my-dire --> 里面内容前后各一个空格  <!--用于注释--> M: comment 用于注释
    <div my-dire></div><!--用于属性名--> A: attribute 用于属性名
```
### link
link: 它是个函数可以实现 作用域(scope)和DOM元素 的链接 在它上面写的都是私有的 链接阶段
```
link:function(scope,element,attrs){
       scope: 指是当前模板的作用域，如果当前模板没有形成作用域，会直接找父级，和控制器里的$scope是一样的，它里面的属性也是定义在scope上的
       element: 自定义指令所在的DOM元素，是一个jQuery对象，可以调用jQuery里的方法 内置的有轻量级的jQuery也就是 jqLite 是简化板的，不是所有的方法都有
  把jQuery 变成原生的 element[0]; 在变回jQuery的  angular.element(element[0])
       attrs: 自定义指令所在的DOM元素上的属性存储在attrs对象里标签里的属性可以通过它得到;也就是当前指令的所有属性变成一个对象格式
   }
```
> 在angularJS只有link可以操作DOM元素 在控制器里用的都是$scope

### scope
- 可以为当前指令设置独立作用域，也可以起到赋值的作用
  return {
    scope:true,
  }
- "@"引用字符串 指令和作用域间的交互  会形成自己的作用域
想用自定义指令拿到控制器里的属性; 在页面上把自定义指令放入到控制器里面，做它的子级
```
  <div ng-controller="myCtrl">
      <input type="text" ng-model="nameCtrl"/>
      <my-dire name="{{nameCtrl}}"></my-dire>
  //<my-dire name="nameCtrl" greet="greetCtrl(d)"></my-dire> 供 = 来使用
  </div>
  var myMod=angular.module('myMod',[]);
  myMod.controller('myCtrl',['$scope',function($scope){
      $scope.nameCtrl='tongtong';
      $scope.greetCtrl=function(a){//这里面的a就是上面函数里传的 d
          alert(a);
      }
  }]);
  myMod.directive('myDire',function(){
      return{
          template:'`<input type="text" ng-model="nameDir"/>`
                    `<button ng-click="greetDir({d:nameDir})">打招呼</button>'`,
          scope:{
              nameDir:'@name',
              //name:'@', 名字相同时直接用@就可以了 name:'@name'
              nameDir:'=name',
              //name:'=', 名字相同时直接用=就可以了 name:'=name'
              greetDir:'&greet'
          }
      }
  });
```
- @name中的name是自定义指令中的name属性，获取的是name="{{nameCtrl}}"里面{{nameCtrl}}的字面量的值 那么nameDir在当前指令所在的作用域可随处调用nameDir存储的{{nameCtrl}}的字面量的值,但是子级改变不了父级的，实现不了双向绑定 会形成自己的作用域
- '=' 它实现了子级和父级的双向绑定 就是和控制器的双向数据绑定 是变量的传递
'=name' 获取的是name='nameCtrl' 中 'nameCtrl' 存储的变量，在当前指令所在的作用域
 修改nameDir也可以同时修改myCtrl中的nameCtrl的值 上面得属性名(变量)不能用字量形式
-  & 用来取控制器中的方法的  方法的传递
 greetDir({d:nameDir})中{d:nameDir}是一个对象nameDir是要传的参数




