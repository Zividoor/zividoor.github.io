编辑pg_hba.conf配置文件：

vim /var/lib/pgsql/9.6/data/pg_hba.conf
在里面添加：

host    all             all             0.0.0.0/0               md5
重启服务器：

systemctl restart postgresql-11

