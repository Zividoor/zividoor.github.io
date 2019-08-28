---
title: kettle 增量更新案例
date: 2019-04-26 09:45:52
tag:
---

### 增量更新问题分析

假定在源数据表中有一个字段会记录数据的新增或修改时间，可以通过它对数据在时间维度上进行排序。通过中间表记录每次更新的时间戳，在下一个同步周期时，通过这个时间戳同步该时间戳以后的增量数据。通过时间戳来完成增量同步。

### 作业流程
* 开始组件
* 建时间戳中间表
* 获取中间表的时间戳，并设置为全局变量
* 删除目标表中时间戳及时间戳以后的数据
* 抽取两个数据表的时间戳及时间戳以后的数据进行转换操作，并根据比对结果进行删除、新增或修改操作
* 更新时间戳

### 创建作业
最终的作业截图如下（如何创建作业和使用组件这里不重复赘述）：
![info](1.png)
其中获取时间戳的trans截图如下：
![info](2.png)
增量更新数据trans截图如下：
![info](3.png)

#### 创建作业和DB连接
    打开Spoon工具，新建作业，然后在左侧主对象树DB连接中新建DB连接。创建连接并测试通过后可以在左侧DB连接下右键共享出来。因为在单个作业或者转换中新建的DB连接都是局域数据源，在其他转换和作业中是不能使用的，即使属于同一个作业下的不同转换，所以需要把他们共享，这样DB连接就会成为全局数据源，不用多次编辑。

#### 建时间戳中间表
    这一步是为了在目标数据库建中间表etl_temp,并插入初始的时间戳字段。
本例中目标库使用postgresql数据库的创建中间表和初始化数据sql语句如下：
{% codeblock %}
CREATE TABLE IF NOT EXISTS etl_temp(id int primary key,time_stamp timestamp);
INSERT INTO etl_temp (id,time_stamp) VALUES (1, to_timestamp('2016-04-25 15:04:28 605', 'YYYY-MM-DD HH24:MI:SS MS')) ON conflict(id) DO UPDATE SET time_stamp = to_timestamp('2016-04-25 15:04:28 605', 'YYYY-MM-DD HH24:MI:SS MS');
{% endcodeblock %}

#### 获取时间戳并设为变量
SQL代码和组件配置截图如下
获取上次更新时间
{% codeblock %}
select to_char(time_stamp, 'YYYY-MM-DD HH24:MI:SS MS') time_stamp from etl_temp where id=1 ;
{% endcodeblock %}
获取当前服务器时间
{% codeblock %}
select to_char(current_timestamp, 'YYYY-MM-DD HH24:MI:SS MS') as c_time;
{% endcodeblock %}
![info](4.png)

#### 设置变量
变量活动类型可以为该变量设置四种有效活动范围，分别是JVM、该Job、父Job和祖父Job
![info](5.png)

#### 删除目标表中时间戳及时间戳以后的数据
这样做有两个好处：

**避免在同步中重复或者遗漏数据。**
    例如当时间戳在源数据表中不是唯一的，上一次同步周期最后一条数据的时间戳是2018-05-25 18:12:12,那么上一次同步周期结束后中间表中的时间戳就会更新为2018-05-25 18:12:12。如果在下一个同步周期时源数据表中仍然有时间戳为2018-05-25 18:12:12的新数据，那么同步就会出现数据不一致。采用大于时间戳的方式同步就会遗漏数据，采用等于时间戳的方式同步就会重复同步数据。
**增加健壮性**
    当作业异常结束后，不用做任何多余的操作就可以重启。因为会删除目标表中时间戳及时间戳以后的数据，所以不用担心数据一致性问题

#### 抽取、转换和更新数据
这一步才是真正的数据同步步骤，完成了数据的抽取、比对，并根据不同的比对结果删除、更新、插入或不做任何操作。设置如下图：
![info](6.png)
在组件中使用了上一步骤设置的变量，所以必须勾选使用变量替换
sql代码如下：
{% codeblock %}
SELECT id, major_version, minor_version, programme, status, "_tenant_id", release_date FROM autopus_1.cm_cost_model where "_is_deleted" = false and ("_create_date" > to_timestamp('${TIME_STAMP}', 'YYYY-MM-DD HH24:MI:SS MS') or "_update_date"  > to_timestamp('${TIME_STAMP}', 'YYYY-MM-DD HH24:MI:SS MS'));
{% endcodeblock %}

然后通过插入/更新更新数据到目标库。
更新成功后需要更新时间戳，为下次执行脚本时查询增量数据做准备。
下面是更新时间戳的sql：
{% codeblock %}
UPDATE public.etl_temp
SET time_stamp=to_timestamp('${C_TIME}', 'YYYY-MM-DD HH24:MI:SS MS')
WHERE id=1;
{% endcodeblock %}

### 发送邮件
关于发送邮件组件网上有很多资料，就不多做介绍。特别强调一点，邮箱密码是 单独的授权码，而不是邮箱登录密码。



