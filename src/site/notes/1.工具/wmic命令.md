---
{"dg-publish":true,"permalink":"/1.工具/wmic命令/","created":"2024-04-28T21:12:07.466+08:00"}
---


参考资料[^1]

# 介绍

wmic是扩展WMI（Windows Management Instrumentation，Windows管理规范），提供了从命令行接口和批命令脚本执行系统管理的支持。使用WMIC，我们不但可以管理本地计算机，而且还可以管理同一Windows域内的所有远程计算机（需要必要的权限），而被管理的远程计算机不必事先安装WMIC，只需要支持WMI即可。WMIC有一个能够分析、解释和执行从命令行接收的别名（Alias）的引擎，它是一个可执行文件，名为WMIC.exe，这个文件通常位于“c:\windows\system32\wbem”文件夹中。

# 使用方法

1.在cmd中输入`wmic`后出现`wmic:root\cli>`时，即可使用。

# 查看信息

查看内存条信息[^2]
```wimc
memorychip
```


[^1]: [windows比cmd更强大的 WMIC命令使用详解\_wmic memorychip list full命令信息解释-CSDN博客](https://blog.csdn.net/jhsword/article/details/96623333)
[^2]: [查询内存条信息的几种方法!-百度经验](https://jingyan.baidu.com/article/22a299b5c903a19e19376a2e.html)