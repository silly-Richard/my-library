---
{"dg-publish":true,"permalink":"/1.说明文档/python库 virtualenv/","created":"2024-04-28T21:21:43.845+08:00"}
---

# 介绍

在编写，打包python代码时，为了加快编译速度，提高编译专门化水平，可以在系统环境之外独立划分出虚拟环境以对应某一特定编译目标。virtualenv库就是可以创建虚拟环境的python基础库。

工作原理[^1]：把系统Python复制一份到virtualenv的环境，用命令激活activate进入一个virtualenv环境时，virtualenv会修改相关环境变量，让命令python和pip均指向当前的virtualenv环境。

# 指南

## 安装virtualenv

使用pip命令安装：`pip install virtualenv`  
使用清华源安装：`pip install virtualenv -i https://pypi.python.org/simple/`

## 创建虚拟环境

1.创建地址：选择一个文件夹存放虚拟环境。
2.创建虚拟环境：使用cmd（或者vscode终端），输入以下命令创建对应虚拟环境（其中ENV为虚拟环境名称，可替换）：

```cmd
virtualenv ENV 
%创建无第三方库的纯净python虚拟环境。此时环境中python版本为virtualenv库安装位置的python版本的备份，pip为安装位置的pip版本备份%

virtualenv -p python2.7 ENV
%如果系统中存在多个python版本，可以使用 -p 参数指定虚拟环境备份的python版本

virtualenv --system-site-packages env
%--system-site-packages参数表明虚拟库可以访问系统环境%
```

3.激活虚拟环境pip：若使用cmd，则cd至activate所在目录，并启动activate。若使用vscode，可将工作目录改为虚拟环境ENV，将核心kernel切换为虚拟环境python解释器。

```cmd
cd env\Scripts %activate存放在这个位置%

activate 
%激活activate，此时在cmd命令最前端会显示ENV字样，表明虚拟环境被激活%

pip --version 
%查看pip版本，以及指向的python编译核心地址，若地址为虚拟环境，则虚拟环境创建成功%

pip list %查看该虚拟环境下包安装情况%
```

## 退出虚拟环境

```cmd
deactivate
```


# 查表

```cmd
virtualenv [OPTIONS] DEST_DIR

% OPTIONS选项:
--version 显示当前版本号。
-h, --help 显示帮助信息。
-v, --verbose 显示详细信息。
-q, --quiet 不显示详细信息。
-p PYTHON_EXE 指定所用的python解析器的版本
比如 --python=python2.5 就使用2.5版本的解析器创建新的隔离环境。 
默认使用的是当前系统安装(/usr/bin/python)的python解析器

--clear  清空非root用户的安装，并重头开始创建隔离环境。
--no-site-packages  默认，令隔离环境不能访问系统全局的site-packages目录。
--system-site-packages  令隔离环境可以访问系统全局的site-packages目录。
```


[^1]: [virtualenv用法梳理 - 简书](https://www.jianshu.com/p/86cb6e2d77de)