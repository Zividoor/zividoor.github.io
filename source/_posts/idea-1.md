---
title: Idea 工具的使用日志
date: 2019-05-06 10:25:49
tag:
---
## 提升编译速度方法
### 开启并行编译功能
    设置编译器
    在Compiler中勾选Compile independent modules in parallel并开启automatic和parallelized选项，打开并行编译功能。

### 增加Idea工具内存
    在bin目录下的vmoptions文件中修改内存分配。
    或者在Help中编辑Edit Custom VM Options。

### 查看Idea实时使用内存情况
    勾选Apparance的Show memory indicator。

## Git 的 smart checkout 和 force checkout 的区别
比如说我从Dev切换回开发分支时,要是dev某个文件跟开发分支冲突时,他就会弹出一个窗，说这部分文件冲突，问你要怎么处理
smart checkout就会把冲突的这部分内容带到开发分支（如果你没有点进窗口的那些文件处理冲突的话）
force checkout就不会把冲突的这部分内容带到开发分支

### Idea开启自动编译
进入设置setting，Build,Execut, Deployment -> Compiler 勾选右侧的Build Project automatically
