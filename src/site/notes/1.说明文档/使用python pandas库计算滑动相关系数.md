---
{"dg-publish":true,"permalink":"/1.说明文档/使用python pandas库计算滑动相关系数/","created":"2024-04-28T21:21:43.919+08:00"}
---

# 介绍

此脚本的设计目的是计算多个指标，两两之间，以年代为标尺的滑动相关系数。

计算依据：使用pandas的`rolling.corr()`函数，计算指定两个指标对象，指定区间的滑动相关系数。

# 基础版本


脚本如下：
```python
import pandas as pd
import itertools

def calculate_rcc(df,i,m,window,center):
    '''
    两两计算多个指标之间的滑动相关系数的函数\n
    步长为1不可修改\n
    df:存放计算结果的空数据框\n
    i:计算指标1的列名,str\n
    m:计算指标2的列名,str\n
    window:滑动区间,必须是正整数,int\n
    center:计算结果是位于窗口中心还是右区间,bool\n
    '''
    rcc = data[i].rolling(window=window,center=center).corr(data[m])
    col_name = i+'/'+m
    df[col_name] = rcc
    print(col_name+" is finished")


# 输入
data = pd.read_excel("插值结果.xlsx",index_col=0)

# 预处理
## 使用itertools.combinations()函数将指标名称两两配对 ##
## 配对后的指标名称组合为元组格式，作为ele_combin列表的元素 ##
ele_combin = list(itertools.combinations(data.columns,2))

rcc_df = pd.DataFrame()

for ele in ele_combin:
    i = ele[0] # 取出元组中指标名称
    m = ele[1]
    calculate_rcc(rcc_df,i,m,window=1000,center=False)

rcc_df = rcc_df.dropna() # 去除计算结果中的na
rcc_df.to_excel("rcc计算结果.xlsx")

```

# 修饰版本

**增加内容**
- 添加了浏览本地文件导入功能。


```python
import pandas as pd
import itertools
from tkinter import Tk
from tkinter import filedialog
import os

def calculate_rcc(df,i,m,window,center):
    '''
    两两计算多个指标之间的滑动相关系数的函数\n
    步长为1不可修改\n
    df:存放计算结果的空数据框\n
    i:计算指标1的列名,str\n
    m:计算指标2的列名,str\n
    window:滑动区间,必须是正整数,int\n
    center:计算结果是位于窗口中心还是右区间,bool\n
    '''
    rcc = data[i].rolling(window=window,center=center).corr(data[m])
    col_name = i+'/'+m
    df[col_name] = rcc
    print(col_name+" is finished")


def openfile():
	'''
	选择文件，获取文件地址
	'''
	root = Tk()
	root.withdraw() # 消除Tk窗口
	#获取文件地址，默认为当前工作目录
	file_path = filedialog.askopenfilename(initialdir=os.getcwd())
	return file_path


# 输入
file_path = openfile()
data = pd.read_excel(file_path,index_col=0)

# 预处理
ele_combin = list(itertools.combinations(data.columns,2))

rcc_df = pd.DataFrame()

for ele in ele_combin:
    i = ele[0]
    m = ele[1]
    calculate_rcc(rcc_df,i,m,window=1000,center=False)

rcc_df = rcc_df.dropna()
rcc_df.to_excel("123.xlsx")
```