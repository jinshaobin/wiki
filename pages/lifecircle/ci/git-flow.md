git 教程
==========

##git-flow
### 简介

git-flow是一套基于git的工作流程，这个工作流程围绕着project的发布(release)定义了一个严格的如何建立分支的模型。

Git建分支是非常容易的，我们可以任意建立分支，对任意分支再分支，分支开发完后再合并。

比较推荐、多见的做法是特性驱动(Feature Driven)的建立分支法(Feature Branch Workflow)。

简而言之就是每一个特性(feature)的开发并不直接在主干上开发，而是在分支上开发，分支开发完毕后再合并到主干上。

这样做的好处是

1.还处于半成品状态的特性(feature)不会影响到主干

2.各个开发人员之间做自己的分支，互不干扰

3.主干永远处于可编译、可运行的状态

git-flow则在这个基础上更进一步，规定了如何建立、合并分支，如何发布，如何维护历史版本等工作流程。

### master和develop分支
![Alt text](/../pages/image/flow/flow1.png "flow1")

master分支只存放历史发布(release)版本的源代码。各个版本通过tag来标记。上图里的v0.1和v0.2就是tag。

develop分支则用来整合各个feature分支。开发中的版本的源代码存放在这里。

### feature分支

![Alt text](/../pages/image/flow/flow2.png "flow2")

每一个特性(feature)都必须在自己的分支里开发，feature分支派生自develop分支。

当feature开发完毕后，要合并回develop分支。feature分支永远不会和master分支打交道。

### release分支
![Alt text](/../pages/image/flow/flow3.png "flow3")

release分支不是一个放正式发布产品的分支，你可以将它理解为“待发布”分支。

我们用这个分支干所有和发布有关的事情，比如：

1.把这个分支打包给测试人员测试

2.在这个分支里修复bug

3.编写发布文档

所以在这个分支里面绝对不会添加新的特性。

当和发布相关的工作都完成后，release分支合并回develop和master分支。

单独搞一个release分支的好处是，当一个团队在做发布相关的工作时，另一个团队则可以接着开发下一版本的东西。

### hotfix分支
![Alt text](/../pages/image/flow/flow4.png "flow4")

一个项目发布后或多或少肯定会有一些bug存在，而bug的修复工作并不适合在develop上做，这是因为

1.develop分支上包含还未验证过的feature

2.用户未必需要develop上的feature

3.develop还不能马上发布，而客户急需这个bug的修复。

这时就需要新建hotfix分支，hotfix分支派生自master分支，仅仅用于修复bug，当bug修复完毕后，马上回归到master分支，然后发布一个新版本，比如v0.1.1。

同时hotfix也要合并回develop分支，这样develop分支就能享受到bug修复的好处了。

### 配套工具

git-flow不仅仅是一种规范，还提供了一套方便的工具。大大简化了执行git-flow的过程。

####安装
#####OSX

	$ brew install git-flow

#####Debian/Ubuntu Linux

	$ apt-get install git-flow

#####Windows(cygwin)

	$ wget -q -O - --no-check-certificate https://github.com/nvie/gitflow/raw/develop/contrib/gitflow-installer.sh | bash

### Initialize(初始化)
对一个git仓库配置一下git-flow。主要是一些命名规范，比如feature分支的前缀，hotfix分支的前缀等。一般用默认值就行。

	git flow init

### Feature(特性)
#### Start a new feature

从develop开启一个新的分支

    git flow feature start MYFEATURE

这个命令会从develop分出一个分支，然后切换到这个分支上面。

#### Finish up a feature
一个feature分支开发完毕后，要做以下事情：
1.把 MYFEATURE 合并到 develop
2.把这个分支干掉
3.切换回develop分支

    git flow feature finish FEATURE_NAME

#### Publish a feature

如果你想让别人和你一起开发MYFEATURE分支，那就把这个分支push到服务器上
	
	git flow feature publish MYFEATURE

#### Getting a published feature
获得一个别人publish到服务器上的feature分支
	
	git flow feature pull origin MYFEATURE

#### Release
##### 	Start a release

创建一个release分支，派生自develop分支。
	
	git flow release start RELEASE

##### 	Publish a release
	
	git flow release publish RELEASE

##### 	Finish up a release	

一个release分支结束后，需要做以下工作：

1.把release分支合并回master
2.给本次发布打tag
3.同时把release分支合并回develop
4.干掉release分支

	git flow release finish RELEASE

最后不要忘记把tag push到服务器git push --tags

### Hotfix
#### git flow hotfix start
开启一个hotfix分支 

	git flow hotfix start VERSION
Finish a hotfix
结束一个hotfix分支，和release一样，同时合并回develop和master

	git flow hotfix finish VERSION	

###参考资料

[git相关书籍](https://www.git-tower.com/learn/git/ebook/cn/command-line/introduction#start)
[git-flow官网](https://github.com/nvie/gitflow)
[git-flow流程(英文)](https://danielkummer.github.io/git-flow-cheatsheet/)
[git-flow流程(中文)](https://danielkummer.github.io/git-flow-cheatsheet/index.zh_CN.html)
[git-flow模型(英文)](http://nvie.com/posts/a-successful-git-branching-model/)
[git-flow模型(中文)](http://blog.csdn.net/dbzhang800/article/details/6798724)
[git-flow自动化工作流(英文)](https://jeffkreeftmeijer.com/git-flow/)
[git-flow自动化工作流(中文)](http://stormzhang.com/git/2014/01/29/git-flow/ )

##git环境配置
#### git环境配置

1.键盘上点击windows，点击“所有程序”找到“TortoiseGit”打开点击”PuTTYgen”。

2.点击 “generate”, 出现如下图的进度条，将鼠标在Key选项框中空白处随意移动，进度条将行进。

![Alt text](/../pages/image/flow/generate.png "generate")

3.选择Save private key，弹出对话框，选择是，保存私钥至private.ppk\选择Save public key保存公钥至authorized_keys文件,配置SSH登录密钥，如图运行TortoiseGit软件包中的Pageant程序。

![Alt text](/../pages/image/flow/Pageant.png "Pageant")


4.打开浏览器,输入http://git.gigahome.cc/users/sign_in进行登录。

5.点“Add SSH Key”，填上任意 Title，在 Key 文本框里粘贴Putty通过generate里面生成的内容，点击保存就OK了.

![Alt text](/../pages/image/flow/s1.png "s1")![Alt text](/../pages/image/flow/s2.png "s2")

6.复制远程仓库中的项目地址

![Alt text](/../pages/image/flow/copy1.png "copy1")

7.建立一个新文件夹，打开文件夹点击右键选择GIT Clone

* 把在复制的地址粘贴URL里；
* 保存的地址；
* 保存下来的private key 值；点击OK

![Alt text](/../pages/image/flow/copy2.png "copy2")