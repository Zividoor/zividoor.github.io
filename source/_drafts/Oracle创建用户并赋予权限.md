---
title: Oracle创建用户和赋予权限
tags:
---

##### 1、创建用户test：

（1）首先以sysdba（系统管理员）身份登录：例如：用户名（sys）,口令（orcl as sysdba）
（2）输入命令（用户名为test）：

```
create user test identified by test;
```



##### 2、修改密码：

**命令：**

```
alter user test identified by 123456;
```



##### 3、创建表空间：

**命令：**

```
create tablespace test_test datafile ‘E:\Oracle\wyj.dbf’ size 200M;
```

（表空间的名为test_test，wyj.dbf文件直接生成）

##### 4、将创建好的表空间分配给用户：

**命令：**

alter user test default tablespace test_test ;

##### 5、给用户分配权限：

```
grant create session,create table,create view,create
sequence,unlimited tablespace to test;

grant connect,resource to test;
```

表示把 connect,resource权限授予test用户

```
grant dba to test;
```

表示把 dba权限授予给 test

系统权限分类：

DBA: 拥有全部特权，是系统最高权限，只有DBA才可以创建数据库结构；

RESOURCE:拥有Resource权限的用户只可以创建实体，不可以创建数据库结构；

CONNECT:拥有Connect权限的用户只可以登录Oracle，不可以创建实体，不可以创建数据库结构。。。

对于普通用户：授予connect, resource权限；
对于DBA管理用户：授予connect，resource, dba权限。。。

系统权限授权命令：
系统权限只能由DBA用户授出：sys, system(最开始只能是这两个用户)
授权命令：

```
SQL> grant connect, resource, dba to 用户名1 [,用户名2]…;
```

注:普通用户通过授权可以具有与system相同的用户权限，但永远不能达到与sys用户相同的权限，system用户的权限也可以被回收