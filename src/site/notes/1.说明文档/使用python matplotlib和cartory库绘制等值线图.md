---
{"dg-publish":true,"permalink":"/1.说明文档/使用python matplotlib和cartory库绘制等值线图/","created":"2024-04-28T21:21:43.889+08:00"}
---

**输入数据准备**

- x坐标：数据类型：一维数组
- y坐标：数据类型：一维数组
- z值：以x，y为基础构建的二维数组

**使用函数**

- `matplotlib.pyplot.contourf()`函数。主要功能：填充等高线内部颜色。
- `matplotlib.pyplot.contour()`函数。主要功能：绘制登高线轮廓。
- `cartopy.crs.PlateCarree()`函数。主要功能：为等高线图设置坐标系。


**脚本示例**

例如，在[[1.说明文档/使用python 读取ORAS5nc文件\|使用python 读取ORAS5nc文件]]的末尾，会得到经过二维空间插值后的lon数据，lat数据和sss数据。以此为例绘制全球表层海水盐度等值线图。

```python
import matplotlib.pyplot as plt
import cartopy.crs as ccrs

### 默认已获取lon，lat，sss数据 ###

## 创建绘图窗口对象fig
fig = plt.figure(figsize=(12, 8)) # 设置窗口大小

## 创建坐标系实例proj
proj = ccrs.PlateCarree() # 此为cartopy的默认投影,为圆柱投影

## 创建子图对象ax，将坐标系设置为proj
ax = fig.add_subplot(projection=proj) # 设置绘图的坐标系

## gridlines方法绘制网格
### draw_labels：是否绘制网格标签
### color：设置网格颜色
### alpha：设置网格透明度，范围为0-1，0为完全透明，1为不透明
### linestyle：设置网格样式
### xlocs/ylocs:设置x轴、y轴范围及间隔，一维数组
gl = ax.gridlines(draw_labels=True, color="gray", alpha=0, linestyle=':',xlocs=np.arange(-180, 180, 20), ylocs=np.arange(-90, 90, 10)) 

## contourf方法填充等值线，在ax基础上创建对象ac
### 依次传入x坐标lon，y坐标lat，z坐标sss，坐标系transform，等值线划分层级leves，填色色带带号cmap
### levels也可以传入列表，进行手动分层
### 若不指定cmap参数，则会填充默认色带
### 其他可选参数：透明度alpha
ac = ax.contourf(lon_grid, lat_grid, sss_grid,transform=proj, levels=20, cmap="tab20c")

## contour方法，在ac基础上，使用指定颜色绘制轮廓线
ad = ax.contour(ac, colors='k')

## colorbar方法，在ac基础上绘制色带，创建对象cb
### orientation：指定如何放置色带
### shrink：色带长度，以subplot为参照，色带为subplot长的几倍
cb = plt.colorbar(ac, orientation='horizontal', shrink=0.8)

## set_label方法，在cb基础上，设置色带名称
cb.set_label(r"sss", fontsize=15)

```