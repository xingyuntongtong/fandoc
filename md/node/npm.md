## 1.包和npm
- 多个模块可以封装成一个包
- 放置多个模块的文件夹称为包,可以通过包来对一组具有相互依赖的有关系模块进行管理。
- npm是node.js默认的模块管理器,用来安装和管理node模块 网址为 http://npmjs.org
- 可以用包的方式通过npm安装、卸载、发布包
## 2.npm: node package manager(管理器)
- npm 是安装node之后给我们提供一个包管理工具
- 我们想把所有依赖都用一个文件来描述出来,初始化一个描述文件,是一个json格式的
- 创建package.json,在一个文件夹里初始化一个项目，这叫记住依赖是必须有的
```
$ npm init -y
{
    "name":"包的名称",
    "description"："包的简要说明",
    "version":"版本号",
    "keywords"："关键字",
    "licenses":"许可证",
    "repositories"："仓库地址 ",
    "dependencies":"包的依赖，一个关联数组，由包名称和版本组成。"
}
```
## 3.安装第三方包
- 全局安装,所有带-g(-global)的都表示全局安装,直接下载到Node的安装目录中，各个项目都可以调用,适合工具模块,可以在`命令行`下直接使用,比如 hexo deploy 
```
npm install 包的名字 -g
```
> 在任何地方安装都可以，因为安装到全局下 比如 npm install nodeppt -g

- 本地安装 将一个模块下载到当前目录的node_modules子目录，然后只有在当前目录和它的子目录之中，才能调用这个模块,本地安装就是项目中要真正使用的，可以在代码中使用
```
npm install 包的名字 --save-dev/--save
```
> 当我们下载下来的项目文件夹中没有安装的工具(node_modules文件)比如jquery angular 直接用`npm install`即可，因为在npm init -y的时候已经有package.json文件了，里面会记住都有哪些工具

- 查看全局路径,目录安装
```
npm root -g
```
### 依赖列表,依赖的好处可以详细描述你的项目
- 所有的依赖要放入到依赖列表中安装时加入到列表里
hexo init blog 初始化的项目是没有node_modules
cd blog&&npm install 是安装所需要的依赖
- 一个叫开发依赖 例如 webpack --save-dev 只是在写代码时用
- 一个叫发布依赖 例如 jquery --save 写代码和上线都要用
## 4.卸载第三方包,怎么安装的就怎么卸载
- 全局卸载
```
npm uninstall 包的名字 -g
```
- 本地卸载
```
npm uninstall 包的名字 --save
npm uninstall 包的名字 --save-dev
```
## 5.发布包,发布全局项目
- 创建用户,在npm官网注册用户 https://www.npmjs.com/
```
npm adduser
```
- 发布项目,我们想发布我们包 和 jquery一样，让别人使用
```
npm publish
```
> 可以让别人进行下载,当做第三方包

注意项目的名称不能是别人已经注册的名称,尽量用自己的名字做前缀 如果注册失败的话可能是因为改了镜像地址了，需要改回来 npm config set registry “http://registry.npmjs.org/“ 查看所有的配置项 npm config ls
## 6.切换源
- 安装此工具
```
npm install nrm -g
```
- 查看所有源
```
nrm ls
```
- 切换到中国源,使用源
```
nrm use cnpm
```

node npm 都可以在命令行下使用,我把它配置到path中了
hexo是否在path中？
C:\Users\10354_000\AppData\Roaming\npm 这里是npm的安装路径我们说npm可以进行全局访问，在npm中创建了很多连接，做了个快捷方式
C:\Users\10354_000\AppData\Roaming\npm\gulp -> C:\Users\10354_000\AppData\Roaming\npm\node_modules\gulp\bin\gulp.js











