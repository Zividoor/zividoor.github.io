# （一）nginx的安装

　　1、执行以下安装命令

```
sudo apt-get install nginx
```

　　2、安装完成，查看版本来检测是否安装成功。

```
sudo apt-get install nginx
```

# （二）nginx的使用

1. 切换到nginx 的配置文件夹目录下

   ```
   cd /etc/nginx/conf.d
   ```

2. 这里我们需要添加对应网站的配置文件。这里给一个常用的命名规则：项目名+二级域名+端口.conf .使用touch命令创建。

3. 开始编辑我们的conf文件 。vim ice-qjnubk-3000.conf  ,复制以下代码进去

4. 保存退出，按esc +wq！ enter 。

5. 重启nginx服务器

   ```
   service nginx restart
   ```

6. 这个时候我们的nginx配置基本完成，但是我们的域名还没有设置解析。进入到自己域名的控制台，添加A主机记录，并指明自己的服务器ip地址。到这一步如果你的页面访问正常，则显示我们之前Pm2运行的node。js 项目。内容helloword 。如果出现502 BadgateWay 检查自己的pm2运行状态。