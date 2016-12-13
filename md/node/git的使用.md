## 版本控制
### 备份文件
- 类似于网盘备份
- 我们的代码也需要备份。修改完了以后提交给版本库进行保管，哪一天代码没了也可以找回来。
### 记录历史
- 比如我们打游戏就要存档，万一挂了还可以从上个存档的地方重玩。
- 和网盘不同，网盘保留的是最新的状态，历史的记录都没有了，修改的记录也都找不回来了
- 网盘无法知道文件里的某行代码是何人在哪个时间添加进去的
### 回到过去
- 如果我有一天不小心删除了某个文件，我们可以通过历史备份找回来
### 多端共享
- Git仓库可以通过PC端、Android、IOS移动端等各个终端访问
- 可以随时随地修改代码,公司没干完的工作回家接着干，可以在家提交，但是svn不可以
### 团队协作
- 多个人或团队合作编写一个项目
- 合并代码处理冲突

## git 和 svn 的区别，下面说的主要是git的优势
- 速度快
- git是分布式的
- 无需联网
- git把内容按元数据方式存储
- 强大的分支管
## markdown的配置
file - settings - plugin - browser - 搜索markdown
## git的安装
### windows下安装
- 下载地址 http://git-scm.com
### mac的安装
- 先安装homebrew 下载Homebrew http://brew.sh
```
brew install git
```
> 安装xcode,会默认下载git

## 配置git邮箱和密码
### 先查看配置，如果查看没有配置进行配置
```
git config --global user.name
git config --list  //查看所有配置
```
### 进行配置，如果不配置不能提交代码,只需要配置一次
```
git config --global user.name "用户名" //最好使用github的用户名
git config --global user.email "自己的邮箱"
```
##  创建文件夹
- 自定义快捷键 file-->settings-->搜索living template  把不想写的设置成快捷键
```
mkdir test //创建文件夹
cd test  //进入文件夹，改变目录
```

## 初始化git仓库
- 先创建一个空目录 ，然后进入此目录
- 点击右键选择Git-Bash打开命令行
- 输入git init命令把这个目录变成Git可以管理的仓库
```
git init
```
> ls -al显示全部文件 会产生一个.git的隐藏文件，用来管理我们的文件
.git文件存储着所有的内容(提交，版本库，标签)，初始化后会给一个分支默认叫master

## 对文件夹里面文件的操作
### 新建文件
```
touch index.txt
```
### 像文件内写入内容
```
echo hello > index.txt //如果文件不存在则会创建文件
echo word >> index.txt //向文件中追加内容
```
### 查看文件内容
```
cat index.txt
```
> 红色表示未加入暂存区内,就是没有增加之前的

### 删除文件
```
rm index.txt
```
> ls 查看文件；clear 是清屏

### 编辑文件，可以往里面写入内容
```
vi index.txt
```
> 进入插入模式进行编辑 i，esc退出编辑，：wq 保存并退出

### 打印之前的命令行中输入的命令
```
history > index.txt
```
## git中的三个区
- 工作流程
http://card.mugeda.com/campaigns/56d2c4a0a3664e3308000407/20160304090522/56d97729a3664e9c65000047/index.html
### 工作区
- 添加文件到暂存区中
```
git add file
git add . / -A
```
### 暂存区/缓存区
- 提交到版本库/历史区
```
git commit -m"此次提交的注释内容"
```
> 暂存区特点：过渡的作用，避免误操作，保护工作区和历史区，分支处理;
-m ：message就是消息的意思；svn里面没有暂存区

### 版本库/历史区
- 提交到版本库中通过git log 进行查看日志，里面的内容是注释的内容
```
git log --oneline  //查看一行
```
### 修改时查看git当前状态
```
git status
```
### 搜索日志(版本里面的)
```
git log --author/--grep=co //grep搜索的意思
```
> co 是搜索注释的内容 是git commit -m"注释内容"里面的注释内容

## 代码的比较，看代码的不同
- 比较工作区和缓存区
```
git diff //默认是工作区和缓存区进行比较
```
- 比较缓存区和历史区
```
git dif --cached //cached：缓存的意思
```
- 比较工作区和历史区，一般很少比较
```
git diff master //master 分支是什么名字这里就写什么名字
```

## 撤销
从缓存区中撤销add内容
```
git reset HEAD index.txt
```
### 撤回文件里的内容，就是工作区的内容有更改完了又不想改了
如果工作区有更改 ，我想用暂存区域或者历史区的内容去覆盖掉我们的工作区域
先查找暂存区 ，查找不到 查找历史区
用暂存区替换掉工作区里的内容，就是从暂存区回到工作区
```
git checkout index.txt
```
## 合并提交
如果我们使用git commit 会产生一个新的版本，但是我们此次的提交，还想算作上次的提交 
```
git commit --amend //可以选择是否用以前的提交注释
```
> 如果进入编辑模式按esc键 输入:wq

### 从版本库(历史区)拉回代码 回滚 回到过去，就是从历史区回到工作区
- git log 查看版本号（7位以上）如果太长了不想看，按q退出
```
git reset --hard 版本号
```
## 在回到未来 就是在回到历史区
- 通过git reflog 查看未来的版本，使用reset回到未来
```
git reflog //获取所有操作的版本号
git reset --hard 版本号
```

### 删除缓存区，但是要保证工作区里内容必须不存在
```
git rm
```
## 只删除缓存区
```
git rm --cached filename
```
## 同时删除暂存区和工作区
```
git rm -force 文件的名字
```
> rm -rf .git 递归删除，删除了就在也找不回来了

### 要把写好的文件推送到远程仓库（github仓库）上
- 创建新仓库（空的里面什么文件都不能有）
- 推送的时候空的文件夹是不会被提交的
- 在提交之前想要忽略掉一些文件(.gitignore)
- 添加到历史区中 git commit -m"haha"
- 添加远程仓库地址
### 创建新仓库后需要关联远程仓库
```
git remote add origin '仓库的地址'
```
### 推送到远程仓库，就是把写好的文件推上去gitHub
```
git push -u origin master
```
> -u的意思是upstream 如果下次再提交可以直接使用git push

### 从工作区 直接提交到历史区（必须走一下过渡区）
```
git commit -a -m"add"
```
> 不支持首次提交，如果文件没有加到过缓存区中是不能使用这种方式的

## 分支管理
- 查看所有分支
```
git branch
```
- 创建分支
```
git branch 分支名
```
- 切换分支
```
git checkout 分支名(dev)
```
> 相当于把master复制了一份到dev里，如果在dev分支里创建文件在master里也可以看到，如果在dev里提交master上才看不到dev上的内容，就是在哪里提交就是哪里的，在另一个里面就看不到了

- 删除分支
```
git branch -D dev
```
> 得先把分支切换到master，不能自己删除自己

## 创建分支并且切换分支
```
git branch dev
git checkout dev
git checkout -b dev  //这是把上面2条合并成1条简写的
```
## 合并分支
- 要切换到master身上来合并dev，就是合并两个人的代码
```
git merge dev
```
### 在合并分支时有冲突我们需要先把最新的拉下来
- 先使用git fetch拉下来 我们可以先比较一下不同在合并
```
git fetch
git merge
```
git pull  = get fetch + git merge

- 产生冲突(就是两个人同时改了一个文件)比如：
    - 1.在dev分支中改变hello.js文件，把里面的内容改成了helloword，进行提交
    - 2.在master分支中改变hello.js文件，把里面的内容改成了helloword123，进行提交
    - 3.合并时会产生冲突，解决冲突 删除>>>>  ==== <<<< 保存剩余的再次提交即可

## 发布静态页面
- 发布页面要求分支的名字必须叫做gh-pages(并且把内容推送到这个分支上)
- 1.创建页面(html)，写上内容
- 2.切换到gh-pages分支上
- 3.提交到分支上
- 4.推到远程仓库

### 每天完成的代码 提交流程
- 领导建立一个仓库
- 所有的'组长'都需要fork领导的仓库(只能fork一次)
> fork：将现在你看好的仓库代码复制一份，放到自己的仓库下(领导的代码发生变化不会影响自己的)

### 组长提交作业
- 将fork领导的仓库下载下来,将自己的作业丢进去
```
git clone 自己的仓库地址 别名(可有可无，就是改变文件夹的名字)
```
- 当我们建立好一个仓库后不是所有文件都要提交的，要忽略掉一些文件
    - 在文件根目录下创建一个.gitignore文件去忽略掉不想要的
    .idea
    node_modules
    .DS_Store
    bower_component
- 提交到自己的仓库下
```
git add .
git commit -m ''
git push origin master
```
> 克隆后会默认提供origin地址

### 发送合并请求
- 你的项目是从领导这克隆下来的，可以直接发送请求
- new pull request
### 组长需要给组员发配权限
- 让组员操作自己的仓库 自己项目下-> settings -> collabrators -> 添加 -> 给组员发送邀请函 ->组员同意即可 （组员拥有组长所有权限）
- 组员需要将组长的仓库拉到本地
> git clone 组长的地址

- 在提交到组长仓库之前要拉取组长的最新代码
> 防止你写代码时 别人有提交的代码，你需要先拉取别人，更新成最新代码提交到仓库上
  将自己的作业提交到组长的仓库上，在根目录下提交，相当于作业提交给了组长

### 组长将作业提交给老师
- 先拉取自己仓库最新代码
- 拉取领导的最新代码
git remote add teacher 领导的仓库地址
- 提交到自己的仓库下，在发送pull request请求









