ssh-server与ssh-agent
Ubuntu 桌面版默认没有安装 ssh-server

Ubuntu server版默认没有安装 ssh-client

在集群搭建中，需要集群中全部机器都具有两种服务进程

使用命令 ps -e | grep ssh 可以查看ssh服务的运行情况，显示效果分别为：

wj@ubuntu:~/apps/spark-2.2.0-bin-hadoop2.7/sbin$ ps -e | grep ssh
   873 ?        00:00:00 sshd                                     对应服务器端
  1291 ?        00:00:00 ssh-agent                                对应客户端
  2124 ?        00:00:00 ssh-agent
1
2
3
4
ssh-agent表示ssh-client启动了

sshd表示ssh-server启动了

如果缺少：（一般而言，初装的ubuntu桌面版都缺少ssh-server，有ssh-client，而ubuntu server版都缺少ssh-client，有ssh-server）

安装ssh-client的命令：apt-get install openssh-client

安装ssh-server的命令：apt-get install openssh-server

在ubuntu桌面版中，安装完成之后，使用命令：

service sshd restart
1
即可启动sshd服务。
