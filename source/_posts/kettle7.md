---
title: kettle在centos7环境下的安装
date: 2019-04-26 17:24:38
tag:
---

### jdk 1.8 环境搭建。（必要）
    这里关于jdk安装省略。
### 下载：在官网下载kettle的安装包8.2版本。

### 安装：
    先安装一些依赖项：
yum -y install epel-release
yum -y install webkitgtk
**注：**
    这时候会发现没有webkitgtk包，因为centos7的相关软件源没有这个包，这时候可以：
    安装nux桌面相关软件包的一个专用软件源
    rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
    再执行前面的安装就会成功了；
    下面我们解压之前下载好的安装包
    yum install -y unzip;
    unzip pdi-ce-7.0.0.0-25.zip
    cd data-integration/
    赋予所有脚本执行权限：
    chmod +x -R *.sh;
linux下脚本的执行执行命令：
    trans 使用pan.sh 执行。
    job 使用 kitchen.sh执行。




