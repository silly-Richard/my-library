---
{"dg-publish":true,"permalink":"/1.说明文档/使用python 读取ORAS5nc文件/","created":"2024-04-28T20:33:59.264+08:00"}
---

# 介绍

由于1958-2014年逐月ORAS5数据[^1]的经纬度格网为非均匀格网，即经度与纬度数据并非一维数组，而是二维格网结构，因而不可用通常办法读取该nc文件。需要对其进行二维空间插值后才可以对其进行进一步处理。

![image-20240419184912106.png](/img/user/1.%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3/assets/%E4%BD%BF%E7%94%A8python%20%E8%AF%BB%E5%8F%96ORAS5nc%E6%96%87%E4%BB%B6/image-20240419184912106.png)
<center>通过xarray库读入nc文件可发现，lon和lat本身就为二维数组</center>

![image-20240419185030379.png](/img/user/1.%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3/assets/%E4%BD%BF%E7%94%A8python%20%E8%AF%BB%E5%8F%96ORAS5nc%E6%96%87%E4%BB%B6/image-20240419185030379.png)

![image-20240419185037407.png](/img/user/1.%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3/assets/%E4%BD%BF%E7%94%A8python%20%E8%AF%BB%E5%8F%96ORAS5nc%E6%96%87%E4%BB%B6/image-20240419185037407.png)
<center>panpoly软件下的nav_lon数据格式。该数组内部被划分为了1441*1201格，每格内存储了一个lon坐标。</center>

# 基础版本

**所使用到的数据集**
- 测试用数据集：[[1958年1月ORAS5表层海水盐度数据.nc\|1958年1月ORAS5表层海水盐度数据.nc]]，精度0.25°X0.25°。为方便处理，在测试中将其重命名为`abc.nc`。
**脚本**
- 使用说明：
	- 由于xarray包限制，工作地址必须没有中文。
	- 运行时可能会有提示`warnings.warn(f"A NumPy version >={np_minversion} and <{np_maxversion}"`，这个没有关系，这是由于numpy包版本有点过高，脚本依然可以运行。

脚本如下：
```python
import xarray as xr # 读取nc文件函数
import pandas as pd
import numpy as np
from metpy.interpolate import inverse_distance_to_grid # 二维空间插值函数

data = xr.open_dataset("123.nc") # 使用xarray导入nc文件

# 获取lon，lat和sss数据。ravel()函数表示将多维数组展平，转变为一维数组
lon = data["nav_lon"].values.ravel()
lat = data["nav_lat"].values.ravel()
sss = data["sosaline"].values.ravel()

# 构建一个df，只是想查看sss数据范围
data_df = pd.DataFrame({"lon":lon,"lat":lat,"sss":sss})
# agg函数，查看经纬网的最大和最小坐标，即sss数据的范围
data_df.agg(["min","max"])

# 如果想查看具体sss数据，可以运行以下量两句代码。写出格式只能为csv，由于数据太长（1472282）excel写不下。
#data_df.to_csv("sss_data.csv")

# 构建新的经纬网，lon范围为-180至180，lat范围为-77至90，精度为0.25°。lon和lat的范围可以根据agg函数结果设置。产生的是lon和lat的一维数组
lon_grid = np.arange(-180,180,0.25)
lat_grid = np.arange(-77,90,0.25)
# 使用np.meshgrid函数将一维的lon和lat构建成二维格网，为插值做准备。
lon_gridmesh,lat_gridmesh = np.meshgrid(lon_grid,lat_grid)

# 使用inverse_distance_to_grid函数进行二维插值。以原有的经纬度数据作为基础，将与它们最邻近的新格网坐标点值插值出来。
# 传入原始的lon,lat,sss,新构建格网lon_grid,lat_grid
# r:插值半径。必须设置，可以根据格网精度调整。min_neighbors:最小邻近半径。
# 生成的sss_grid便是插值后得到的一维sss数据
sss_grid = inverse_distance_to_grid(lon,lat,sss,lon_gridmesh,lat_gridmesh,r=0.2,min_neighbors=0.1)

# 如果有需要可以将结果输出。行名为纬度，列名为经度。
# sss_result = pd.DataFrame(sss_grid,columns=lon_grid,index=lat_grid)
# sss_result.o_csv("sss_result.csv")
```





[^1]: [ORAS5 global ocean reanalysis monthly data from 1958 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-oras5?tab=form)