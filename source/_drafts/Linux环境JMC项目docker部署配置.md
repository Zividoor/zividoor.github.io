---
title: Linux环境JMC项目docker部署配置
tags:



---

#### JMC项目Jmc项目docker部署配置

**下载所需环境安装包**

**这里的环境已经在linux环境JMC项目部署中已经配置完成。**

编写Dockerfile文件如下：

`FROM    tomcat:8.5-jre8-alpine

ARG builder_tag=latest

MAINTAINER      Door

\# install envsubst,telnet,curl

RUN     apk add --no-cache gettext busybox-extras curl;\

​                rm -rf webapps/*;\

​                mkdir /logstore /store;

COPY    ROOT.war webapps/ROOT.war

VOLUME  /usr/local/tomcat/logs /logstore /store

ENV     CATALINA_OPTS="-Xmx1000m -XX:+UseG1GC -Dfile.encoding=UTF-8 -Dapp.logbase=/logstore"

CMD     echo "Start webapp in mode: ${RUN_ENV}" && \

​        catalina.sh run`

这里需要注意的是 war 包要和Dockerfile在同一目录下。

创建docker镜像：

`docker build -t testjmcdocker:0.1 .`

使用docker images 查看镜像是否生成成功。

生成成功则会出现jmctestdocker 镜像

接着使用命令 docer run 镜像id 启动docker 

启动成功后可以使用docker ps 查看docker 启动状态

然后使用docker exec -it 容器id bash 进入docker容器中，执行curl localhost:8080 查看程序tomcat 是否成功运行。