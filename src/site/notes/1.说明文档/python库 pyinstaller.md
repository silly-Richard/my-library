---
{"dg-publish":true,"permalink":"/1.说明文档/python库 pyinstaller/","created":"2024-04-28T21:21:43.792+08:00"}
---

# 介绍

pyinstaller是一个将python源文件进行打包的第三方库，打包后生成的可执行程序可在没有python的环境中执行。

# 指南

## 安装

使用pip安装：`pip install pyinstaller`

## 指令

参考资料[^1]

```cmd
pyinstaller [参数] [py_file]

$参数:
-F 生成一个可执行文件，最常用的参数
-D 生成一个目录（包含多个文件）作为可执行文件
-W 运行exe时，不显示命令行窗口（仅对Windows有效）
-i [icon_filepath] 修改exe图标，该参数后跟icon图标路径
-n [new_name] 修改exe的名称，该参数后跟exe的新名称
$

pyinstaller -F file.py
pyinstaller -F -W file.py
pyinstaller -F -i abc.icon file.py
```


# 在虚拟环境中打包程序

1.首先应当在虚拟环境中安装pyinstaller库！
2.激活虚拟环境，运行pyinstaller，执行打包命令即可。



[^1]: [Python生成exe和安装包之Pyinstaller带参数【只看这篇就够了】\_pyinstaller -p参数-CSDN博客](https://blog.csdn.net/weixin_43804047/article/details/119704965)