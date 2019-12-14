# [Oracle 数据库密码过期问题](https://www.cnblogs.com/huangwentian/p/8243310.html)



```
`（1）在CMD命令窗口中输入：` `        ``sqlplus 用户名/密码@数据库本地服务名 ``as` `sysdba;（如：sqlplus scott/1234@oracle1 ``as` `sysdba;）`
```



```
`（2）查看用户的proifle是哪个，一般是``default` `：` `　　 sql>SELECT username,PROFILE FROM dba_users;`
```



```
`（3）查看对应的概要文件(如``default``)的密码有效期设置：` `　　 sql>SELECT * FROM dba_profiles s WHERE s.profile=``'DEFAULT'` `AND resource_name=``'PASSWORD_LIFE_TIME'``;`
```



```
`（4）将概要文件(如``default``)的密码有效期由默认的180天修改成“无限制”：` `         ``sql>ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;` `修改之后不需要重启动数据库，会立即生效。`
```



```
（5）修改后，还没有被提示ORA-28002警告的用户账号不会再碰到同样的提示;而已经被提示的用户账号必须再改一次密码，举例如下：
 
　　 $sqlplus / as sysdba
 
　　 sql>alter user 用户名 identified by <原来的密码> account unlock; ----不用换新密码
       样例： alter  sys as sysdba identified by abcABC;
```

```
注意：oracle11g启动参数resource_limit无论设置为``false``还是``true``，密码有效期都是生效的，所以必须通过以上方式进行修改。
```

执行完后，发现还是无法连接数据库，这时候需要

conn test; //test 是数据库名

它会提示输入口令 输入原来的口令就好。

输入后，它提示更改口令，就把原来的口令输入就可以了；



