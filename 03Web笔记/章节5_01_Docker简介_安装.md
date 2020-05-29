# 一. Docker简介

* Docker是开源的应用容器引擎.托管在Github上
* Docker使用Go语言进行开发,采用Apach2.0协议
* Docker最开始基于Linux的LXC(Linux Container)技术,在此基础上进一步封装.所以Docker只能运行在Linux.后来推出了Mac版和Window版本
* 在搜索Docker时,Docker官网显示了`Docker - Build, Ship, and Run Any App, Anywhere`说明Docker致力于让开发者在任何位置构建、运输、运行任何App
* 在官网上下载的Windows版本要求必须是Win10,其他版本需要下载dock-toolbox.Docker for Windows也就是Docker CE for Windows，是Docker公司发布了全新的Docker版本，同时将Docker分成CE和EE2个版本，CE版本即社区版，免费使用
* Docker本质就是多个虚拟系统(windows/linux)在系统中配置好项目运行环境,保证项目的运行

# 二. Docker安装

* 本节中安装系统为win10,需要开启Hyper-V
  * win10家庭版没有Hyper-v,需要升级到专业版或企业版
  * 控制面板->程序->程序和功能->启用和关闭Windows功能->Hyper-V勾选
* 在`02资料/章节5_01_Docker安装_简介/InstallDocker.msi`中给各位同学已经下载好Docker的安装包双击运行即可安装
* 安装后可以在命令行输入`docker --help`查看Docker支持的命令,或使用`docker -v`查看安装的版本