# php开发人员相关环境说明：

## 开发环境  
### WINDOWS 10 

官网：
[https://www.microsoft.com/zh-cn](https://www.microsoft.com/zh-cn)  
版本：专业版  
下载地址：
[https://www.microsoft.com/zh-cn/software-download/windows10](https://www.microsoft.com/zh-cn/software-download/windows10)  

### WINDOWS 7

官网：
[https://www.microsoft.com/zh-cn](https://www.microsoft.com/zh-cn).  
版本：专业版  
下载地址：
[http://www.xitongzhijia.net/win7/](http://www.xitongzhijia.net/win7/) 

### linux centos和ubantu 系列
官网：
[https://www.linux.org/](https://www.linux.org/)  
版本：专业版  
下载地址：
[https://www.linux.org/pages/download/](https://www.linux.org/pages/download/) 
说明：根据相应功能开发需求进行相应版本下载


## 浏览器 

### Chrome
官网：
[http://www.google.cn/chrome/browser/desktop/index.html](http://www.google.cn/chrome/browser/desktop/index.html)  
版本：64.0.3282.119 m(最新版)  
下载地址：
[http://rj.baidu.com/soft/detail/14744.html?ald](http://rj.baidu.com/soft/detail/14744.html?ald)

### Firefox
官网：[http://www.mozilla.com](http://www.mozilla.com)  
版本：58.0.0.6592 
下载地址：[https://www.mozilla.org/zh-CN/firefox/new/?scene=2](https://www.mozilla.org/firefox/new/?scene=2)  


### IE11
官网：
[https://support.microsoft.com/zh-cn/help/18520/download-internet-explorer-11-offline-installer](https://support.microsoft.com/zh-cn/help/18520/download-internet-explorer-11-offline-installer)  
版本：11  
下载地址：[IE11-Windows6.1-x64-zh-cn.exe](http://download.microsoft.com/download/5/6/F/56FD6253-CB53-4E38-94C6-74367DA2AB34/IE11-Windows6.1-x64-zh-cn.exe)

## 开发工具 

### phpstorm(推荐)
官网：
[http://www.jetbrains.com/phpstorm/](http://www.jetbrains.com/phpstorm/)  
版本：2017.3.3  
下载地址：[http://www.jetbrains.com/phpstorm/download/#section=windows](http://www.jetbrains.com/phpstorm/download/#section=windows)  

### zend studio 
官网：
[http://www.zend.com/en/products/studio](http://www.zend.com/en/products/studio)  
版本：13.6.1  
下载地址：[http://www.zend.com/en/products/studio/downloads#Windows](http://www.zend.com/en/products/studio/downloads#Windows)  

### netbeans 
官网：
[https://netbeans.org/](https://netbeans.org/)  
版本：8.2  
下载地址：[https://netbeans.org/downloads/](https://netbeans.org/downloads/)  

### eclipse
官网：
[https://www.eclipse.org](https://www.eclipse.org)  
版本：4.7.0 
下载地址：[https://www.eclipse.org/downloads/](https://www.eclipse.org/downloads/)  



### visual studio 2015
官网：
[https://beta.visualstudio.com/zh-hans/](https://beta.visualstudio.com/zh-hans/)  
版本：14.0.25425.01 Update 3  
下载地址：[go.microsoft.com-visual](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409)  
安装配置步骤：安装文件有提示

### editplus
官网：[https://www.editplus.com/](https://www.editplus.com/)  
版本：4.1  
下载地址：[www.editplus.com](https://www.editplus.com/download.html)

### SublimeText
官网：
[http://www.sublimetext.com/3](http://www.sublimetext.com/3)  
版本：3.3143
下载地址：[http://www.sublimetext.com/](http://www.sublimetext.com/)  

### Notepad ++
官网：
[https://notepad-plus.en.softonic.com/](https://notepad-plus.en.softonic.com/)  
版本：6.8.7  
下载地址：[https://notepad-plus.en.softonic.com/](https://notepad-plus.en.softonic.com/)



## 集成环境phpStudy
官网：[http://www.phpstudy.net/](http://www.phpstudy.net/)  
版本：2018 
下载地址：[http://www.phpstudy.net/](http://www.phpstudy.net/)
说明：根据具体情况进行版本调节和控制

## 数据库
### MariaDB 
官网：
[http://mariadb.org/](http://mariadb.org/)  
版本：10.2.12  
下载地址：
[downloads.mariadb.org](https://downloads.mariadb.org/mariadb/10.2.12/)  
安装配置步骤：  
1.安装

推荐安装路径：D:\db\mariadb\10.2.12
将mariadb-10.2.12-winx64.zip解压到安装路径  
将安装目录下的my-large.ini复制一份到(同一)安装目录并改名为my.ini  
将安装路径的bin目录(比如: D:\db\mariadb\10.2.12\bin)，添加到环境变量path   

2.设置数据目录  
选择一个数据文件目录，比如：F:\\_dbfiles\mariadb\data 
将这个目录路径的 \ 换成 /，比如：F:/_dbfiles/mariadb/data 
设置为my.ini文件datadir选项的属性。
(顺便添加另外两个设置)

	[mysqld]
	skip-name-resolve
	character-set-server = utf8
	datadir = F:/_dbfiles/mariadb/data 

将安装目录data文件夹(比如：D:\db\mariadb\10.2.12\data)中的所有文件，都拷贝到你的新的datadir中,比如：F:\_dbfiles\mariadb\data   
(注意datadir的斜杠和windows路径的斜杠不同)  

3.服务  
打开命令行，进入安装目录的bin目录(比如: D:\db\mariadb\10.2.12\bin)输入以下命令：
    
    \> mysqld --install-manual MariaDB  
修改服务启动类型 (sc config /?) auto-自动,demand-手动,disabled-禁用  
\> sc config MariaDB start= auto  
启动服务  
\> sc start MariaDB  
4.修改root密码  
\> mysqladmin -u root -p password sa  

5.允许远程连接(可选)

	mysql -u root -p  
	use mysql;  
	select user,password,host from user; 
	//给IP-xxx.xxx.xxx.xxx赋予了所有的权限，包括远程访问权限。  
	grant all privileges on *.* to root@"xxx.xxx.xxx.xxx" identified by "密码";  
	grant all privileges on *.* to root@"192.168.104.50" identified by "sa";  
	flush privileges;  
	select user,password,host from user;  

### mysql
官网：[https://www.mysql.com/](https://www.mysql.com/)  
下载地址：[https://www.mysql.com/downloads/](https://www.mysql.com/downloads/)
说明：根据实际应用选择相应版本下载。

### redis
官网：[https://redis.io/](https://redis.io/)  
版本：4.0.7 
下载地址：[https://redis.io/download](https://redis.io/download)

### mongodb 
官网：[https://www.mongodb.com/](https://www.mongodb.com/)  

### HeidiSQL
官网：[http://www.heidisql.com/](http://www.heidisql.com/)  
版本：9.5  
下载地址：[www.heidisql.com-installers](https://www.heidisql.com/download.php) 

### 多线程swoole

入门和配置：[https://wiki.swoole.com/](https://wiki.swoole.com/)

## navicat premium
官网：[http://www.navicat.com.cn/](http://www.navicat.com.cn/)  
版本：11.1.14.0 
下载地址：[http://rj.baidu.com/soft/detail/24309.html?ald](http://rj.baidu.com/soft/detail/24309.html?ald)  


## 代码管理工具
### git
官网：
[https://git-scm.com/](https://git-scm.com/)  
版本：2.16.1  
下载地址：[https://git-scm.com/download/win](https://git-scm.com/download/win)

### TortoiseGit
官网：
[https://tortoisegit.org/](https://tortoisegit.org/)  
版本：2.5.0  
下载地址：[https://tortoisegit.org/download/](https://tortoisegit.org/download/)

## 文档管理工具

### wps
官网：
[http://www.wps.cn/](http://www.wps.cn/)  
版本：2016 
下载地址：[http://www.wps.cn/product/wps2016/](http://www.wps.cn/product/wps2016/)

### markdown
官网：
[http://www.markdown.cn/](http://www.markdown.cn/)  

### ShowDoc
官网：
[https://www.showdoc.cc/](https://www.showdoc.cc/)  

## 加密工具
### Zend Guard 7
官网：
[http://www.zend.com](http://www.zend.com)  
版本：7.0  
下载地址：[http://www.zend.com/en/products/guard/downloads#Windows](http://www.zend.com/en/products/guard/downloads#Windows)

##其它工具

### Lantern(翻墙软件)
官网：
[http://lanterncn.org/](http://lanterncn.org/)  
版本：3.1.0  
下载地址：[lantern-binaries/lantern-installer-beta.exe](https://raw.githubusercontent.com/getlantern/lantern-binaries/master/lantern-installer-beta.exe)

### 7-zip(解压缩文件)
官网：
[http://www.7-zip.org/](http://www.7-zip.org/)  
版本：16.02  
下载地址：[www.7-zip.org](http://www.7-zip.org/a/7z1602-x64.exe)


### evernote(笔记工具)
官网：
[https://dev.evernote.com/](https://dev.evernote.com/)  
下载地址：[evernote-master.zip](https://github.com/evernote/evernote-cloud-sdk-windows/archive/master.zip)


### Fiddler(抓包工具)
官网：[http://www.telerik.com/fiddler](http://www.telerik.com/fiddler)  
版本：4.6.2.0  
下载地址：[download-fiddler](http://www.telerik.com/download/fiddler)  

### Beyond Compare
官网：[http://www.scootersoftware.com/download.php](http://www.scootersoftware.com/download.php)  
版本：4.1.6     
下载地址：[BeyondCompare-download](http://www.scootersoftware.com/BCompare-4.1.6.21095.exe)

### MkDocs(项目文档工具)
官网：[http://www.mkdocs.org/](http://www.mkdocs.org/)  
下载地址：
[www.mkdocs.org](https://github.com/mkdocs/mkdocs/zipball/master)  
安装：安装python后运行命令：pip install mkdocs

### Postman(Http请求模拟工具)
官网：[https://www.getpostman.com/](https://www.getpostman.com/)  
下载地址：
[https://app.getpostman.com/app/download](https://app.getpostman.com/app/download)  
