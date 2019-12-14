---
title: Oracle备份与恢复
tags:
---

#### 基本语法和实例： 

​    1、**EXP**: 
​      有三种主要的方式（完全、用户、表） 
​      1、完全：           

```
EXP SYSTEM/MANAGER BUFFER=64000 FILE=C:\FULL.DMP FULL=Y 
```

​          如果要执行完全导出，必须具有特殊的权限 
​      2、用户模式： 

```
EXP SONIC/SONIC    BUFFER=64000 FILE=C:\SONIC.DMP OWNER=SONIC 
```

​          这样用户SONIC的所有对象被输出到文件中。 
​      3、表模式：

```
 EXP SONIC/SONIC    BUFFER=64000 FILE=C:\SONIC.DMP OWNER=SONIC TABLES=(SONIC) 
```

​          这样用户SONIC的表SONIC就被导出 
​    2、**IMP**: 
​      具有三种模式（完全、用户、表） 
​      1、完全： 

```
IMP SYSTEM/MANAGER BUFFER=64000 FILE=C:\FULL.DMP FULL=Y 
```

​      2、用户模式： 

```
 IMP SONIC/SONIC    BUFFER=64000 FILE=C:\SONIC.DMP FROMUSER=SONIC TOUSER=SONIC 
```

​          这样用户SONIC的所有对象被导入到文件中。必须指定FROMUSER、TOUSER参数，这样才能导入数据。 
​      3、表模式： 

```
EXP SONIC/SONIC    BUFFER=64000 FILE=C:\SONIC.DMP OWNER=SONIC TABLES=(SONIC) 
```

​          这样用户SONIC的表SONIC就被导入。

#### ORACLE数据库有两类备份方法：

第一类为物理备份，该方法实现数据库的完整恢复，但数据库必须运行在归挡模式下（业务数据库在非归挡模式下运行），且需要极大的外部存储设备，例如磁带库；第二类备份方式为逻辑备份，业务数据库采用此种方式，此方法不需要数据库运行在归挡模式下，不但备份简单，而且可以不需要外部存储设备。
　　
　　数据库逻辑备份方法
　　
　　ORACLE数据库的逻辑备份分为三种模式：表备份、用户备份和完全备份。
　　
　　表模式
　　
　　备份某个用户模式下指定的对象（表）。业务数据库通常采用这种备份方式。
　　
　　若备份到本地文件，使用如下命令：

```
exp icdmain/icd rows=y indexes=n compress=n buffer=65536
　　feedback=100000 volsize=0
　　file=exp_icdmain_csd_yyyymmdd.dmp
　　log=exp_icdmain_csd_yyyymmdd.log
　　tables=icdmain.commoninformation,icdmain.serviceinfo,icdmain.dealinfo
```


　　若直接备份到磁带设备，使用如下命令：

```
exp icdmain/icd rows=y indexes=n compress=n buffer=65536
　　feedback=100000 volsize=0
　　file=/dev/rmt0
　　log=exp_icdmain_csd_yyyymmdd.log
　　tables=icdmain.commoninformation,icdmain.serviceinfo,icdmain.dealinfo
```

　　注：在磁盘空间允许的情况下，应先备份到本地服务器，然后再拷贝到磁带。出于速度方面的考虑，尽量不要直接备份到磁带设备。
　　
　　用户模式
　　
　　备份某个用户模式下的所有对象。业务数据库通常采用这种备份方式。
　　若备份到本地文件，使用如下命令：

```
exp icdmain/icd owner=icdmain rows=y indexes=n compress=n buffer=65536
　　feedback=100000 volsize=0
　　file=exp_icdmain_yyyymmdd.dmp
　　log=exp_icdmain_yyyymmdd.log
```

　　若直接备份到磁带设备，使用如下命令：

```
　exp icdmain/icd owner=icdmain rows=y indexes=n compress=n buffer=65536
　　feedback=100000 volsize=0
　　file=/dev/rmt0
　　log=exp_icdmain_yyyymmdd.log
```

　　注：如果磁盘有空间，建议备份到磁盘，然后再拷贝到磁带。如果数据库数据量较小，可采用这种办法备份。

　　**以下为详细的导入导出实例：**

　　一、数据导出：

　　1、 将数据库TEST完全导出，用户名system 密码manager 导出到D：\daochu.dmp中

```
exp system/manager@TEST file=d：\daochu.dmp full=y
```

　　2、 将数据库中system用户与sys用户的表导出

```
exp system/manager@TEST file=d：\daochu.dmp owner=（system，sys）
```

　　3、 将数据库中的表table1 、table2导出

```
exp system/manager@TEST file=d：\daochu.dmp tables=（table1，table2）
```

　　4、 将数据库中的表table1中的字段filed1以"00"打头的数据导出

```
exp system/manager@TEST file=d：\daochu.dmp tables=（table1） query=\" where filed1 like '00%'\"
```

　　上面是常用的导出，对于压缩我不太在意，用winzip把dmp文件可以很好的压缩。

　　不过在上面命令后面 加上 compress=y  就可以了

 

　　二、数据的导入

 　 1、将D：\daochu.dmp 中的数据导入 TEST数据库中。

```
imp system/manager@TEST  file=d：\daochu.dmp
```

　　上面可能有点问题，因为有的表已经存在，然后它就报错，对该表就不进行导入。

　　在后面加上 ignore=y 就可以了。

　　2 将d：\daochu.dmp中的表table1 导入

```
imp system/manager@TEST  file=d：\daochu.dmp  tables=（table1）
```

　　基本上上面的导入导出够用了。不少情况我是将表彻底删除，然后导入。

　　注意：

　　你要有足够的权限，权限不够它会提示你。

　　数据库时可以连上的。可以用tnsping TEST 来获得数据库TEST能否连上。

#### 实际操作成功示例：

##### 用户模式备份示例：

```
exp 'sys/app@JMC as sysdba' file=E:\bak.dmp owner=(CORE,MSTDATA,BASEPM,CSTMGMT,BOMMGMT,BPMGMT,PPMGMT,PRDCTPM,INTEGRATION,JMCPP)
```

##### 用户模式导入示例：

```
imp 'sys/app@JMCRESTORE as sysdba' file=D:\bak.dmp FULL=Y
```

##### 数据导入问题解决：

###### 1.确认自己在哪个实例中

**使用如下的命令：**    

```
select instance_name from v$instance;
```

或者，使用：

```
show parameter instance;
```

进入指定的ORACLE_SID实例：

```
Sqlplus “/@dbsch as sysdba”
```

###### 2.中文字符集转换

```
KUP-11007: conversion error loading table "TEST"."T_PSR"
ORA-12899: 列 REASON_CODE 的值太大 (实际值: 21, 最大值: 20)
KUP-11009: data for row: REASON_CODE : 0X'BABDBFD5C6F7C8DDC1BFCFDED6C6'
```

这里涉及到了字符集转换的问题，中文在GBK字符集中占2位，但在UTF-8字符集中占3位，所以在GBK中保存小于20个字符的情况下，导入到了UTF-8的库中，就可能因为需要额外的字符空间导致超出字段长度定义，报了ORA-12899的错误。



###### 3. 主外键关联

```
ORA-31693: Table data object "TEST"."T_ITE" failed to load/unload and is being skipped due to error:
ORA-29913: error in executing ODCIEXTTABLEFETCH callout
ORA-02291: integrity constraint (TEST.FK_ITE_REF_PSR) violated - parent key not found
```

由于有些表之间是存在主外键关联的，expdp导出的时候选择了data_only仅导出数据，impdp导入的时候会因未插入主键记录而插入外键记录，出现ORA-02291的错误，对于这种情况可以选择先禁止主外键关联，导入后再恢复关联。

操作顺序：

(a) 导入前，执行如下SQL**找到需要禁止的外键关联**

```
select 'ALTER TABLE '||TABLE_NAME||' DISABLE CONSTRAINT '||constraint_name||';' 
from user_constraints WHERE CONSTRAINT_TYPE='R';
```

(b) 执行(a)的结果SQL

(c) 导入后，执行如下SQL**找到需要恢复的外键关联**

```
select 'ALTER TABLE '||TABLE_NAME||' ENABLE NOVALIDATE CONSTRAINT '||constraint_name||';' 
from user_constraints WHERE CONSTRAINT_TYPE='R';
```

NOVALIDATE参数不会验证已存储的数据，但未来再插入的记录则会遵循主外键关联的关系。

###### 4.用户创建与权限赋予

```
create user test identified by test;
grant connect,resource to test;
```

这里由于我使用了用户导出模式，所以需要通过用户来导入数据。

###### 4.成功导入后数据库中的视图都丢失了

原因是System用户下的pbcatcol表没有，所以是由于System对象不完整导致的（为什么System对象会不完整？），之后需要重新导出System用户，并重新导入。



**总结：**

1. 使用10g以上版本提供的expdp/impdp数据泵导入导出工具，较以往的exp/imp工具，无论是在参数的可选择性上，还是速度和压缩比上，都有了不小的改进，提供更为方便快速的数据导入导出方法给我们。
2. 导入导出可能碰到最多的问题，字符集转换算是其中之一，要明确导入导出数据对字符集的依赖程度，才能确保数据导入导出的正确。
3. 对于有主外键关联的数据，如果选择data_only仅导出数据，那么可在导入前禁止约束，这样导入过程不会受到主外键关联的影响，导入后可以恢复约束，保证约束的正确。
   

