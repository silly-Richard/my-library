---
{"dg-publish":true,"permalink":"/1.说明文档/使用python scipy库线性插值/","created":"2024-04-28T21:21:43.928+08:00"}
---


# 介绍

此脚本的设计目的是使用python的scipy库的interpolate函数，对带有年代层位的研究指标进行线性插值。

计算依据：使用interpolate函数计算出原始数据中`年代`与`研究指标`之间的函数关系，将该关系运用到新的`年代`序列中，计算出对应的`研究指标`数值。

# 初始版1.0

**所使用到的数据集**
- 原始层位数据和年代数据：[[原数据.xlsx\|原数据.xlsx]]，即脚本中导入的`深度年龄.xlsx`
- 目标层位数据，求年代数据：[[目标数据.xlsx\|目标数据.xlsx]]，即脚本中导入的`depth.xlsx`

**脚本**
- 封装使用说明：
	- 封装exe必须与要处理的数据集放在同一目录下
	- 原始数据文件名必须是`深度年龄.xlsx`，目标数据文件名必须是`depth.xlsx`
	- 原始数据中`插值依据`的列名必须是**层位**，插值对象的列名必须是**年代**
	- 目标数据中的`插值依据`的列名必须是**层位**
	- 插值年代原则上允许外推
	- 计算结果的文件名固定为**脚本插值结果.xlsx**

脚本如下：
```python
# 注意：若需要外插，则需要注意bounds_error和fill_values参数


import pandas as pd
from scipy import interpolate 

data = pd.read_excel('深度年龄.xlsx')
data_target = pd.read_excel('depth.xlsx')

  
x = data['层位'] # 输入列标签名对dataframe格式数据切片
y = data['年代']

x_new = data_target['层位']

f=interpolate.interp1d(x,y,kind='slinear',bounds_error=False,fill_value='extrapolate')
# 通过原数据中x和y值建立关系f
# kind参数为选择方法：线性插值
# bounds_error参数：默认为true，当true时，不允许目标数据超过原数据，即x_new范围必须小于x。若插值需要外推，则将该参数改为false。
# fill_value参数：当bounds_error参数为false时，传入'extrapolate'参数以允许外推

y_new=f(x_new)
# 将目标层位带入上述建立的关系中，插值输出对应的年代。
# 该y_new数据格式为ndarray 

y_new = pd.DataFrame(y_new,index=x_new)
# to_excel()函数只能以dataframe数据格式为对象，所以需要将数据从ndarray转化为dataframe
# 使用index参数指定了y_new数据的行标签名，而非使用系统的整数填充。

y_new.to_excel('脚本插值结果.xlsx')
# 使用to_excel()函数输出结果为excel
```


# 版本2.0

**增加内容**
- 增加了循环插值功能，可以同时计算多个对象
- 只需要传入一个文件就可以


**脚本**
- 封装使用说明：
	- 封装exe必须与要处理的数据集放在同一目录下
	- 原始数据文件名必须是`插值原始数据.xlsx`
	- 原始数据第一列必须是`年代`（即插值依据），从第二列开始为`研究指标`（即插值对象），可以一次性输入多个`研究指标`
	- 插值年代间隔为1，**不可修改**。插值区间可选择，原则上允许外推
	- 计算结果的文件名固定为**插值结果.xlsx**

脚本如下：
```python
import pandas as pd
from scipy import interpolate
import numpy as np
import xlsxwriter

def write_in_sheet_col(sheet_name,lists,title,col_num=0,start_num=1):
    '''
    将列表逐个写入指定excel的指定列内,无返回值\n
    sheet_name:指定写入的excel工作簿\n
    lists:需要写入的列表\n
    col_num:需要写入的列号,默认写入第0列\n
    start_num:需要写入的起始单元格位置,默认从第1个单元格开始写入
    '''
    sheet_name.write(0,col_num,title)
    for list in lists:
        sheet_name.write(start_num,col_num,list)
        start_num += 1


f = xlsxwriter.Workbook('插值结果.xlsx') # 创建结果文件
sheet = f.add_worksheet("插值结果")

data = pd.read_excel("插值原始数据.xlsx") # 读取原始数据
print("原始数据的描述尺度为"+data.columns[0]+",最小值为"+str(data[data.columns[0]][0])+",最大值为"+str(data[data.columns[0]][len(data[data.columns[0]])-1]))

print("请输入目标插值尺度的起始和结束数据,间隔为1,不要超过原始数据的描述尺度!")
start_num = int(input("请输入目标插值起始数据："))
end_num = int(input("请输入目标插值结束数据："))
x_new = np.linspace(start_num,end_num,end_num-start_num+1)# 创建插值目标对象
write_in_sheet_col(sheet,x_new.tolist(),"年代")
num = 1

for i in data.columns[1:]: # 从data索引值1开始，注意i实际上是data的列名，str
    x = data[data.columns[0]]
    y = data[i]
    fun = interpolate.interp1d(x,y,kind="slinear",bounds_error=False,fill_value="extrapolate")
    y_new = fun(x_new).tolist() # 从array转变为list
    write_in_sheet_col(sheet,y_new,i,col_num=num)
    num += 1
f.close()
```



# 版本3.0

**增加内容**
- 增加了窗口化选择文件功能，允许传入文件与exe不在同一目录下
- 允许传入文件名任意
- 允许修改插值年代间隔
- 设置了窗口暂停选项，按任意键退出

**脚本**
- 已知问题：
	- 脚本启动时会显示warning，这是使用numpy版本过高，与scipy有些不匹配
	- 如果在传入文件时，选择其他窗口，可能会导致传入窗口消失
- 封装使用说明：
	- 传入文件窗口的默认地址为该exe所在目录
	- 原始数据第一列必须是`年代`（即插值依据），从第二列开始为`研究指标`（即插值对象），可以一次性输入多个`研究指标`
	- 插值年代间隔可以修改，**必须传入正整数**（推荐1,5,10等）
	- 插值区间可选择，原则上允许外推
	- 计算结果文件名固定为**插值结果.xlsx**，保存在该exe所在目录下

脚本如下：
```python
import pandas as pd
from scipy import interpolate
import numpy as np
import xlsxwriter
from tkinter import Tk
from tkinter import filedialog
import os

def write_in_sheet_col(sheet_name,lists,title,col_num=0,start_num=1):
    '''
    将列表逐个写入指定excel的指定列内,无返回值\n
    sheet_name:指定写入的excel工作簿\n
    lists:需要写入的列表\n
    col_num:需要写入的列号,默认写入第0列\n
    start_num:需要写入的起始单元格位置,默认从第1个单元格开始写入
    '''
    sheet_name.write(0,col_num,title)
    for list in lists:
        sheet_name.write(start_num,col_num,list)
        start_num += 1


def openfile():
    ''''
    选择文件,获取文件地址
    '''
    root = Tk()
    root.withdraw()
    file_path = filedialog.askopenfilename(initialdir=os.getcwd())
    return file_path


openfile_path = openfile() # 获取文件地址
data = pd.read_excel(openfile_path) # 读取原始数据
print("该地址文件已读入:"+openfile_path)
print("插值年代最小值为"+str(data[data.columns[0]][0])+",最大值为"+str(data[data.columns[0]][len(data[data.columns[0]])-1])) # 反馈年代基本情况

start_num = int(input("请输入目标插值起始数据："))
end_num = int(input("请输入目标插值结束数据："))
step = int(input("请输入插值间隔,最小为1,请输入一个正整数:"))
x_new = np.linspace(start_num,end_num,int((end_num-start_num)/step)+1)# 创建插值目标对象


savefile_path = os.getcwd()+"\插值结果.xlsx"
f = xlsxwriter.Workbook(savefile_path) # 创建结果文件
sheet = f.add_worksheet("插值结果")
write_in_sheet_col(sheet,x_new.tolist(),"年代")
num = 1

for i in data.columns[1:]: # 从data索引值1开始，注意i实际上是data的列名，str
    x = data[data.columns[0]]
    y = data[i]
    fun = interpolate.interp1d(x,y,kind="slinear",bounds_error=False,fill_value="extrapolate")
    y_new = fun(x_new).tolist() # 从array转变为list
    write_in_sheet_col(sheet,y_new,i,col_num=num)
    num += 1
    print("已插值完毕————"+i)

f.close()
print("全部对象已经插值完毕。生成文件:插值结果.xlsx\n生成于"+savefile_path)

input('请输入任意键退出')
```