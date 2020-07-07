先决条件：已安装Java JDK orJRE且版本为7或者以上，可通过命令$:java -version查看JDK版本。



一、下载

下载地址https://gradle.org/releases

Gradle Distribution地址http://services.gradle.org/distributions



二、解压

将安装包解压到选定的安装目录

sudo mkdir /opt/gradle
sudo unzip -d /opt/gradle gradle-3.4.1-all.zip




三、环境变量配置

sudo vim /etc/profile  #打开配置文件
export GRADLE_HOME=/opt/gradle/gradle-3.4.1  #添加的内容
export PATH=$GRADLE_HOME/bin:$PATH   #添加的内容
保存完成后运行命令 source /etc/profile 使环境变量生效



四、验证

gradle -v

————————————————
版权声明：本文为CSDN博主「yuzhuzhong」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/yuzhuzhong/java/article/details/60954901