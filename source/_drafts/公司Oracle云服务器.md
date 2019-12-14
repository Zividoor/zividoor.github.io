title: 公司Oracle云服务器信息
tags:

dockerhub镜像地址

https://hub.docker.com/r/oracleinanutshell/oracle-xe-11g

运行docker实例的命令

```shell
docker run -d -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true oracleinanutshell/oracle-xe-11g
```

ORACLE_ALLOW_REMOTE: 允许远程访问

挂载数据卷



Connect database with following setting:

hostname: localhost 

port: 49161 

sid: xe 

username: system 

password: oracle

Password for SYS & SYSTEM

oracle





Host autopus-jira
User deploy
Hostname jira.autopus.com.cn
Port 27023
ServerAliveInterval 30



私钥文件

 [id_rsa](公司Oracle云服务器\id_rsa) 



服务器jira:
root
aUt0pusrOOt



/u01/app/oracle



docker run -d -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true -v /opt/oracle/20190929:/u01/app/oracle --name dev_oracle_xe_11g oracleinanutshell/oracle-xe-11g



docker run -d -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true -v /opt/oracle/20190929/oradata:/u01/app/oracle/oradata -v /opt/oracle/20190929/product:/u01/app/oracle/product -v /opt/oracle/20190929/fast_recovery_area:/u01/app/oracle/fast_recovery_area -v /opt/oracle/20190929/admin:/u01/app/oracle/admin --name jmc_dev_oracle_xe_11g oracleinanutshell/oracle-xe-11g

