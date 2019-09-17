---
title: SSH远程连接很慢
tags:


---

**ssh远程连接很慢**

正常情况下默认配置下 sshd 初次接受 ssh 客户端连接的时候会自动反向解析客户端 IP 以得到 ssh 客户端的域名或主机名。如果这个时候 DNS 的反向解析不正确，sshd 就会等到 DNS 解析超时后才提供 ssh 连接，这样就造成连接时间过长、ssh 客户端等待的情况，一般为10-30秒左右。有个简单的解决办法就是在 sshd 的配置文件（sshd_config）里取消 sshd 的反向 DNS 解析。

```
# vi /etc/ssh/sshd_config 
UseDNS no  #修改为no
# service sshd restart
```

