javaScript 从一个仅仅在浏览器上面的一个玩具语言，一转眼演变成无所不能神一般的存在。但是，由于天生存在着一点戏剧性，模块系统作为一门语言最基本的属性却是javaScript所缺的。
## Node.js里的模块和包的概念
- 在前端的编程里，我们经常把单独功能的一些JS脚本写一个文件里，用这个文件的时候就用script标签的src属性来引用。在node.js里，同样也是把单独功能的脚本写一个单独的文件里，不过引入的方式变了，是用require来引用。这个单独的JS文件，在node.js里叫模块
- 什么是模块呢？我们可以简单的把它看成一个实现单独功能的文件。比如在前端JS中的jQuery.js，在node.js里，我们把它就看成一个模块的概念就行。在前端的JS中使用它之前，先要引入此文件。在node.js里，则需要require来引入.
- 包的概念：包含一个或多模块的(也就是JS文件的)文件夹（我们先简单的理为包就是一个实现完整功能的目录，其实它的意义远不止这些，先这样上手）
- 包是模块化JS必须的。
  我们可以在node.js项目中直使用别人写好的功能，就是使用别人写的包。也可以自己写一个包，发布到互联网上供大家使用。 
- 为了让Node.js的文件可以相互调用，Node.js提供了一个简单的模块系统。
  模块是Node.js 应用程序的基本组成部分，文件和模块是一一对应的。换言之，一个 Node.js 文件就是一个模块，这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展。
##  1.JS的不足
js没有模块系统（现在es6中有），不支持封闭作用域或依赖管理
没有`标准库`，没有文件系统API
没有包管理系统，不能自动加载和安装依赖
## 2.什么是CommonJS?
- CommonJS 是javascript模块化编程的一种规范，主要是在服务器端模块化的规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，在该模块内部定义的变量，无法被其他模块读取，除非定义为global对象的属性。
- commonjs规范
一个 node.js由大量模块组成，每个JS文件都是一个模块
实现了require方法，npm基于commonjs实现了自动加载和安装依赖
> require 它不是global上的属性，它是形参

## 3.模块化优点
- 增加内聚性，有助分工协作（高内聚，低耦合），你干你的我干我的不影响
- 方便重构(你走了在来个人接着干) 方便管理代码
- 提高代码质量
## 4.模块使用
- 定义(创建)模块 我们创建一个文件就是声明了一个模块，它会在外层自动的增加一个闭包函数
- 使用模块 require('./filename'); 可以引用一个模块/包
- 导出模块 module.exports和exports 模块的接口
- 实现原理 文件外面默认被套了一个闭包函数,所以可以模块化,我们是看不到的,是文件形参传入进来的,虽然不是global上的属性，但是可以直接使用
```
(function(exports,require,module,__filename,__dirname){
    exports  = module.exports = {};
    1.exports.Person=Person; //这样{}就不是空对象了相当于给module.exports也加了
    2.module.exports = Person; //这种方法也行2种方法 但是exports=Person不行
    return module.exports; //但是导出的是module.exports
})();
```
module.exports和exports的区别

- 导出模块导出的不是exports对象  exports默认是一个空对象，得exports.a=a;
- 真正require进来的是module.exports对象,向外暴露一个接口
- 一个global全局上的变量 一个是形参,它的实现原理就是这样
- 使用module.exports = Person;的时候在另一个模块中`var person=require('./Person')`; 在使用exports.Person=Person;的时候在另一个模块中`var person=require('./Person').Person`;
- 当导出一个时用module.exports = Person; 导出多个时用exports.Person=Person;
## 5.require（引入）的应用
- 需要一个模块就要把这个模块require(引)进来，通过相对路径引用模块
- require()是同步的：有返回值的是同步方法 如果带有回调函数的是异步
- 多次导入一个模块会对模块进行缓存,多次加载后得到同一对象,防止浪费性能 所以只会出现一次
```
require('./test.js');
```
- 查看模块缓存: require.cache 根据模块的绝对路径进行缓存,{'一个绝对路径':'模块'}
```
console.log(require.cache);  //相当于对象
```
- 查询(解析)模块绝对路径: 要让模块多次加载，可以加载后删除掉缓存的模块，得解析出一个绝对路径，例如解析一个绝对路径  C:\Users\10354_000\Desktop\node8\3.module\test.js
```
require.resolve('./test.js');   //相当于key
```
> 可以帮我们通过一个已经存在的文件解析出一个绝对路径出来

- 查看单个的模块缓存
```
require.cache[require.resolve('./test.js')]; //{key}： 对象[key]就取出key了
```
- 删除模块缓存,这样多次加载就可以输出多次
```
delete require.cache[require.resolve('./test.js')]; //就是delete 对象[key]
```
## 6.模块分类
- 1.内置模块(核心模块)http url fs util  是node中自带的
```
require('util');
util.inherits继承 原理就是Object.create方法
inspect解析属性
isArray isRegExp isDate isError判断数据类型
```
- inherits 默认为原型链继承(不继承私有属性)
```
ctor.prototype.__proto__ = superCtor.prototype;
ctor.prototype = Object.create(superCtor.prototype);
```
- inspect 解析对象
```
var obj  = {name:'tong',age:7};
util.inspect(obj,{showHidden:true,depth:1,colors:true});
showHidden 显示隐藏属性
depth 解析的深度
colors 输出的时候带颜色
```
- 给对象增加属性,这里拿util举例
```
Object.defineProperty(obj,'age',{
    value:'8', //当前定义属性的值
    enumerable:false,//是否可枚举
    writable:true,//是否可写
    configurable:false,//是否可配置
});
```
- 判断类型
```
console.log(util.isArray([]));
console.log(util.isError(new RegExp()));
console.log(util.isRegExp(new Error()));
```
- 2.文件模块(自定义模块) 以./方式引入的都是文件模块
```
require('./文件名字');
```
- 3.第三方模块,是需要安装的
```
require('mime');//会自动搜索package.json里的main属性 mime.js
```
> 我们引入第三模块是不要路径的./,直接引用模块的名字
自己写的模块 要注意:当我们安装一个模块会就近安装 找最近目录一直往上找，直到找到node_modules文件夹，如果找不到在自己的目录下创建

## 7.模块查找规则
当没有以`/`或者`./`来指向一个文件时，这个模块要么是核心模块要么就是从node_modules文件夹加载的第三方模块

- 从module.paths(模块的查找路径)取出第一个目录开始。
- 直接从目录中查找，存在结束，不存在下一条。先查找模块的名字,找不到往上找，找到跟目录
- 查找index.js index.json如果没有index 会找package.json里的main字段
- 我们的文件模块不用指定后缀名默认会自动添加.js后缀，如果找不到在添加.json后缀
- 尝试将require的参数作为一个包来查找，读取package.json，取得main配置项指定的文件查找，不存在进行3
- 继续失败查看下一个目录










