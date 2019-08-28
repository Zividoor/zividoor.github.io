问题1：

在执行Excel转PDF时错误信息：no jacob-1.18-M2-x64 in java.library.path

是由于无法加载jacob-1.18-M2-x64.dll 导致的。

解决方案： 

​	在jdk的jre/bin中添加jacob-1.18-M2-x64.dll应用程序拓展文件。



问题2： 

 com.jacob.com.ComFailException: Invoke of: Open
Source: Microsoft Office Excel
Description: Microsoft Office Excel 不能访问文件“C:\Windows\TEMP\1161938112034518682.xls”。 可能的原因有:

• 文件名称或路径不存在。
• 文件正被其他程序使用。
• 您正要保存的工作簿与当前打开的工作簿同名。



解决方案：

​	<https://blog.csdn.net/smeyou/article/details/7754392>

1.
 1).通过webconfig中增加模拟,加入管理员权限，
 <identity impersonate="true" userName="系统管理员" password="系统管理员密码"/>
 2).这样就能够启动Application进程，操作EXCEL了，能够新建EXCEL，导出EXCEL,但是还是不能打开服务器端的EXCEL文件

2.  
     在组件服务，DOCM设置 Microsoft Excel Application的属性，
     因为是在64位系统上面操作，组件服务中DOCOM中默认是没有的，因为Microsoft Excel Application是32的DCOM配置，所以通过如下方式解决（参考第三步)

3.
   1).开始--〉运行--〉cmd
   2)命令提示符下面，输入mmc -32,打开32的控制台
   3).文件菜单中，添加删除管理单元--〉组件服务
   4).在"DCOM配置"中找到"Microsoft Excel 应用程序",在它上面点击右键,然后点击"属性",弹出"Microsoft Excel 应用程序属性"对话框
　5).点击"标识"标签,选择"交互式用户"
　6).点击"安全"标签,在"启动和激活权限"上点击"自定义",然后点击对应的"编辑"按钮,在弹出的"安全性"对话框中填加一个"NETWORK SERVICE"用户(注意要选择本计算机名),并给它赋予"本地启动"和"本地激活"权限
   7).依然是"安全"标签,在"访问权限"上点击"自定义",然后点击"编辑",在弹出的"安全性"对话框中也填加一个"NETWORK SERVICE"用户,然后赋予"本地访问"权限.
4.重新启动IIS，测试通过



如果是iis7+win2008 R2 则在组件服务中的 EXCEL Application 修改以上属性即可。
