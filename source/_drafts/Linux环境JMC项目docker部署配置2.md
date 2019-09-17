---
title: Linux环境JMC项目docker部署配置2
tags:
---

#### JMC项目Jmc项目docker部署配置

**下载所需环境安装包**

需要tomcat，jdk（这个环境可以使用docker镜像直接复制过来）

然后安装git 获取最新代码

centOS上安装git命令：yum install -y git

上传grails 镜像，运行grails镜像 （这里grials镜像包来自公司项目环境）

编写dockerfile文件如下：

```shell
FROM    grailsbuild:0.1 as grails-builder

ARG builder_tag=latest

MAINTAINER      Door

\# source code, just necessary ones
ARG     source=/opt/source

COPY    . ${source}/

RUN     whoami

RUN     ls -la ${source}

RUN     ls -l /opt/grails-2.3.11/bin/

RUN     chmod 777 /opt/grails-2.3.11/bin/grails

RUN     ls -l /opt/grails-2.3.11/bin/

\# 打包
RUN     cd $source/prj-jmc-pp-app && /opt/grails-2.3.11/bin/grails war

\# tomcat
FROM    tomcat:8.5-jre8-alpine

\# install envsubst,telnet,curl
RUN     apk add --no-cache gettext busybox-extras curl;\
                rm -rf webapps/*;

COPY    --from=grails-builder /opt/source/prj-jmc-pp-app/target/*.war webapps/ROOT.war

VOLUME  /usr/local/tomcat/logs

ENV     CATALINA_OPTS="-Xmx1000m -XX:+UseG1GC -Dfile.encoding=UTF-8 -Dapp.logbase=/logstore"

CMD     echo "Start webapp in docker" && \
        catalina.sh run
```

替换配置文件BuildConfig文件中的maven仓库地址

```shell
find . | grep BuildConfig.groovy | xargs perl -pi -e 's|mavenLocal(.)*./m2"\)|mavenLocal("/opt/source/m2")|g'
```

替换配置文件BuildConfig中的windows换行符

```shell
find . | grep BuildConfig.groovy | xargs sed -i 's/\r//'
```

创建docker镜像

```shell
 docker build -t jmctestdocker:0.2 .
```

启动docker容器 

```shell
docker run -p 8089:8080 -v volume-jmc-log:/logs -d jmctestdocker:0.2
```

