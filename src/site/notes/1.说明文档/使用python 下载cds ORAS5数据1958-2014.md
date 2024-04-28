---
{"dg-publish":true,"permalink":"/1.说明文档/使用python 下载cds ORAS5数据1958-2014/","created":"2024-04-28T21:21:43.946+08:00"}
---

使用python cdsapi库下载哥白尼气象数据中心`ORAS5 global ocean reanalysis monthly data`数据。

以单层数据（single level）表层海水盐度指标为例。

```python
import cdsapi
import os
import shutil

def dirs(path):
    folder = os.path.exists(path)

    if not folder:
        os.makedirs(path)
    else:
        print(path+'年文件夹已经创建')


year = [
            '1958', '1959', '1960',
            '1961', '1962', '1963',
            '1964', '1965', '1966',
            '1967', '1968', '1969',
            '1970', '1971', '1972',
            '1973', '1974', '1975',
            '1976', '1977', '1978',
            '1979', '1980', '1981',
            '1982', '1983', '1984',
            '1985', '1986', '1987',
            '1988', '1989', '1990',
            '1991', '1992', '1993',
            '1994', '1995', '1996',
            '1997', '1998', '1999',
            '2000', '2001', '2002',
            '2003', '2004', '2005',
            '2006', '2007', '2008',
            '2009', '2010', '2011',
            '2012', '2013', '2014',
        ]

month = ['01', '02', '03','04', '05', '06','07', '08', '09','10', '11', '12',]

for y in year:
    dirs(y)
    for m in month:
        check_file = os.path.exists('%s/%s%s.zip' %(y,y,m))
        if not check_file:
            c = cdsapi.Client()
            print(y+'年'+m+'月'+'文件正在下载')
            c.retrieve(
                'reanalysis-oras5',
                {
                    'product_type': 'consolidated',
                    'vertical_resolution': 'single_level',
                    'variable': 'sea_surface_salinity',
                    'format': 'zip',
                    'year': [y],
                    'month': [m],
                    'day': [
                        '01', '02', '03',
                        '04', '05', '06',
                        '07', '08', '09',
                        '10', '11', '12',
                        '13', '14', '15',
                        '16', '17', '18',
                        '19', '20', '21',
                        '22', '23', '24',
                        '25', '26', '27',
                        '28', '29', '30',
                        '31',
                    ],
                },
                y+m+'.zip')
            shutil.move(y+m+'.zip',y)
        else:
            print(y+'年'+m+'月'+'文件已下载')

```