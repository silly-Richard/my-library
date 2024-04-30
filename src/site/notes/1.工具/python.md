---
{"dg-publish":true,"permalink":"/1.工具/python/","created":"2024-04-28T20:32:07.632+08:00"}
---


# 指南
[list2tab]
- 基础
	- [[1.说明文档/python包管理 pip\|python包管理 pip]]
-  基础库
	- [[1.说明文档/python库 virtualenv\|virtualenv库]]
	- virtualenvwrapper库
	- [[1.说明文档/python库 pyinstaller\|pyinstaller库]]
	- [[1.说明文档/python库 itertools\|itertools库]]
- 第三方库
	- [[1.说明文档/python库 pyqt5\|pyqt5库]]
	- [[python库 cdsapi\|cdsapi库]]
	- [[1.说明文档/python库 sqlite3\|sqlite3库]]
	- [[1.说明文档/python库 matplotlib\|matplotlib库]]

# 备注信息

清华源地址：`pip install [package] -i https://pypi.python.org/simple/`
阿里云地址：`https://mirrors.aliyun.com/pypi/simple/`
中国科技大学：`https://pypi.mirrors.ustc.edu.cn/simple/`
豆瓣：`https://pypi.douban.com/simple/`


# 流程

## 数据分析

**数据下载**
- [[1.说明文档/使用python 下载cds ORAS5数据1958-2014\|使用python 下载cds ORAS5数据1958-2014]]

**读取nc文件**
- [[1.说明文档/使用python 读取ORAS5nc文件\|使用python 读取ORAS5nc文件]]——非均匀格网数据读取

**将nc文件转化为geotiff**
- [[1.说明文档/使用python osgeo库创建单波段geotiff文件\|使用python osgeo库创建单波段geotiff文件]]
	- SetGeoTransform参数使用？

**对已知坐标提取点值**
- [[1.说明文档/使用python  geopandas库对geotiff提取点值\|使用python  geopandas库对geotiff提取点值]]


## 读取数据库

- 读取sqlite3数据库：[[1.说明文档/python库 sqlite3\|python库 sqlite3]]

## 绘制图像

**绘制等值线图**
- [[1.说明文档/使用python matplotlib和cartory库绘制等值线图\|使用python matplotlib和cartory库绘制等值线图]]



## 创建虚拟环境
- 使用[[1.说明文档/python库 virtualenv\|virtualenv库]]或virtualenvwrapper库创建虚拟环境
- 使用[[1.说明文档/python库 pyinstaller#在虚拟环境中打包程序\|python库 pyinstaller#在虚拟环境中打包程序]]

## 图形化界面
- 使用[[1.说明文档/python库 pyqt5\|pyqt5库]]创建GUI
	- [[1.说明文档/python库 pyqt5#布局嵌套\|python库 pyqt5#布局嵌套]]
	- [[1.说明文档/python库 pyqt5#QBoxLayout 盒式布局\|python库 pyqt5#QBoxLayout 盒式布局]]
	- [[1.说明文档/python库 pyqt5#QGridLayout网格布局\|python库 pyqt5#QGridLayout网格布局]]
	- [[1.说明文档/python库 pyqt5#QFormLayout表单布局\|python库 pyqt5#QFormLayout表单布局]]
	- [[1.说明文档/python库 pyqt5#QStackedLayout抽屉布局\|python库 pyqt5#QStackedLayout抽屉布局]]


