php架构
==========

##架构设计的流程
------------------

1、 网络拓扑图

2、 应用部署图

3、 架构图

4、 系统结构图

5、 系统交互图

6、 接口列表

7、 数据库ER图，表格，索引，示图，存储过程

8、 功能，需求列表




##laravel框架（入门指引）
------


### 版本选择


Laravel 有两个版本类型：


- LTS 版本 - 长期支持版本，此类版本是 Laravel 能提供的最长时间维护版本。
- 一般发行版 - 只提供 6 个月的 Bug 修复支持，一年的安全修复支持。




| 版本     | 发布日期    | 版本类型     |  维护周期 |
|:-------:|:------:|:--------:|:------:|:--------:|
| Laravel 5.1  | 2015 年 6 月    | LTS 长久支持   | Bug 修复 2017 年 6 月份，安全修复 2018 年 6 月份
| Laravel 5.3	 | 2016 年 8 月   | 一般发行 | 提供 6 个月的 Bug 修复支持，一年的安全修复支持
| Laravel 5.4     | 2017 年 1 月 | 一般发行 | 提供 6 个月的 Bug 修复支持，一年的安全修复支持
| Laravel 5.5    | 2017 年 8 月  | LTS 长久支持 | Bug 修复 2019 年 8 月份，安全修复 2020 年 8 月份
| Laravel 5.6     | 2018 年 2 月 | 一般发行 | 提供 6 个月的 Bug 修复支持，一年的安全修复支持


优先考虑 LTS 版本  **``( Laravel 5.5 )``**



### 手册及相关文档


- 官方文档： [https://laravel.com/docs/5.5](https://laravel.com/docs/5.5)
- 中文文档： [https://d.laravel-china.org/docs/5.5](https://d.laravel-china.org/docs/5.5)
- 社区 :   [https://laracasts.com/](https://laracasts.com/skills/laravel)
- 国内社区 ： [https://laravel-china.org](https://laravel-china.org)
- laravel 编程进阶（中英文对照）： [From Apprentice To Artisan](https://my.oschina.net/zgldh/blog/389246)
- 点灯坊博客 (Laravel 架构指引)： [oomusou.io](http://oomusou.io/tags/#Laravel)
- 国内某牛人自建社区 : [https://www.codecasts.com/learn/laravel](https://www.codecasts.com/learn/laravel)
- [PHP The-Right-Way](https://laravel-china.github.io/php-the-right-way/)




### 安装起步

> 环境要求：

- PHP >= 7.0.0
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
- composer


* 安装 [composer](http://getcomposer.org/download)
* composer 中国地区镜像配置


```
$ composer config -g repo.packagist composer https://packagist.phpcomposer.com
```


1.Composer 安装 laravel 

```
 $ composer create-project --prefer-dist laravel/laravel projectName
```
    

2.Laravel Installer 安装 Laravel 

```
$ composer global require "laravel/installer"
```

确定系统的环境变量中包含了  **``$HOME/.composer/vendor/bin``**


- macOS: $HOME/.composer/vendor/bin
- Linux: $HOME/.config/composer/vendor/bin
- Windows : 控制面板 -> 系统属性 -> 高级系统设置 -> 环境变量 
  添加 laravel-installer 的 路径到 PATH 即可


```
laravel new projectName
```  



### Laravel 基本配置


1.服务器基本配置 (nginx) :

在站点配置中加入以下内容, 将会将所有请求都引导到 index.php 前端控制器：

```
location / {
    try_files $uri $uri/ /index.php?$query_string;
}
```


2.环境配置：

Laravel 利用 ``Vance Lucas`` 的 PHP 库 ``DotEnv`` 使得此项功能的实现变得非常简单。

在新安装好的 Laravel 应用程序中，其根目录会包含一个 ``.env.example`` 文件。如果是通过 Composer 安装的 Laravel，该文件会自动更名为`` .env``。否则，需要手动更改一下文件名。


**``.env`` 文件不应该提交到应用程序的源代码控制系统中，因为每个使用应用程序的开发人员/ 服务器可能需要有一个不同的环境配置。**

更多配置参考 [laravel.com/configuration](https://d.laravel-china.org/docs/5.5/configuration)



### 快速入门

[# Quick Start](http://laravel.com/docs/5.1/quickstart)


### Laravel package 扩展

- Composer package 仓库 (使用 composer 安装 ）[packagist.org](https://packagist.org/)

- Laravel 套件库  [Laravel package](http://packalyst.com/)

- Laraval 自身常用的包组件
    - illuminate/session    &nbsp;&nbsp; **Session package**

    - illuminate/contracts   &nbsp;&nbsp; Contracts package.

    - illuminate/database    &nbsp;&nbsp; Eloquent ORM
    
    - illuminate/queue      &nbsp;&nbsp; Queue package

    - illuminate/container &nbsp;&nbsp; Container package

    - illuminate/events &nbsp;&nbsp; Events package. 

- 初始化流程

    [overview](https://github.com/huanghua581/laravel-getting-started/wiki/Laravel-%E5%88%9D%E5%A7%8B%E5%8C%96%E6%B5%81%E7%A8%8B)  



### Debug 技巧

1. 框架自身调试工具：[laravel-debugbar](https://github.com/barryvdh/laravel-debugbar)

2. IDE 调试技巧

  [PHPStorm 调试](https://laravel-china.org/articles/4098/the-first-step-to-becoming-a-senior-php-programmer-debug-xdebug-configuration)



##权限控制RBAC
------

参考文献：
RBAC模型：[Role-Based Access Control Models](https://csrc.nist.gov/CSRC/media/Projects/Role-Based-Access-Control/documents/sandhu96.pdf)
本地访问链接：[Role-Based Access Control Models](/../pages/chapter/sandhu96.pdf)
