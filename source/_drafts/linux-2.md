---
title: Linux无法链接问题
tags:
---

**解决办法**

1.检查是否安装了openssh-server

ps  -e|grep sshd



2.sshd未启动

systemctl restart sshd