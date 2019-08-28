---
title: kettle分布式集群搭建
date: 2019-04-23 18:55:27
tag:
---


## 集群环境设置

### ip地址映射（在主机和从机中均做此映射）

![info](kitlle1.jpg)

### kettle 配置
之后我们进入之前安装好的kettle目录下，找到配置文件 carte-config-master-8080.xml。
![info](kettle1.png)
注意红框部分的配置
![info](kettle2.jpg)

找到 carte-config-8081.xml 文件做如下配置
{% codeblock %}
  <masters>

    <slaveserver>
      <name>master1</name>
      <hostname>192.168.213.128</hostname>
      <port>8080</port>
      <username>cluster</username>
      <password>cluster</password>
      <master>Y</master>
    </slaveserver>

  </masters>

  <report_to_masters>Y</report_to_masters>

  <slaveserver>
    <name>slave1-8081</name>
    <hostname>192.168.213.129</hostname>
    <port>8081</port>
    <username>cluster</username>
    <password>cluster</password>
    <master>N</master>
  </slaveserver>

{% endcodeblock %}

找到 carte-config-8082.xml 文件做如下配置
{% codeblock %}
  <masters>

    <slaveserver>
      <name>master1</name>
      <hostname>192.168.213.128</hostname>
      <port>8080</port>
      <username>cluster</username>
      <password>cluster</password>
      <master>Y</master>
    </slaveserver>

  </masters>

  <report_to_masters>Y</report_to_masters>

  <slaveserver>
    <name>slave2-8082</name>
    <hostname>192.168.213.127</hostname>
    <port>8082</port>
    <username>cluster</username>
    <password>cluster</password>
    <master>N</master>
  </slaveserver>


{% endcodeblock %}

这里我们因为是配置3节点集群，所以就配置3个就好了，同样的配置我们需要在另外两个服务器上配置。

在主节点上启动服务
![info](kettle3.png)
启动成功后会出现如下结果：
![info](kettle4.png)

在浏览器打开http://192.168.213.128:8080/ 地址，其中账号密码由于没有配置，所以都是默认的cluster
![info](kettle5.jpg)
可以查看状态master 是否启动成功。

同样在其他从节点也需要启动服务
![info](kettle6.png)
在浏览器打开http://192.168.213.129:8081/ 地址，可以查看状态server1 是否启动成功
![info](kettle7.jpg)
在浏览器打开http://192.168.213.127:8082/ 地址，可以查看状态server2 是否启动成功