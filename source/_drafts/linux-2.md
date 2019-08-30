---
title: Linux无法链接问题
tags:
---

**解决办法**

1.检查是否安装了openssh-server

ps-e|grep ssh1



2.sshd未启动

service sshd restart