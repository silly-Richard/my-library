---
{"dg-publish":true,"permalink":"/1.说明文档/python库 matplotlib/","created":"2024-04-28T21:21:43.756+08:00"}
---

# 绘制图像

## 绘制等值线图

- `matplotlib.pyplot.contourf()`函数。主要功能：填充等高线内部颜色。
- `matplotlib.pyplot.contour()`函数。主要功能：绘制登高线轮廓。

contourf和contour函数在绘制等值线图时功能不同，但是参数是相同的。

```python
contourf([X, Y,] Z,[levels], **kwargs)
```
参数如下：
- x：x坐标，一维数组，必选
- y：y坐标，一维数组，必选
- z：以x，y为基础的二维数组，必选
- transform=：指定坐标系
- levels=：划分z的层级数。可以只传入一个int，表示划分为多少层。也可以传入一个int列表，以手动划分层级
- cmap=：填充的色带号，若不指定则为默认色带。对contourf有效
- color=：线段颜色，对contour有效
- alpha=：设置透明度，0为完全透明，1为不透明


示例：
- [[1.说明文档/使用python matplotlib和cartory库绘制等值线图\|使用python matplotlib和cartory库绘制等值线图]]