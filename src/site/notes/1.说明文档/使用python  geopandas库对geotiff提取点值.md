---
{"dg-publish":true,"permalink":"/1.说明文档/使用python  geopandas库对geotiff提取点值/","created":"2024-04-28T21:21:43.870+08:00"}
---

# 介绍

如同arcgis一样，构造一个点shp文件进行值提取是一个有效的手段。使用geopandas函数构造具有地理坐标系的点shp数据，对geotiff进行值提取。

# 思路

- 使用rasterio包读取geotiff文件。
- 使用pandas包读取带有经纬度坐标值的目标点数据。
- 使用geopandas包将不具有地理意义的经纬度坐标转化为具有坐标系的点shp文件。
- 使用shp文件对geotiff数据进行计算。

脚本如下
```python
import pandas as pd
import numpy as np
import rasterio # 读取tiff文件
import geopandas

rs = rasterio.open("new_tiff.tif") # 使用rasterio库读取tiff文件
station = pd.read_csv("station.csv") # 使用pandas读取station.csv

# 使用geopandas.GeoDataFrame构建点shp文件。传入参数依次为：
## station对象
## geometry（使用points_from_xy方法将目标点经度station.lon和纬度station.lat构建起来，从普通数据转变为地理坐标）
## crs：赋予点shp文件坐标系。"EPSG:4326"，即WGS 84坐标系
point_geo = geopandas.GeoDataFrame(station,geometry=geopandas.points_from_xy(station.lon,station.lat),crs="EPSG:4326")

def ExtractPointValue(geometry,rs):
    '''
    geometry: geodataframe的geometry列
    rs: 栅格数据
    '''
    x = geometry.xy[0][0]
    y = geometry.xy[1][0]
    row, col = rs.index(x,y)
    value = rs.read(1)[row,col]
    return value

point_geo["value"] = point_geo["geometry"].apply(ExtractPointValue,rs=rs) # 值提取

point_geo.to_csv("point_value.csv") # 输出结果
```

