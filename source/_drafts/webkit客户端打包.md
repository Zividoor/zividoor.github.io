关于node-webkit 打包web程序的流程。



**新建文件夹myapp，并把index.html和package.json放在文件中**

**进入myapp文件夹（下面的压缩路径很重要），把这文件中中的所有文件压缩为一个app.zip文件，然后改名为app.nw**

**合并app.nw和nw.exe:**

　　**将app.nw文件移动到和nw.exe同级目录下，然后执行命令copy /b nw.exe+app.nw app.exe，这时是可以直接执行app.exe的，但换到其它目录就不可以执行了，因为换到其它目录找不到nwjs包内的依赖文件**



通过使用node-webkit 0.40.1 版本打包的程序，无法实现web跳转，初步判定为node-webkit程序bug



通过更换node-webkit 0.39.3 版本，可以正常打包web程序。



然后通过Enigma Virual Box 打包exe



最后使用Resource Hacker 和 Axialis IconWorkshop 修改程序图标。