---
title: 公司产品项目Eclipse环境配置
tags:
---

**从代码库对应的分支下载相应代码：**

```
autopus-module-framework ---master
autopus-module-security  ---master
autopus-module-sysmgmt  ---v201905
autopus-pp-app  ---master
autopus-pp-core  ---master
autopus-pp-mstdata  ---master
```



**配置项目构建工具**

下载并安装Gradle 5.4及以上版本，解压并配置环境变量

```
C:\jarFile\gradle-5.4.1-all\gradle-5.4.1\bin
```

下载Ext构建工具Sencha 6.6及以上版本，安装并配置环境变量

```
C:\Users\Autopus\bin\Sencha\Cmd
```

**项目导入方式**

利用Eclipse的Git插件下载项目并将每个仓库上的代码设置成一个Working，收录到一个文件包下。

在windows下使用命令在pp-web-client的packages目录上复制开发人员需要做修改的项目。命令如下：

```
mklink /d "C:\eclipseWorkProject\autopus-pp-app\pp-web-client\packages\local\sysmgmt" "C:\eclipseWorkProject\autopus-module-sysmgmt\sysmgmt-ui\src\main\resources\package"

mklink /d "C:\eclipseWorkProject\autopus-pp-app\pp-web-client\packages\local\mstdata" "C:\eclipseWorkProject\autopus-pp-mstdata\mstdata-ui\src\main\resources\package"

mklink /d "C:\projectFile\productCode\autopus-pp-app\pp-web-client\packages\local\autopus-core" "C:\projectFile\productCode\autopus-UI-core\autopus-core\src\main\resources\package"
```

**修改项目配置文件**

在autopus-pp-app的gradle.properties文件中修改maven仓库连接和账户密码

```
moduleVersion=2.0-SNAPSHOT
autopusMavenUrl=http://repo.autopus.com.cn:19419/repository/
autopusViewUser=wuzw
autopusViewPassword=wu123
```

在autopus-pp-app的build.gradle中定义需要本地引入依赖的包名

```
// 定义最后需要写到app.json中的包依赖名
ext.webclientPackages = [
	'font-awesome',
    'sysmgmt',
    'mstdata'
]
```

在pp-web-client的build.gradle添加需要动态加载到pp-web-client的packages文件夹下项目

```
//"prjmgmt",
dependencies {
  api "com.autopus:autopus-core:2.0-SNAPSHOT"
  api "com.autopus:autopus-mainframe:2.0-SNAPSHOT"
}
```

然后在cmd窗口中使用 gradle cleaneclipse清理packages下的缓存，使用gradle eclipse 动态加载刚刚配置的项目到packages文件下。

然后在pp-web-client的autopusApp路径下执行Sencha编译命令

```
sencha app build development
```

**配置tomcat**

使用8.5及以上版本的tomcat，添加pp-web的web module，配置好端口，然后就可以启动tomcat查看环境是否正确配置。 

**配置tomcat 数据源 context.xml** 

**配置Nginx**

使用1.16版本的nginx ，在servers中配置apqp-app.conf中的监听端口与tomcat服务一致，配置server.inc中的location与本地项目一致。





???在autopus-module-sysmgmt的sysmgmt-common下config中找到spirng缓存配置文件，给每个缓存添加id。

