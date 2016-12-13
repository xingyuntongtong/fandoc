## 配置环境变量
`我的电脑->属性->高级系统设置->环境变量->找到path`

- 配置ws提示，setting->npm->引用源码包
## 什么是node
- node是一个环境，供js代码执行的环境，我们可以把它等价于浏览器，只不过我们一般都会把node这个环境安装到服务器端
- 它实现了诸如文件系统、模块、包、操作系统API、网络通信等核心JS没有或不完善的功能
- 它摒弃了传统平台依赖多线程来实现高并发的设计思路，而采用单线程、异步式I/O(读写文件)、事件驱动式的程序设计模型
- 它使用了来自于Google的V8引擎
> 验证时一定要前后台都验证
能使用异步的不用同步，防止阻塞主线程
安装的时候要默认c盘不然会更改环境变量会有问题
向一个文件中写入内容，我们需要把文件读出来，在写入到这个文件中;
IO: 输入输出 Input Output

## node.js的优点
- node.js基于JS语言
- 统一公共类库，代码标准化（因为代码都能看懂）
- Node本身运行V8 JavaScript。V8 JavaScript引擎是Google用于其Chrome浏览器的底层JavaScript引擎。
- http://cnodejs.org/ node中文社区，它的社区非常活跃
## node的线程
- node的主线程永远是单线程的
- 单进程里面可以开多个线程
- node里面就是单进程、单线程，可以开多个进程就是多进程、单线程
## 单线程和多线程
- 多线程：同一个时间可以干多件事，其实不可能同时干多件事，只是切换上下文(速度非常快)，让你感觉不出来
- 单线程：同一个时间干一件事，一件事干完了才能干别处一件事
> 不管是单核还是多核，都可以开多个进程

## 阻塞和非阻塞
- 针对内核来说的，非阻塞是异步的前置条件
## node的全局对象 global
- 写代码时，可以在任意一个地方访问
- 在node中最大的就是global 在webStorm中运行的是文件中的代码
```
不能通过global访问var声明的变量
var a=100;
console.log(global.a);//找不到得把var去掉，也可以写成global.a=100;
```
## global上的属性
- 1.console.log(this); //在文件中直访问this不是global，但是在命令行中就是global
  2.如果文件外面被套了一个函数，我们看不到 可以模块化 闭包
```
(function () {
   console.log(this); //这里的this就是global
})();
```
-  console输出的是不一定谁先谁后的
    - console.log('log'); //日志
    - console.warn('warn'); //警告
    - console.error('error'); //错误
    - console.info('info'); //信息
    - console.dir('dir');
```
   console.time('start');  //时间  time和timeEnd里面参数名字要一样
   setTimeout(function () {
       console.timeEnd('start'); //这是异步的要写在函数中
       console.log(this);  //这里的this不是global,指向的是自己
   },2000);
```
- __filename 文件的绝对路径 他是不可更改的
      比如  F:\xuexi\3.node.js\node\node_8\2.node\2.global.js
- __dirname  当前文件夹的名字  F:\xuexi\3.node.js\node\node_8\2.node。无法更改
> 全局对象，在任何地方都能访问到，但是它们不是global上的属性
文件相当于一个模块在执行时默认会添加一个自执行函数__filename和__dirname是形参可以在文件内直接访问

- setTimeout 延迟 I/O是最慢的
- setImmediate 立即  两个方法都是异步的 第二个本上setImmediate是个setTimeout一些机会的 它可以让0的setTimeout先执行 它没有时间
- process.nextTick 下一个队列，它也没有时间，它永远比上面2个快，就是同步方法执行后就执行它，也就是在服务员上的第一个小本，上面2个都在第二个小本上
- process(进程) 是global上的属性 进程在整个代码运行后会退出进程
    - process.exit(); 退出进程
    - console.log(__dirname); //不可更改的 它和cwd是一样的
    - console.log('cwd',process.cwd()); //可更改的 也可以进行手动修改
    cwd： current  working directory  当前工作目录，得看它在哪工作的
    pwd:  print working directory 打印当前的工作目录 它是一个Linux命令
    - process.chdir(); 改变目录
    - process.memoryUsage(); //内存的使用 可以监控内存的变化
    rss 常住内存 heapTotal堆内存的总和  heapUsed内存的使用
    - process.pid 当前进程id 运行的性能可在任务管理器上查看
    - process.kill(11104);  //删除进程，可以让进程挂掉比如把开着的浏览器关掉



















