---
{"dg-publish":true,"permalink":"/1.说明文档/使用python osgeo库创建单波段geotiff文件/","created":"2024-04-28T21:21:43.900+08:00"}
---

# 介绍

**用途**
- 已知坐标点，需要从nc文件中提取坐标点数值。

# 使用osgeo库

**前置数据获取**
- 从nc文件中读取的lon数据：一维数据
- 从nc文件中读取的lat数据：一维数据
- 从nc文件中读取的指标数据：以lon和lat构建的二维数据，以sss为例

思路
- 确定数据最大最小经纬度值，经纬度长度和分辨率
- 构建tiff对象，包含tiff名称，经纬度分辨率，波段数，编码方式
- 构建tiff网格，以特定参照点为标志，延伸经纬网构建网格
	- 关于`SetGeoTransform`函数内容，可见
- 构建tiff坐标系
- 导入指标数据，消除空值
- 保存tiff

脚本如下：
```python
from osgeo import gdal,osr

## 获取最大，最小经纬度坐标值，作为建立tiff文件的参照点
lonmax = lon.Max()
lonmin = lon.Min()
latmax = lat.Max()
latmin = lat.Min()

## 获取lon和lat的分辨率和长度，以参照点为标志延伸
N_lon = len(lon)
N_lat = len(lat)

lon_res = (lonmax-lonmin)/(float(N_lon)-1) # 获取分辨率
lat_res = (latmax-latmin)/(float(N_lat)-1)

## 创建tiff文件
driver = gdal.GetDriverByName("GTiff") # 创建一个空的tiff实例
out_tif_name ="new_tiff"+".tif" # 设置tiff的名称

## 使用Create方法创建geotiff对象。传入参数依次为：tiff名称，经度长度，纬度长度，波段数（只有一个时间段所以为1），tiff文件编码方式。
out_tif = driver.Create(out_tif_name,N_lon,N_lat,1,gdal.GDT_CFloat32)

## 这样创建的tiff空有名称，经纬度长度等信息，没有有效的格网和坐标系信息，下面为它添加格网和坐标系

# 创建一个经纬度样式geotransform。它需要依次传入以下内容：最小经度，经度分辨率，经度旋转角度（由于不旋转，所以为0），最小纬度，纬度旋转角度（由于不旋转，所以为0），纬度分辨率。
geotransform = (lonmin,lon_res,0,latmin,0,lat_res)

# 将这个样式传递给SetGeoTransform方法，为已经建立的out_tif文件创建格网
out_tif.SetGeoTransform(geotransform)

# 创建一个坐标系实例srs，将其设置为WGS 84坐标系（EPSG 4326为WGS 84的编号）
srs = osr.SpatialReference()
srs.ImportFromEPSG(4326)

# 将srs传入，赋予out_tif坐标系
out_tif.SetProjection(srs.ExportToWkt())

# 格网和坐标系搭建完毕后，将插值得到的sss数据写入tiff中
out_tif.GetRasterBand(1).WriteArray(sss)

# 将sss中的空值修改为-99
out_tif.GetRasterBand(1).SetNoDataValue(-99)
out_tif.FlushCache() # 将数据写入硬盘
del out_tif # 注意必须关闭tif文件，才能保存

```

