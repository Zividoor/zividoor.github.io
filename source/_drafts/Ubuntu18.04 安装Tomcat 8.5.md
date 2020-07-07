环境信息：
OS：Ubuntu18.04
JDK：1.8
Tomcat: 8.5.31

1.下载Tomcat 8.5.31
到Apache Tomcat®官网，选择tar.gz包下载，点击跳转：



2.安装配置
2.1 把tomcat放到你想要方的位置

sudo cp apache-tomcat-8.5.31.tar.gz /usr/local/

图示：

2.2 解压

sudo tar -zxvf apache-tomcat-8.5.31.tar.gz

图示：

2.3 赋权限

sudo chmod 755 -R apache-tomcat-8.5.31

图示：

2.4 修改启动脚本
进入tomcat的bin目录下：

sudo vi startup.sh

在最后一行之前加入如下信息（注意根据自己实际情况修改JAVA_HOME和TOMCAT_HOME）：

#set java environment
export JAVA_HOME=/usr/local/jdk1.8
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#tomcat
export TOMCAT_HOME=/usr/local/apache-tomcat-8.5.31

图示：


3.启动服务
执行startup.sh，提示Tomcat started就是服务启动正常了

sudo ./startup.sh



浏览器中访问localhost:8080验证：

————————————————
版权声明：本文为CSDN博主「Weison Wei」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixx3/java/article/details/80808484