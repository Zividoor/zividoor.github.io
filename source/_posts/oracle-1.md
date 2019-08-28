---
title: Oracle 使用笔记
date: 2019-05-09 14:41:41
tag:
---

### oracle中char,varchar,varchar2的区别
区别： 
     1． CHAR的长度是固定的，而VARCHAR2的长度是可以变化的， 比如，存储字符串“abc"，对于CHAR (20)，表示你存储的字符将占20个字节(包括17个空字符)，而同样的VARCHAR2 (20)则只占用3个字节的长度，20只是最大值，当你存储的字符小于20时，按实际长度存储。 
     2．CHAR的效率比VARCHAR2的效率稍高。 
     3．目前VARCHAR是VARCHAR2的同义词。工业标准的VARCHAR类型可以存储空字符串，但是oracle不这样做，尽管它保留以后这样做的 权利。Oracle自己开发了一个数据类型VARCHAR2，这个类型不是一个标准的VARCHAR，它将在数据库中varchar列可以存储空字符串的 特性改为存储NULL值。如果你想有向后兼容的能力，Oracle建议使用VARCHAR2而不是VARCHAR。

何时该用CHAR，何时该用varchar2？ 
    CHAR与VARCHAR2是一对矛盾的统一体，两者是互补的关系. 
    VARCHAR2比CHAR节省空间，在效率上比CHAR会稍微差一些，即要想获得效率，就必须牺牲一定的空间，这也就是我们在数据库设计上常说的‘以空间换效率’。 
    VARCHAR2 虽然比CHAR节省空间，但是如果一个VARCHAR2列经常被修改，而且每次被修改的数据的长度不同，这会引起‘行迁移’(Row Migration)现象，而这造成多余的I/O，是数据库设计和调整中要尽力避免的，在这种情况下用CHAR代替VARCHAR2会更好一些。

    char中还会自动补齐空格，因为你insert到一个char字段自动补充了空格的,但是select 后空格没有删除。

### 新增表字段
{%codeblock%}
comment  on  column  表名.字段名   is  '注释内容';
{%endcodeblock%}

### 表或字段添加注释
{%codeblock%}
comment on table 表名  is  '注释内容';
{%endcodeblock%}

### 创建索引
{%codeblock%}
CREATE INDEX 索引名 ON 表名 (列名) TABLESPACE 表空间名;
{%endcodeblock%}

### 数据库备份
本地的导出与导入:
    备份(也叫导出):

{%codeblock%}
        exp用户名/密码@本地服务名  file = 目标地址
{%endcodeblock%}

    (注:导出的文件是在硬盘上生成后缀名为dmp的文件)

    还原【导入】:

{%codeblock%}
        imp 用户名/密码@本地服务名 file=文件的位置  ignore=y
{%endcodeblock%}

(注:ignore=y的作用是忽视一些不必要的错误,当然,不加也可以,但是小编向来都是那种眼不见心不乱的人,所以向来都是加上的)

远程的导出与导入:


    备份【导出】：

{%codeblock%}
        exp 用户名/密码@网络服务名 file=目标地址
{%endcodeblock%}

    还原【导入】:

{%codeblock%}
        imp 用户名/密码@网络服务名 file=文件位置  ignore=y
{%endcodeblock%}

(注意：如果从A用户导出，然后导入B用户，则需要加上 fromuser=A touser=B)




