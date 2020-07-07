
|**0****前言**

我是从eclipse转IDEA的，对于习惯了eclipse快捷键的我来说，转IDEA开始很不习惯，IDEA快捷键多，组合多，记不住，虽然可以设置使用eclipse的快捷键，但是总感觉怪怪的。开始使用的时候自己也在网络上收集各种IDEA使用的教程，但是很多都不全，东说一点西说一点，因此我想在这里整理一份全而整的使用教程系列，不定时更新。

**2**|**0****官网下载**

一般都会去官网下载，官网地址[IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=windows)，官网上对于不同的操作系统（windows，macOS，Linux）都有两个版本可供下载

[![img](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205194610628-1721397153.png)](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205194610628-1721397153.png)

 

**Ultimate**即为旗舰版，功能全面，插件丰富，但是收费，按年收费。如果非要比较的话类似于myEclipse。

**Community**即为社区版，免费试用，功能相对而言不是很丰富，但是不影响开发使用。如果非要比较的话类似于eclipse。

如果有经济实力的话还是建议购买Ultimate版使用，但是不是终身的而是一年一付；但是网络上也有破解版的，各位相较而选。

**3**|**0****安装**

下载好了安装包，确认已经安装好了 JDK ，双击开始安装。

[![img](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200348222-2064563648.png)](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200348222-2064563648.png)

 选择安装路径

 [![img](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200527425-1461784901.png)](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200527425-1461784901.png)

1.根据你的操作系统类型选择，32位或64位的桌面快捷方式。

2.选择是否根据文件后缀名关联相应的文件，例如勾选了Java以后打开java文件默认是以IDEA打开，可以不勾选。

[![img](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200554003-1282727364.png)](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200554003-1282727364.png)

点击Install，就开始安装了

[![img](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200628863-1602142211.png)](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171205200628863-1602142211.png)

**4**|**0****目录说明**

[![img](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171206122455566-1188233198.png)](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171206122455566-1188233198.png)

- Bin：容器，执行文件和启动参数等。
- Help：快捷键文档和其他帮助文档
- Jre64:64 位 java 运行环境
- Lib：idea 依赖的类库
- License：各插件许可
- Plugin：插件

**5**|**0****破解方式**

**1. 注册码激活（已废弃）**

[![img](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171206100851706-229737583.png)](https://images2017.cnblogs.com/blog/1162587/201712/1162587-20171206100851706-229737583.png)

 但是这种破解是有有效期的，过了有效期又需要重新破解，下面介绍一个永久破解的方式 - 补丁破解。

**2. 破解补丁**

- 2017.2    版本补丁下载：<https://files.cnblogs.com/files/jajian/JetbrainsCrack-2.6.10-release-enc.jar.zip>
- 2018.3.1 版本补丁地址：<https://files.cnblogs.com/files/jajian/JetbrainsIdesCrack-3.4-release-enc.jar.zip>
- 2018.3.5 版本补丁地址：<https://files.cnblogs.com/files/jajian/JetbrainsIdesCrack-4.2-release.zip>

 

> **最新补丁下载**：
>
> 这里需要注意：不同的版本，补丁不同。因此这里不能列出所有的补丁文件，如果你是官网最新的 Idea 版本，关注公众号后回复“IDEA”可获取最新破解方法。

 

1. 先下载压缩包解压后得到 jetbrains-agent.jar，把它放到你认为合适的文件夹内。

2. 启动你的IDEA，如果上来就需要注册，选择：试用（`Evaluate for free`）进入IDEA，如果之前已经有过破解，请先 remove License。

3. 点击你要注册的IDEA菜单：`Configure` 或 `Help` -> `Edit Custom VM Options …`，如果提示是否要创建文件，请点"Yes"。

4. 在打开的vmoptions编辑窗口末行添加：`-javaagent:/absolute/path/to/jetbrains-agent.jar，**这里需要填写你自己的路径**`。

5. 重启你的IDE。

6. 点击IDE菜单 

   ```
   Help
   ```

    -> 

   ```
   Register…
   ```

    或 

   ```
   Configure
   ```

    -> 

   ```
   Manage License…
   ```

   ，支持两种注册方式：License server 和 Activation code。

   1).  选择License server方式，地址填入：http://fls.jetbrains-agent.com （只针对2019.3以后的版本）。
   2). 选择Activation code方式离线激活，请使用：ACTIVATION_CODE.txt 内的注册码激活。
   如果激活窗口一直弹出（error 1653219），请去hosts文件里“移除”jetbrains相关的项目，License key is in legacy format == Key invalid，表示agent配置未生效。

>  IDEA 2020.1以后的版本，直接把`jetbrains-agent-latest.zip`拖进IDE就可以破解了。

**注意事项：**

一定要自己确认好路径(不要使用中文路径)，填错会导致IDE打不开！！！最好使用绝对路径。一个vmoptions内只能有一个-javaagent参数。

*示例:*

```
1 mac: -javaagent:/Users/jetbrains/jetbrains-agent.jar
2 linux: -javaagent:/home/jetbrains/jetbrains-agent.jar 
3 windows: -javaagent:C:\Users\jetbrains\jetbrains-agent.jar
```

如果还是填错了，参考这篇文章编辑vmoptions补救：[intellij-support](https://intellij-support.jetbrains.com/hc/en-us/articles/206544519).

 

点击OK，破解完成，此时在Help -> About中可看到有效期至2089年结束，够用了吧。

[![img](https://img2020.cnblogs.com/i-beta/1162587/202003/1162587-20200304110536717-2092796239.png)](https://img2020.cnblogs.com/i-beta/1162587/202003/1162587-20200304110536717-2092796239.png)