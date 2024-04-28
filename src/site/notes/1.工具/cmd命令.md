---
{"dg-publish":true,"permalink":"/1.工具/cmd命令/","created":"2024-04-28T21:12:00.833+08:00"}
---

# 格式

**注释格式**[^1]
1.`::内容`，英文状态下的双冒号。
2.`%内容%`，使用百分号。

# 基础命令

```cmd
d:    进入d盘（直接输入，不要用cd）
dir    查看当前目录下所有文件
md 目录名    创建目录（包括文件和文件夹）
rd 目录名    删除目录
copy 路径\文件名 路径\文件名    将文件拷贝至另一个地址
move 路径\文件名 路径\文件名    将文件移动至另一个地址
del 文件    删除文件（专门删除文件）
```


# 查看信息命令

查看笔记本电池损耗情况
```cmd
powercfg/batteryreport
```

查看当前ip地址
```cmd
ipconfig
```

重置网络连接[^3]
```cmd
netsh winsock reset catalog
netsh int ip reset reset.log
```

# 启动程序命令

启动word[^2]
```cmd
start winword %启动word程序%
start winword "c:\Users\%UserName%\Desktop\123.docx" %启动具体路径下的word%
```

# wimc命令







[^1]: [Fetching Title#khw3](https://blog.csdn.net/weixin_42596182/article/details/93783388)
[^2]: [cmd打开word命令-掘金](https://juejin.cn/s/cmd%E6%89%93%E5%BC%80word%E5%91%BD%E4%BB%A4)
[^3]: [电脑能连上wifi，但显示无internet，要怎么解决？ - 知乎](https://www.zhihu.com/question/491519702)