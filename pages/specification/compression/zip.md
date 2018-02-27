文件压缩规范
==========
###文件压缩

文件压缩格式统一使用.zip格式（**研发部为强制标准**）

###推荐工具7zip
官网：[7zip](https://sparanoid.com/lab/7z/) 
版本：16.04(最新版本)  
下载地址：[7zip](https://sparanoid.com/lab/7z/) 
说明：根据具体的操作系统下载相应的版本。

###7zip相关介绍

####协议许可

7zip是一款开源软件。大多数源代码都基于GNU LGPL许可协议下发布。AES代码基于BSD许可下发布。unRAR代码基于两种许可：GNU LGPL 和 unRAR 限制许可。更多下许可信息请查看：7zip许可。
####主要特征
- 使用了 LZMA 与 LZMA2 算法的 7z 格式 拥有极高的压缩比
- 支持格式：
 - 压缩 / 解压缩：7z, XZ, BZIP2, GZIP, TAR, ZIP and WIM
 - 仅解压缩：ARJ, CAB, CHM, CPIO, CramFS, DEB, DMG, FAT, HFS, ISO, LZH, LZMA, MBR, MSI, NSIS, NTFS, RAR, RPM, SquashFS, UDF, VHD, WIM, XAR, Z
- 对于 ZIP 及 GZIP 格式，7-Zip 能提供比使用 PKZip 及 WinZip 高 2-10% 的压缩比
- 为 7z 与 ZIP 提供更完善的 AES-256 加密算法
- 7z 格式支持创建自释放压缩包
- Windows 资源管理器集成
- 强大的的文件管理器
- 更给力的命令行版本
- 支持 FAR Manager 插件
- 支持 87 种语言

7zip适用于Windows 10/8 /7/Vista/XP/2013/2008/2003/2000/NT。并且有支持 Linux/Unix平台的命令行移植版本。

####压缩比

让我们用 7-Zip 和 WinRAR 5.20 进行比较。

![zip1](/../pages/image/zip1.png "zip1")

**文件设置**：完整安装后的Windows版Mozilla Firefox 34.0.5 以及 Windows 版 Google Earth 6.2.2.6613。

**压缩比结果由被压缩的数据大小而定。通常使用7zip的7z格式能比使用zip格式的压缩档案小30-70%。并且使用7zip创建的zip格式比大多数其它压缩软件创建的都小 2-10%。**
