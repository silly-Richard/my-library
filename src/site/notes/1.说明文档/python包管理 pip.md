---
{"dg-publish":true,"permalink":"/1.说明文档/python包管理 pip/","created":"2024-04-28T21:30:49.612+08:00"}
---

# 介绍

pip是python自带的包管理工具，主要依靠cmd命令行运行

# 镜像地址

因网速问题，有时候无法从官方库下载第三方包，需要使用镜像网站。

- 清华源地址：`https://pypi.python.org/simple/``
- 阿里云地址：`https://mirrors.aliyun.com/pypi/simple/`
- 中国科技大学：`https://pypi.mirrors.ustc.edu.cn/simple/`
- 豆瓣：`https://pypi.douban.com/simple/`

镜像网站设置有两种方式：
- 手动设置：使用`-i`参数传入镜像网站url：`pip install [somepackage] -i URL`
- 配置文件修改：修改pip.ini（地址位于：`C:\Users\Dell\AppData\Roaming\pip\pip.ini`），将`index-url`值改为需要的镜像网站url


# 问题

## pip install 出现拒绝访问错误

引起这个错误的原因是因为当前账户的修改权限不足。在命令中添加`--user`，使用管理者身份运行命令[^1]。

```text
pip install --user *package_name*     # *package_name*即为你想安装的库
```


# 命令

## pip设置

```cmd
pip --help %获取帮助% 
pip --version %显示版本和路径%
pip install -U pip %升级pip%
```

## pip包管理

```cmd
pip list %列出已安装的包% 
pip list -o %查看可升级的包%

pip install [somepackage] %安装包%
pip install [somepackage] -i URL %从指定镜像网站安装指定包%
pip install [somepackage]==1.0.4 %==表示安装指定版本包%
pip install --upgrade [somepackage] %升级包%
pip uninstall [somepackage] %卸载包%

pip search [SomePackage] %搜索包%

pip show %显示安装包信息%
pip show -f [SomePackage] %查看指定包的详细信息%

pip debug --verbose %查看自己电脑可以安装什么配置的包%
```

[^1]: [pip install 出现[winError] 拒绝访问\_pip install numpy拒绝访问-CSDN博客](https://blog.csdn.net/a6840231/article/details/88246149)