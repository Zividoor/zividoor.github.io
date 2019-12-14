---
title: 无法为表空间中的段创建INITIAL区
tags:

---

##### 1、**查看表空间使用率情况**

可以看到对应的数据库表空间已经满了。

```sql
SELECT a.tablespace_name "表空间名",
total/1024/1024  "表空间大小单位M",
free/1024/1024 "表空间剩余大小单位M",
(total - free)/1024/1024 "表空间使用大小单位M",
Round((total - free) / total, 4) * 100 "使用率   [[%]]"FROM 
(SELECT tablespace_name,Sum(bytes) free FROM DBA_FREE_SPACE GROUP BY tablespace_name) a,
(SELECT tablespace_name,Sum(bytes) total FROM DBA_DATA_FILES GROUP BY tablespace_name) b WHERE a.tablespace_name = b.tablespace_name;
```



##### 2、**查看表空间是否自动扩容**

对应无法扩容的表空间没有自动扩容

```sql
select tablespace_name,file_name,autoextensible from dba_data_files;
```



##### 3、**查看表空间文件已使用情况**

对应的表空间数据文件已名称，避免重复



##### 4、**使用sysdba用户登录数据库**

```shell
su - oracle
sqlplus "/as sysdba"
```



##### 5、**为对应的表空间增加物理文件扩展空间**

```sql
alter tablespace HS_HIS_DATA add datafile '/u01/app/oracle/oradata/orcl/hisdat3.dbf' size 500M AUTOEXTEND on next 100m;
```

表空间扩容后，问题得到解决