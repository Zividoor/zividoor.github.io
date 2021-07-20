- CentOS7安装Docker
	1.卸载docker旧版本（我的centos7是新的，所以运行后不删除任何软件包）
```
   yum remove docker*
```
	2.安装yum-utils软件包
```
    yum install -y yum-utils
```
	3.设置Docker仓库（阿里云）
```
    yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
	4.更新yum软件包索引
```
    yum makecache fast
```
	5.安装最新版本的Docker
```
    yum install docker-ce docker-ce-cli containerd.io
```
	6.启动docker
```
    systemctl start docker
```
	7.查看docker版本
```
    docker version
```


#下载oracle镜像
docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g

#容器内授权 BEGIN （出现权限问题时处理，容器启动后执行）
#在容器中通过"id oracle"命令查看容器内 oracle 组合用户的 id
su root
chown -Rf oracle:oinstall /home/oracle/app/oracle/oradata/helowin
chmod -Rf 777 /home/oracle/app/oracle/oradata/helowin
chown -Rf oracle:oinstall /opt/oracle-dump-dir
chmod -Rf 777 /opt/oracle-dump-dir
#容器内授权 END

#目录创建及授权
mkdir -p /opt/docker/oracle/oracle-base-data-files
chown -R 500.500 /opt/docker/oracle/oracle-base-data-files

mkdir -p /opt/docker/database/srm-demo
chown -R 500.500 /opt/docker/database/srm-demo

mkdir -p /opt/docker/database/srm-demo/oracle-db-files
chown -R 500.500 /opt/docker/database/srm-demo/oracle-db-files

mkdir -p /opt/oracle-dump-dir
chown -R 500.500 /opt/oracle-dump-dir

#拷贝数据到本地，并修改拥有者 BEGIN （不用处理）
docker run -d --name oracle-test --privileged=true --restart=always -p 1521:1521 registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
docker cp oracle-test:/home/oracle/app/oracle/oradata/helowin/. /opt/docker/oracle/oracle-base-data-files/
cp -r /opt/docker/oracle/oracle-base-data-files/. /opt/docker/oracle/srm-demo
chown -R 500.500 /opt/docker/oracle/srm-demo  # 500 500 是容器内 oracle 组合用户的 id
#拷贝数据到本地，并修改拥有者 END

#启动容器（没有提取srm-demo前去掉srm-demo目录的挂载）
docker run -d --name srm-demo --privileged=true --restart=always -p 1521:1521 -e TZ=Asia/Shanghai -v /opt/docker/database/srm-demo/oracle-db-files:/home/oracle/app/oracle/oradata/helowin -v /opt/oracle-dump-dir:/opt/oracle-dump-dir registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g

#进入容器
docker exec -it srm-demo bash

#配置环境
cd /home/oracle                                           # 进入到 oracle 用户目录
source .bash_profile                                      # 加载 oracle 环境变量
$PATH                                                     # 查看 oracle 环境变量是否生效
sqlplus / as sysdba                                       # 连接 oracle 数据库
alter user system identified by abc123;                   # 修改 DBA 账号的密码
alter user sys identified by abc123;                      # 修改 DBA 账号的密码
alter profile default limit password_life_time unlimited; # 设置密码为永不过期

show parameter deferred_segment_creation;          -- 查看是否启用 true 为启动
alter system set deferred_segment_creation=false;  -- 修改为不启用
show parameter deferred_segment_creation;          -- 查看是否修改成功 false 未启用
select value from v$parameter where name = 'processes'; -- 查看最大连接数
alter system set processes = 1500 scope = spfile; -- 修改最大连接数
show parameter java_jit_enabled; 
alter system set java_jit_enabled=false;

#问题解决（直接迁移docker数据库文件时需要处理）
    docker logs -f srm-demo
    ORA-00214: control file '/home/oracle/app/oracle/oradata/helowin/control01.ctl'
    version 1302 inconsistent with file
    '/home/oracle/app/oracle/flash_recovery_area/helowin/control02.ctl' version 841

    #解决
    docker exec -it oracle-srm-demo bash
    cd /home/oracle              # 进入到 oracle 用户目录
    source .bash_profile         # 加载 oracle 环境变量（每次进入容器都要加载配置文件）
    
    # 删除新生成的版本控制文件，将数据卷中的版本控制文件复制为新生成的版本控制文件
    rm -rf /home/oracle/app/oracle/flash_recovery_area/helowin/control02.ctl
    cp /home/oracle/app/oracle/oradata/helowin/control01.ctl /home/oracle/app/oracle/flash_recovery_area/helowin/control02.ctl
    
    sqlplus / as sysdba          # 以 dba 身份连接 oracle 数据库
    shutdown immediate           # 关闭数据库实例（这里会报错，不用管）
    startup                      # 启动实例



# 开始导入数据（连上数据库，以下命令可以在数据库客户端软件中执行）

create directory DATA_DATA_DUMP_DIR as '/opt/oracle-dump-dir';

/* CSC_PLATFORM_DATA：创建数据表空间  */
create tablespace CSC_PLATFORM_DATA
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_PLATFORM_DATA.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;
/* CSC_PLATFORM_INDEX：创建数据表空间  */
create tablespace CSC_PLATFORM_INDEX
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_PLATFORM_INDEX.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;

/* CSC_SYSMGMT_DATA：创建数据表空间  */
create tablespace CSC_SYSMGMT_DATA
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_SYSMGMT_DATA.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;
/* CSC_SYSMGMT_INDEX：创建数据表空间  */
create tablespace CSC_SYSMGMT_INDEX
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_SYSMGMT_INDEX.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;

/* CSC_MSTDATA_DATA：创建数据表空间  */
create tablespace CSC_MSTDATA_DATA
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_MSTDATA_DATA.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;
/* CSC_MSTDATA_INDEX：创建数据表空间  */
create tablespace CSC_MSTDATA_INDEX
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_MSTDATA_INDEX.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;

/* CSC_CSTMGMT_DATA：创建数据表空间  */
CREATE TABLESPACE CSC_CSTMGMT_DATA
LOGGING
DATAFILE '/home/oracle/app/oracle/oradata/helowin//CSC_CSTMGMT_DATA.DBF'
SIZE 50M
AUTOEXTEND ON
NEXT 50M MAXSIZE 20480M
EXTENT MANAGEMENT LOCAL;
/* CSC_CSTMGMT_INDEX：创建数据表空间  */
CREATE TABLESPACE CSC_CSTMGMT_INDEX
LOGGING
DATAFILE '/home/oracle/app/oracle/oradata/helowin//CSC_CSTMGMT_INDEX.DBF'
SIZE 50M
AUTOEXTEND ON
NEXT 50M MAXSIZE 20480M
EXTENT MANAGEMENT LOCAL;

/* CSC_CONTRACT_DATA：创建数据表空间  */
create tablespace CSC_CONTRACT_DATA
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_CONTRACT_DATA.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;
/* CSC_CONTRACT_INDEX：创建数据表空间  */
create tablespace CSC_CONTRACT_INDEX
logging
datafile '/home/oracle/app/oracle/oradata/helowin//CSC_CONTRACT_INDEX.dbf'
size 50m
autoextend on
next 50m maxsize 20480m
extent management local;

create user CSC_PLATFORM identified by abc123
default tablespace CSC_PLATFORM_DATA;
grant connect,resource to CSC_PLATFORM;

create user CSC_SYSMGMT identified by abc123 default tablespace CSC_SYSMGMT_DATA;
grant connect,resource to CSC_SYSMGMT;

create user CSC_MSTDATA identified by abc123 default tablespace CSC_MSTDATA_DATA;
grant connect,resource to CSC_MSTDATA;

create user CSC_CSTMGMT identified by abc123 default tablespace CSC_CSTMGMT_DATA;
grant connect,resource to CSC_CSTMGMT;

create user CSC_CONTRACT identified by abc123 default tablespace CSC_CONTRACT_DATA;
grant connect,resource to CSC_CONTRACT;

grant read,write on directory DATA_DUMP_DIR to CSC_PLATFORM;
grant read,write on directory DATA_DUMP_DIR to CSC_SYSMGMT;
grant read,write on directory DATA_DUMP_DIR to CSC_MSTDATA;
grant read,write on directory DATA_DUMP_DIR to CSC_CSTMGMT;
grant read,write on directory DATA_DUMP_DIR to CSC_CONTRACT;

#导入前准备（在Docker容器中操作）
docker exec -it oracle-srm-demo bash
cd /opt/oracle-dump-dir
chown oracle:oinstall CSC_PLATFORM-202105131700-TEST.dmp
chown oracle:oinstall CSC_SYSMGMT-202105131700-TEST.dmp
chown oracle:oinstall CSC_MSTDATA-202105131700-TEST.dmp
chown oracle:oinstall CSC_CSTMGMT-202105131700-TEST.dmp
chown oracle:oinstall CSC_CONTRACT-202105131700-TEST.dmp

chmod 755 CSC_PLATFORM-202105131700-TEST.dmp
chmod 755 CSC_SYSMGMT-202105131700-TEST.dmp
chmod 755 CSC_MSTDATA-202105131700-TEST.dmp
chmod 755 CSC_CSTMGMT-202105131700-TEST.dmp
chmod 755 CSC_CONTRACT-202105131700-TEST.dmp

#导入数据文件
cd /home/oracle
source .bash_profile
impdp system/abc123 DIRECTORY=DATA_DUMP_DIR DUMPFILE=CSC_PLATFORM-202105131700-TEST.dmp logfile=CSC_PLATFORM-202105131700-TEST.log REMAP_SCHEMA=CSC_PLATFORM:CSC_PLATFORM table_exists_action=replace
impdp system/abc123 DIRECTORY=DATA_DUMP_DIR DUMPFILE=CSC_SYSMGMT-202105131700-TEST.dmp logfile=CSC_SYSMGMT-202105131700-TEST.log REMAP_SCHEMA=CSC_SYSMGMT:CSC_SYSMGMT table_exists_action=replace
impdp system/abc123 DIRECTORY=DATA_DUMP_DIR DUMPFILE=CSC_MSTDATA-202105131700-TEST.dmp logfile=CSC_MSTDATA-202105131700-TEST.log REMAP_SCHEMA=CSC_MSTDATA:CSC_MSTDATA table_exists_action=replace
impdp system/abc123 DIRECTORY=DATA_DUMP_DIR DUMPFILE=CSC_CSTMGMT-202105131700-TEST.dmp logfile=CSC_CSTMGMT-202105131700-TEST.log REMAP_SCHEMA=CSC_CSTMGMT:CSC_CSTMGMT table_exists_action=replace
impdp system/abc123 DIRECTORY=DATA_DUMP_DIR DUMPFILE=CSC_CONTRACT-202105131700-TEST.dmp logfile=CSC_CONTRACT-202105131700-TEST.log REMAP_SCHEMA=CSC_CONTRACT:CSC_CONTRACT table_exists_action=replace