---
title: SSH使用私钥链接linux服务器
tags:

---

**使用方式**

1，获取**私钥**和服务器**配置**信息

配置示例如下：

```
Host autopus-jira
User deploy
Hostname jira.autopus.com.cn
Port 27023
ServerAliveInterval 30
IdentityFile C:\Users\Autopus\.ssh\id_rsa_dev_oracle
```

2，在用户目录下创建**.ssh**文件夹，添加id-rsa私钥文件和配置文件config

3，使用ssh autopus-jira命令即可免密连接服务器