首先是在/usr/share/applications下创建eclipse.desktop文件

1. 创建并编辑eclipse.desktop

sudo vim /usr/share/applications/eclipse.desktop

2. 在里面编辑如下

    [Desktop Entry]
    Name=eclipse
    Exec=U eclipse Path/eclipse/eclipse (Eclipse路径)
    Icon=U eclipse Path//eclipse/eclipse.png (Eclipse图标路径)
    Terminal=false
    Type=Application
    Categories=Development

提示：

Exec=U eclipse Path/eclipse/eclipse
Icon=U eclipse Path//eclipse/eclipse.png
这两项是需要将你本地Eclipse目录进行填充的

 

Esc->:wq保存退出，然后你就可以在Dash主页搜索eclipse，是不是就可以看到一个Eclipse的图标？点击执行，Eclipse应用被打开，同时Eclipse的图标也出现在了桌面左侧的启动栏上。
我们可以将Eclipse图标锁定在侧边栏上，避免每次执行Eclipse时都必须再到Dash主页进行搜索。当Eclipse应用执行时，鼠标右键单击侧边栏上的Eclipse图标，在弹出的菜单里面选择“锁定到启动器”，这样就可以锁定了。