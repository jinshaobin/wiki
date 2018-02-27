共享服务器
==========

### 

腾讯企业邮箱
集团EXCHANGE

本地
------

概述概述

| 名称           | IP地址        | CPU/内存/硬盘 / 其它配置  | 描述          | 物理位置 | 备注          |
| -------------- |:------------:| ------------------------ | -------------| ------- | ------------- |
| 本地服务器1     | 172.16.234.9 | N/A win2018              | 物理机        | 机柜旁边 | 胡文俊 API DOC|
| 本地服务器2    | 172.16.234.11 | N/A win2018              | 物理机        | 机柜旁边 | 赵良 |
| vm1    | 172.16.234.12 | N/A ubuntu            | 虚拟机 新git       | 本地服务器2 | N/A|
| vm2    | 172.16.234.13 | N/A ubuntu              | 虚拟机 app.gigahome.cc       | 本地服务器2 | N/A|
| vm3     |172.16.234.14 |N/A ubuntu             | 虚拟机 中转服务器 VPN桥接      | 本地服务器2 | N/A|
| Foo    | Foo | Foo           | Foo     | Foo| Foo|


生产
------



概述概述


文件夹共享及配置
------

### windows使用指南


#### 访问地址
公司共享文件夹：\\\172.16.234.21\companyDocument

#### 访问方法1
1、windows+R 调出运行框

![Alt text](/../pages/image/knowledge/knowledge1.png "knowledge1")

2、输入：\\\172.16.234.21\companyDocument

![Alt text](/../pages/image/knowledge/knowledge2.png "knowledge2")

3、点击确定以后可以看到以下共享文件夹

![Alt text](/../pages/image/knowledge/knowledge3.png "knowledge3")

#### 访问方法2

1、点击我的计算机

![Alt text](/../pages/image/knowledge/knowledge4.png "knowledge4")

2、空白处右击选择“添加一个网络位置”

![Alt text](/../pages/image/knowledge/knowledge5.png "knowledge5")

3、选择下一步，直到下面这个界面

![Alt text](/../pages/image/knowledge/knowledge6.png "knowledge6")

4、在框内输入 \\\172.16.234.21\companyDocument，如下图所示：

![Alt text](/../pages/image/knowledge/knowledge7.png "knowledge7")

5、一直点击下一步，最后出现下图所示界面

![Alt text](/../pages/image/knowledge/knowledge8.png "knowledge8")

6、以后每次打开我的计算机，会多出下图所示，即共享文件夹

![Alt text](/../pages/image/knowledge/knowledge9.png "knowledge9")

### MAC OS使用指南

1、打开Finder（或在桌面），CMD + k，可以得到以下页面：

![Alt text](/../pages/image/knowledge/share5.png "share5")

2、输入： smb://172.16.234.21/companyDocument并选择客人访问

![Alt text](/../pages/image/knowledge/share6.png "share6")

![Alt text](/../pages/image/knowledge/share7.png "share7")





#### 注意事项

1.根目录的文件夹不要删除
2.重要文件请不要放共享



### 共享文件夹说明

#### 01-市场扩展部
市场扩展部相关共享资料分享存放

#### 02-产品运营部
产品运营相关共享资料分享存放

#### 03-技术研发部
* 电子书
	各种技术相关的电子资料分享存放

* 规范文档 
	技术部各种规范分享存放

* 开发接口文档
	技术部开发接口相关文档分享存放

* 统一开发环境配置
	技术部各种开发环境相关文档分享存放

* 需求分析文档
	技术部各种需求文档分享存放

#### 04-行政及人力资源部

行政及人力资源部相关共享资料分享存放

#### 05-财务管理部  

财务部门相关共享资料分享存放

#### 06-法律合规部

法律合规部相关共享资料分享存放

#### 07-客服部

客服部相关共享资料分享存放

#### 90-超大文件临时文件夹(20M以上)

20M以上的文件请放在 “90-超大文件临时文件夹(20M以上)” 这个文件夹里

#### 99-公司晨会 

公司晨会相关内容等分享存放


### 共享文件夹配置
####**1.安装**

	[root@base ~]# yum -y install samba samba-client

####**2.配置**

 - **进入samba配置目录**

		[root@base ~]# cd /etc/samba/

 - **备份smb.conf**

 		[root@base samba]# mv smb.conf smb.conf.origin
 - **新建smb.conf**

 		[root@base samba]# vim smb.conf

	内容如下，保存并退出
		[global]
        workgroup = WorkGroup
        server string = Ted Samba Server %v
        netbios name = TedSamba
        security = user
        map to guest = Bad User
        passdb backend = tdbsam

		[FileShare]
        comment = share some files
        path = /smb/fileshare
        public = yes
        writeable = yes
        create mask = 0644
        directory mask = 0755

	    [WebDev]
        comment = project development directory
        path = /smb/webdev
        valid users = dev
        write list = dev
        printable = no
        create mask = 0644
        directory mask = 0755
注释：
workgroup 项应与 Windows 主机保持一致，这里是WORKGROUP security、map to guest项设置为允许匿名用户访问再下面有两个section，实际为两个目录，section名就是目录名（映射到Windows上可以看见）。
第一个目录名是FileShare，匿名、公开、可写
第二个目录吗是WebDev，限定ted用户访问
默认文件属性644/755（不然的话，Windows上在这个目录下新建的文件会有“可执行”属性）
 - **创建用户**

		[root@base samba]# groupadd co3
		[root@base samba]# useradd dev -g co3 -s /sbin/nologin
		[root@base samba]# smbpasswd -a dev
		New SMB password:
		Retype new SMB password:
		Added user ted.
		[root@base samba]# 

	注意这里smbpasswd将使用系统用户。
 - **创建共享目录**

		[root@base samba]# mkdir -p /smb/{fileshare,webdev}
		[root@base samba]# chown nobody:nobody /smb/fileshare/
		[root@base samba]# chown dev:co3 /smb/webdev/

	注意设置属性，不然访问不了。
 - **启动Samba服务，设置开机启动**

		[root@base samba]# systemctl start smb
		[root@base samba]# systemctl enable smb
		Created symlink from /etc/systemd/system/multi-user.target.wants/smb.service to /usr/lib/systemd/system/smb.service.
		[root@base samba]# 

 - **开放端口**

		[root@base samba]# firewall-cmd --permanent --add-port=139/tcp
		success
		[root@base samba]# firewall-cmd --permanent --add-port=445/tcp
		success
		[root@base samba]# systemctl restart firewalld
		[root@base samba]# 

	或者直接把防火墙关了也行。

####**3.使用**

**本机测试**

可以使用testparm测试samba配置是否正确

![Alt text](/../pages/image/knowledge/share1.png "share1")

![Alt text](/../pages/image/knowledge/share2.png "share2")

root用户的话，不用密码可直接查看samba服务器情况

![Alt text](/../pages/image/knowledge/share3.png "share3")

效果：

![Alt text](/../pages/image/knowledge/share4.png "share4")
