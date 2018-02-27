# 服务器配置

##文档介绍

### 1.文档目的

阐述服务器搭建kvm方案

### 2.术语与缩写解释
| 缩写、术语  | 解 释  |
| ------------ | ------------ |
|Centos   |Centos操作系统   |
|  kvm | 虚拟机  |
| gitlab | 代码版本控制软件   |
| wiki   |  信息发射源，信息同步平台  |
|samba | 文件共享服务|


##背景介绍

整合公司技术项目，统一资源存放和管理。 
 
## 技术方案

	Centos7.2+kvm+gitlab+wiki+samba

## kvm安装说明

1. 验证CPU是否支持KVM；如果结果中有vmx（Intel）或svm(AMD)字样，就说明CPU的支持的。

		egrep '(vmx|svm)' /proc/cpuinfo
	
	![Alt text](/../pages/image/set/set1.png "set1")
2. 关闭SELinux，将 /etc/sysconfig/selinux 中的 SELinux=enforcing 修改为 SELinux=disabled 

	![Alt text](/../pages/image/set/set2.png "set2")

3. 最小安装的CentOS安装一些最基本的包（可选项，因为我是刚安装好的CentOS，所以为了下面方便点，先把一些必要的工具安装下）

		yum install epel-release net-tools vim unzip zip wget ftp -y

4. 安装KVM及其依赖项

	KVM组件需要安装的包

	![Alt text](/../pages/image/set/set27.png "set27")

	检查这些包是否安装

		rpm -q qemu-kvm libvirt python-virtinst virt-viewer virt-manager bridge-utils
	安装这些包

		yum -y install qemu-kvm libvirt python-virtinst virt-viewer virt-manager virt-install bridge-utils 

5. 验证安装结果，下图说明已经成功安装了

		lsmod | grep kv

	![Alt text](/../pages/image/set/set3.png "set3")
6. 开启kvm服务，并且设置其开机自动启动
	
		systemctl start libvirtd

		systemctl enable libvirtd
7. 查看状态操作结果,如下图所示，说明运行情况良好
	
		systemctl status libvirtd
		systemctl is-enabled libvirtd

	![Alt text](/../pages/image/set/set4.png "set4")

8. 配置网桥模式，先将 /etc/sysconfig/network-scripts/ 目录下的网卡配置文件备份一份(不要备在当前目录以及子目录下，其他目录随意)
a. 创建 ifcfg-br0 文件，内容如下：

		DEVICE=br0
		TYPE=Bridge
		NM_CONTROLLED=no
		BOOTPROTO=static
		IPADDR=172.16.233.20
		PREFIX=24
		ONBOOT=yes
		GATEWAY=172.16.233.1
		DNS1=8.8.8.8	
b. 移除掉原来的 ifcfg-p2p1,重新创建该文件，内容如下：

		DEVICE=p2p1
		TYPE=Ethernet
		HWADDR=b0:83:fe:96:f8:db
		ONBOOT=yes
		NM_CONTROLLED=no
		BRIDGE=br0
c. 重启网络服务

		systemctl restart network
	使用 ip addr验证操作结果,多了一块网卡br0

	![Alt text](/../pages/image/set/set5.png "set5")

## kvm安装centos

###一.安装centos

1. 准备操作系统安装镜像文件，在本文中将使用和宿主环境一样的CentOS7.2，把该文件放到 /home/iso 目录下

	![Alt text](/../pages/image/set/set6.png "set6")

2. 创建虚拟机文件存放的目录

		mkdir -p /home/kvm-bak
3. 使用 virt-install 创建虚拟机

		virt-install -n localhost-kvmbase -r 2048 --disk 
		/home/kvm-bak/localhost-kvmbase.img,format=qcow2,size=20 --network 
		bridge=virbr0 --os-type=linux --os-variant=rhel7.2 --cdrom 
		/home/iso/CentOS-7-x86_64-DVD-1708.iso --vnc --vncport=5910 --vnclisten=0.0.0.0
	
	 操作结果显示：

	![Alt text](/../pages/image/set/set7.png "set7")

	不要理会里面提示的错误，接着往下走

4. 打开防火墙上的5910端口

		firewall-cmd --zone=public --add-port=5910/tcp --permanent
		firewall-cmd --reload

	![Alt text](/../pages/image/set/set8.png "set8")

5. 使用VNC连接该虚拟机，进行虚拟机操作系统的安装，直接到VNC官网上下载最新版的VNC Viewer即可
	a. VNC Viewer

	![Alt text](/../pages/image/set/set9.png "set9")

	b. 新建连接，提供宿主IP、端口号(在virt-install创建过程中指定的)，以及名称

	![Alt text](/../pages/image/set/set10.png "set10")

	接下来就安装CentOS吧，过程略。
6. 安装完CentOS，系统要求重启，这时候虚拟机没有重启(也不知是因为什么问题)，VNC也连不上，先在宿主机上查看虚拟机状态，显示think8848-kvmbase为关闭状态

		virsh list --all

	![Alt text](/../pages/image/set/set11.png "set11")

7. 手动启动虚拟机
		virsh start think8848-kvmbase

	![Alt text](/../pages/image/set/set12.png "set12")

	再次使用VNC连接，发现已经可以连进去了

	![Alt text](/../pages/image/set/set13.png "set13")

###二.简单配置KVM虚拟机

1. 网桥配置，这里值得一提的是，如果你在虚拟机中安装CentOS过程中，配置了合适的网格参数，那么这时虚拟机里应该就可以使用网络了。如果当时就采用系统安装包的默认参数，未启用网卡，那么此时你需要启动虚拟机的网卡，先看下虚拟机网卡的配置文件列表，貌似和普通电脑安装没啥区别，网卡的配置文件是 ifcfg-eth0 

	![Alt text](/../pages/image/set/set14.png "set14")

	再查看 ifcfg-eth0配置文件，和普通电脑安装的也没啥区别，按照要求呢，貌似在一些文章中介绍，说需要添加一行配置 NM_CONTROLLED=no ，我没添加也没发现有什么问题。

	![Alt text](/../pages/image/set/set15.png "set15")

2. 配置在宿主端登录虚拟机shell。当然在宿主端也是可以通过SSH登录，但是直接登录貌似也是一个不错的方式。这个配置主要有两个步骤：
	a. 编辑 /etc/default/grub 文件，对照如下：
	编辑前：

	![Alt text](/../pages/image/set/set16.png "set16")

	编辑后：

	![Alt text](/../pages/image/set/set17.png "set17")

	文本内容：
		GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
		GRUB_DEFAULT=saved
		GRUB_DISABLE_SUBMENU=true
		GRUB_TERMINAL="console serial"
		GRUB_SERIAL_COMMAND="serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1"
		GRUB_CMDLINE_LINUX="rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb"
		GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200"
		GRUB_DISABLE_RECOVERY="true"
	b. 运行下面一行的代码
		grub2-mkconfig -o /boot/grub2/grub.cfg
	![Alt text](/../pages/image/set/set18.png "set18")

	c. 重启虚拟机 reboot 
	d. 在宿主机上进入虚拟机控制台，想退出时使用 Ctrl 键+ ]  （左方括号）键退出。
		virsh console think8848-kvmbase

	![Alt text](/../pages/image/set/set19.png "set19")

## kvm安装ubuntu

1. 准备操作系统安装镜像文件，把该文件放到 /home/iso 目录下

	![Alt text](/../pages/image/set/set20.png "set20")

2. 使用 virt-install 创建虚拟机
	   virt-install --virt-type kvm --name ubuntu-kvmbase --ram 1024 \
	   --cdrom /home/iso/ubuntu-16.04.3-server-amd64.iso \
	   --disk /home/kvm-bak/ubuntu-kvmbase.img,format=qcow2,size=10  \
	   --network bridge=virbr0  \
	   --vnc \
	   --vncport=5911 \
	   --vnclisten=0.0.0.0 \
	   --noautoconsole  \
	   --os-type=linux 
 操作结果显示：

	![Alt text](/../pages/image/set/set21.png "set21")

	不要理会里面提示的错误，接着往下走

3. 打开防火墙上的5910端口

		firewall-cmd --zone=public --add-port=5910/tcp --permanent
		firewall-cmd --reload

	![Alt text](/../pages/image/set/set22.png "set22")

## kvm安装windows

1. 准备操作系统安装镜像文件，把该文件放到 /home/iso 目录下

	![Alt text](/../pages/image/set/set23.png "set23")

2. 使用 virt-install 创建虚拟机
		virt-install --name=win-kvmbase --ram 1024 --vcpus=2 --disk path=/home/kvm-bak/win-kvmbase.img,size=20  --accelerate --cdrom /home/iso/cn_windows_multipoint_server_2012_premium_x64_dvd_1191670.iso --vnc --vncport=5912 --vnclisten=0.0.0.0 --network bridge=virbr0 --force --autostart 
	 操作结果显示：

	![Alt text](/../pages/image/set/set24.png "set24")

	不要理会里面提示的错误，接着往下走

3. 打开防火墙上的5912端口
		firewall-cmd --zone=public --add-port=5912/tcp --permanent
		firewall-cmd --reload

	![Alt text](/../pages/image/set/set25.png "set25")

	使用vnc进入系统并安装window，过程略。

	![Alt text](/../pages/image/set/set26.png "set26")

		Windows MultiPoint Server 2012 Premium	序列号
		RTDKG-7K8HQ-3QVVW-W8NKF-Q9H43 --- 00181-82704-22746-AAOEM
		VNKYM-JBKJ7-DC4X9-BT3QR-JHRF3 --- 00181-80070-00000-AAOEM  
		QXHGN-GRJQH-K7WVV-MTXP3-YBFGD --- 00182-60000-00000-AAOEM

实例：windows服务器 http://172.16.233.24/

说明：windows服务器，没有安装web服务，所以不能直接访问，目前仅支持远程登录

## gitlab安装

1. 安装和配置需要的依赖
		sudo yum install -y curl policycoreutils-python openssh-server
		sudo systemctl enable sshd
		sudo systemctl start sshd
		sudo firewall-cmd --permanent --add-service=http
		sudo systemctl reload firewalld
安装Postfix以发送通知邮件
		sudo yum install postfix
		sudo systemctl enable postfix
		sudo systemctl start postfix
2. 添加GitLab包并安装包
	添加GitLab包仓库
		curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
	接下来，安装GitLab包
		sudo EXTERNAL_URL="http://gitlab.example.com" yum install -y gitlab-ee

实例 ：gitlab(centos) http://172.16.233.22/

## wiki安装

下载解压：https://github.com/Dynalon/mdwiki/releases/download/0.6.2/mdwiki-0.6.2.zip

实例：wiki(ubuntu) http://172.16.233.23/mdwiki.html#!index.md

##  参考文档

[CentOS7.2部署KVM虚拟机](http://www.linuxidc.com/Linux/2017-01/140007.htm)
[gitlab安装](https://about.gitlab.com/installation/#centos-7)
[github官网](http://dynalon.github.io/mdwiki/#!download.md)
[samba官网](https://www.samba.org/)
 


