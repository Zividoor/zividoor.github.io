---
title: Linux环境JMC项目部署
tags:


---

#### JMC项Linux部署流程

**下载所需环境安装包**

​	本例使用jdk1.7，tomcat 7，centOS7

**部署步骤（详细）**

使用ssh协议的scp指令传输文件到主机

​	示例： 

`scp -r C:/Users/Autopus/Downloads/Compressed/apache-tomcat-7.0.96-windows-x64 autopus@172.29.18.58:/home/autopus/Downloads`

配置centOS下的jdk环境

解压jdk到opt目录下

`tar -xzvf jdk压缩包 目标路径opt/java/jdk`

配置java环境变量 

在/etc/profile 文件末尾添加以下设置

`export JAVA_HOME=/opt/java
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH`

保存退出

然后使用

`source profile`

刷新环境变量

使用javac 检查jdk是否配置正确。

配置centOS下的tomcat

`tar -xzvf tomcat压缩包 目标路径opt/tomcat/tomcat7`

配置tomcat环境变量

进入tomcat的bin目录后通过vi命令打开catalina.sh文件，并在上方加入如下配置：

`JAVA_OPTS="-Xms512m -Xmx1024m -Xss1024K -XX:PermSize=512m -XX:MaxPermSize=1024m"
export TOMCAT_HOME=/home/autopus/Downloads/apache-tomcat-7.0.96/apache-tomcat-7.0.96
export CATALINA_HOME=/home/autopus/Downloads/apache-tomcat-7.0.96/apache-tomcat-7.0.96
export JRE_HOME=$JAVA_HOME/jre`

然后启动tomcat ，通过curl 访问8080端口来检测tomcat是否启动成功。

部署war包

将打包好的war包上传至服务器主机

移动到tomcat的webapps目录下，并命名为ROOT.war

同时删除其他tomcat默认的文件

启动tomcat 验证是否成功部署项目

使用startup启动不会打印控制台日志，会在后台运行，使用catalina 启动则会在当前终端打印日志，不会再后台运行。