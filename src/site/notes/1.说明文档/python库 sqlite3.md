---
{"dg-publish":true,"permalink":"/1.说明文档/python库 sqlite3/","created":"2024-04-28T21:21:43.836+08:00"}
---

sqlite3 模块是SQLite3的python api接口。

# 数据类型

sqlite3中可使用以下五种数据类型：
- NULL——空值——NULL
- Integer——整数——10,20
- Real——浮点数——1.0,2.5
- Text——字符串——hello
- Blob——blob数据，二进制大对象——文件，音频，zip压缩包等

# 安装

使用pip安装[^1]：`pip install pysqlite3`

# 创建数据库，创建表

1.使用`connect()`函数创建数据库对象`conn`，或者链接已创建的数据库。
2.使用`cursor()`函数创建一个游标对象。（游标：数据库中用来移动和执行查询的对象）
3.使用游标创建表。
4.使用游标添加数据。
5.使用游标执行sql语句。
6.关闭数据库链接

```python
import sqlite3

conn = sqlite3.connect('database.db') # 创建数据库
cur = conn.cursor() # 创建游标cursor对象

## 创建表头信息 ##
## sql语法：按照sql语法，诸如create table等命令应该要大写，但是小写也行
## students：表名title。
## name，sexy，id_num：行名及其数据类型
sql = '''create table students (
		name text,
		sexy text,
		id_num int)'''
		
# 使用execute()方法创建表，本质是将sql语句传递给execute，由其执行
cursor.execute(sql) 

# 这样写也可以
cursor.execute("create table students(name text,sexy text,id_num int)")

```

调用sql内置的`sqlite_master`查询表是否存在
```python
# 查询数据库中所有表的表名，返回元组tuple
res = cur.execute("SELECT name FROM sqlite_master")
# 调用res.fetchone()获取结果
res = cur.execute("SELECT name FROM sqlite_master").fetchone()

# 查询指定表是否存在，如果存在，返回表名，如果不存在，返回None
res = cur.execute("SELECT name FROM sqlite_master WHERE name='students'").fetchone()

res is None # 返回布尔值，判断res是否为None

```

# 写入数据

使用sql语句`INSERT INTO table VALUES()`插入
```python
conn = sqlite3.connect('database.db') # 创建数据库
cur = conn.cursor() # 创建游标cursor对象

# 逐行插入
cur.execute('''insert into students values
           ("anna","girl",18),
            ("bob","boy",18)''')

# 一次性多行插入，需要使用executemany函数
data = [
    ("anna","girl",18),
    ("bob","boy",18),
    ("alex","boy",18)
]

## 占位符 (placeholders) `?` 是用来在查询中绑定数据 `data` 的
cur.executemany("insert into students values(?,?,?)",data)

# INSERT语句将隐式创建一个事务，事务需要在将更改保存到数据库前提交
conn.commit()

```

# 查询内容

使用sql语句`SELECT obj FROM table`查询
```python
conn = sqlite3.connect('database.db') # 创建数据库
cur = conn.cursor() # 创建游标cursor对象

# 单列查询，返回由元组组成的列表
res = cur.execute("select name from students")
# 调用res.fetchall()函数输出所有结果
res = cur.execute("select name from students").fetchall()

# 逐列查询
for row in cur.execute("select name,sexy,id_num from students order by id_num"):
	print(row)

```

# 更新内容

使用sql语句`UPDATE table SET obj_1=value WHERE obj_2=value`
```python
conn = sqlite3.connect('database.db') # 创建数据库
cur = conn.cursor() # 创建游标cursor对象

data = [
    ("anna","girl",1),
    ("bob","boy",2),
    ("alex","boy",3)
]

cur.executemany("insert into students values(?,?,?)",data)

conn.commit()

# 单个修改，将bob的id改为5
cur.execute("update students set id_num=5 where name="bob")

conn.commit() # update语句需要提交修改
```

# 删除行

使用sql语句`DELETE FROM table WHERE obj=value`

```python
conn = sqlite3.connect('database.db') # 创建数据库
cur = conn.cursor() # 创建游标cursor对象

data = [
    ("anna","girl",1),
    ("bob","boy",2),
    ("alex","boy",3)
]

cur.executemany("insert into students values(?,?,?)",data)

conn.commit()

# 删除name=anna的整行数据
cur.execute("delete from students where name='anna'")

conn.commit() # selete语句需要提交修改
```

# 查表

sql语句

```sql
CREATE TABLE table_name --创建表
DROP TABLE table_name --删除表
INSERT INTO table_name VALUES() --在表中插入行
SELECT obj FROM table_name --查询
UPDATE table_name SET obj_1=value WHERE obj_2=value --单个更新
DELETE FROM table_name WHERE obj=value --删除整行
```


[^1]: [pip安装sqlite3-CSDN博客](https://blog.csdn.net/qq_42275888/article/details/105369293)