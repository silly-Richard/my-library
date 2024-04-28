---
{"dg-publish":true,"permalink":"/1.说明文档/python小脚本/","created":"2024-04-28T21:21:43.858+08:00"}
---

# 基础对象处理
## 字符串翻转
```python
def reverseWords(input):
     
    # 通过空格将字符串分隔符，把各个单词分隔为列表
    inputWords = input.split(" ")
 
    # 翻转字符串
    # 假设列表 list = [1,2,3,4],  
    # list[0]=1, list[1]=2 ，而 -1 表示最后一个元素 list[-1]=4 ( 与 list[3]=4 一样)
    # inputWords[-1::-1] 有三个参数
    # 第一个参数 -1 表示最后一个元素
    # 第二个参数为空，表示移动到列表末尾
    # 第三个参数为步长，-1 表示逆向
    inputWords=inputWords[-1::-1]
 
    # 重新组合字符串
	output = " ".join(inputWords)
      
    return output

input = 'I like runoob'
rw = reverseWords(input)
print(rw)
```

## 列表翻转
```python
def reverse(li): 
	for i in range(0, len(li)/2): 
		temp = li[i] 
		li[i] = li[-i-1] 
		li[-i-1] = temp 

l = [1, 2, 3, 4, 5] 
reverse(l) 
print(l)
```

## 列表元素排列组合

**一个列表内元素排列组合**

使用itertools库的combinations函数
```python
import itertools

a = [1,2,3,4,5,6,7]

# combinations函数，组合，不注意顺序 #
# 传入参数：a，对象列表。2：组合元素的个数，int #
# 生成的元素组合格式为元组，作为a_combin列表的元素保存 #
a_combin = list(itertools.combinations(a,2))

# permutations函数，排列，注意顺序 #
# 传入参数：a，对象列表。2：排列元素的个数，int #
a_permut = list(itertools.permutations(a,2))

# 调用结果举例 #
for ele in a_combin:
	ele_1 = ele[0] # 使用元组序列号调用
	ele_2 = ele[1]

```

**两个列表的元素组合**

```python

m = [1,2,3,4]
n = [5,6,7,8,9]

p = []

# m中的元素与n中的每个元素组合 #
for x in m:
	for y in n:
		p.append((x,y))
p
>> [(1, 5), (1, 6), (1, 7), (1, 8), (1, 9), (2, 5), (2, 6), (2, 7), (2, 8), (2, 9), (3, 5), (3, 6), (3, 7), (3, 8), (3, 9), (4, 5), (4, 6), (4, 7), (4, 8), (4, 9)]

# m中的元素与n中相同序列号的元素组合 #
for x,y in zip(m,n):
	p.append((x,y))
p
# 由于n中的9的序列号为4，在m中不存在序列号为4的元素，所以被忽略
>> [(1, 5), (2, 6), (3, 7), (4, 8)]

```

---


# GUI相关
## 浏览本地文件导入

**使用tkinter的filedialog模块**，参考资料[^1]
```python
from tkinter import Tk
from tkinter import filedialog
import os

def openfile():
	'''
	选择文件，获取文件地址
	'''
	root = Tk()
	root.withdraw() # 消除Tk窗口
	#获取文件地址，默认为当前工作目录
	file_path = filedialog.askopenfilename(initialdir=os.getcwd())
	return file_path
```

---

# excel相关

## 写入excel

**按行方向写入excel**
```python
import xlsxwriter

def write_in_sheet_row(sheet_name,lists,title,row_num=0,start_num=1):
    '''
    将列表逐个写入指定excel的指定行内\n
    sheet_name:指定写入的excel工作簿\n
    lists:需要写入的数据，list格式\n
    title:该行的行名,str格式\n
    row_num:需要写入的行号,默认写入序列号0行\n
    start_num:需要写入的起始单元格位置,默认从序列号1单元格开始写入
    '''
    sheet_name.write(row_num,0,title)
    for list in lists:
        sheet_name.write(row_num,start_num,list)
        start_num += 1

```

**按列方向写入列表**
```python
import xlsxwriter

def write_in_sheet_col(sheet_name,lists,title,col_num=0,start_num=0):
    '''
    将列表逐个写入指定excel的指定列内,无返回值\n
    sheet_name:指定写入的excel工作簿\n
    lists:需要写入的数据，list格式\n
    title:该列的列名,str格式\n
    col_num:需要写入的行号,默认写入第0行\n
    start_num:需要写入的起始单元格位置,默认从第0个单元格开始写入
    '''
    sheet_name.write(0,col_num,title)
    for list in lists:
        sheet_name.write(start_num,col_num,list)
        start_num += 1
```

---

# 创建nc文件

```python
from netCDF4 import Dataset as D
import numpy

def create_nc_file(nc_name,lats,lons,data): # 传入文件名，经度，纬度，数据
	f_w = D(nc_name+'.nc','w',format='NETCDF4') # 创建nc文件
	f_w.createDimension('lat',3600) # 创建描述变量尺寸
	f_w.createDimension('lon',7200)

	f_w.createVariable('lat',numpy.float64,('lat')) # 创建变量
	f_w.createVariable('lon',numpy.float64,('lon'))

	f_w.variables['lat'][:] = lats # 向经纬度变量导入数据
	f_w.variables['lon'][:] = lons

	f_w.createVariable('data_name',numpy.float64,('lat','lon')) # 表明data_name变量受到经纬度变量的制约和描述
	f_w.variables['data_name'][:] = data
	f_w.close() # 保存文件

# 注意，使用该函数创建出的nc文件是一维文件，仅为经度，纬度所描述的对象，只有一层，如果要提取data数据，不需要使用[0]
```



[^1]: [python浏览本地文件，实现文件路径的选择\_滚雪球\~的博客-CSDN博客](https://blog.csdn.net/qq_21237549/article/details/121751136)