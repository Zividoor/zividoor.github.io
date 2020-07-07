先查看当前系统时间

root@ubuntu:/# date -R   
结果时区是：-0500
我需要的是东八区，这儿显示不是，所以需要设置一个时区

1.运行tzselect

root@ubuntu:/# tzselect


在这里我们选择亚洲 Asia，确认之后选择中国（China)，最后选择北京(Beijing)



2.复制文件到/etc目录下

root@ubuntu:/# cp /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
3.再次查看时间date -R，已经修改为北京时间
————————————————
版权声明：本文为CSDN博主「zhengchaooo」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zhengchaooo/java/article/details/79500032