# Python 和 MySQL

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [Python 通过 PyMySQL 模块连接 MySQL](#Python-通过 PyMySQL 模块连接-MySQL)
- [Python 调用 SQL 语句分页](#Python-调用-SQL-语句分页)

</details>

## Python 通过 PyMySQL 模块连接 MySQL

1. 安装

   `python -m pip install PyMySQL` 或 `pip install pymysql`

2. CURD

   - 创建 MySQL 的连接

     ```py
     import pymysql

     connection = pymysql.connect(
         host='localhost',  # 数据库服务器
         port=3306,  # 端口号，默认 3306
         user='kylin',  # 用户名
         password='12345678',  # 密码
         database='test',  # 数据库名
         charset='utf8mb4',  # 字符集
         cursorclass=pymysql.cursors.DictCursor  # 默认查询的数据是以元组的形式输出，设置 `cursorclass` 为 `DictCursor` 后会以字典的形式输出
     )
     ```

     操作完数据库后，记得及时关闭连接

     ```py
     connection.close()
     ```

     或者使用上下文管理器，交给 python 自动关闭

     ```py
     with connection:
         pass
     ```

   - 创建 Cursor 游标对象

     创建游标对象，用来操作数据库，接收一个游标类型的参数，默认是 `pymysql.cursors.Cursor` 类，使用 `pymysql.cursors.DictCursor` 数据会以字典的形式输出（连接时设置了，这里就不需要设置了）

     ```py
     cursor = connection.cursor(cursor=pymysql.cursors.DictCursor)
     ```

     跟 `connection` 一致，使用完以后，记得手动关闭

     ```py
     cursor.close()
     ```

     或者使用上下文管理器，交给 python 自动关闭

     ```py
     with connection.cursor(cursor=pymysql.cursors.DictCursor) as cursor:
         pass
     ```

   - 使用 Cursor 执行 SQL 查询语句

     ```py
     # 调用 execute 方法，执行一条语句
     cursor.execute("select * from user")

     print(cursor.fetchone())  # 打印查询的一条数据
     print(cursor.fetchall())  # 打印查询的所有数据
     print(cursor.fetchmany(10))  # fetchmany(num) 打印查询的指定数量的数据
     print(cursor.rowcount)  # 打印你查询结果的行数
     ```

   - 使用 Cursor 执行 SQL 编辑语句

     使用 pymysql 执行新增、删除、编辑语句后，需要执行 `commit()` 方法

     ```py
     row_number = cursor.execute("UPDATE user SET age=18 WHERE id=1")
     print(row_number)  # 打印编辑语句影响的行数
     connection.commit()  # 提交修改
     print(cursor.lastrowid)  # 如果执行的是 INSERT 语句，且主键是自增的，lastrowid 会打印下一个主键 id
     ```

     对可能发生异常的语句，可以加上异常捕获，在捕获到异常时执行回滚

     ```py
     connection.rollback()
     ```

     参考链接：<https://www.jianshu.com/p/6e54d0fa4922>

   - 执行多条语句(executemany)

     ```py
     import pymysql

     connection = pymysql.connect(
         host='localhost',
         port=3306,
         user='kylin',
         password='12345678',
         database='test',
         charset='utf8mb4'
     )

     data01 = [['Ada', '23'], ['Black', '19'], ['Tim', '30']]
     data02 = [('Green', '25'), ('Bai', '32')]

     cursor = connection.cursor()
     try:
         sql = 'INSERT INTO `user` (`name`, `age`) VALUES (%s, %s)'
         cursor.executemany(sql, data01)
         cursor.executemany(sql, data02)
         connection.commit()
         print('成功')
     except Exception as e:
         connection.rollback()
         print('错误信息：', e)

     cursor.close()
     connection.close()
     ```

     参考链接：<https://blog.csdn.net/m0_37422217/article/details/105272064>

   - 示例一（官网示例）

     ```py
     import pymysql.cursors

     # Connect to the database
     connection = pymysql.connect(
         host='localhost',
         user='kylin',
         password='12345678',
         database='test',
         charset='utf8mb4',
         cursorclass=pymysql.cursors.DictCursor
     )

     with connection:
         with connection.cursor() as cursor:
             # Create a new record
             sql = "INSERT INTO `user` (`email`, `password`) VALUES (%s, %s)"
             cursor.execute(sql, ('webmaster@python.org', 'very-secret'))

         # connection is not autocommit by default. So you must commit to save your changes.
         connection.commit()

         with connection.cursor() as cursor:
             # Read a single record
             sql = "SELECT `id`, `password` FROM `user` WHERE `email`=%s"
             cursor.execute(sql, ('webmaster@python.org',))
             result = cursor.fetchone()
             print(result)
     ```

   - 示例二

     ```py
     import pymysql.cursors

     connection = pymysql.connect(
         host='localhost',
         port=3306,
         user='kylin',
         password='12345678',
         database='test',
         charset='utf8mb4'
     )

     # 读取数据
     sql = "SELECT * FROM user"
     cursor = connection.cursor()
     cursor.execute(sql)
     result = cursor.fetchall()
     print(result)

     # 事务处理
     sql01 = "UPDATE user SET age = age + 1 WHERE id = 1"
     sql02 = "UPDATE user SET age = age + 1 WHERE id = 2"
     sql03 = "UPDATE user SET age = age + 1 WHERE id = 3"

     try:
         cursor.execute(sql01)
         cursor.execute(sql02)
         cursor.execute(sql03)
     except Exception as e:
         connection.rollback()  # 事务回滚
         print('事务处理失败', e)
     else:
         connect.commit()  # 事务提交
         print('事务处理成功', cursor.rowcount)

     # 关闭连接
     cursor.close()
     connection.close()
     ```

## Python 调用 SQL 语句分页

示例如下，其中 `page_size` 是每页条数，`page_no` 为当前页数，从 0 开始

```py
sql = "SELECT * FROM example  "
if page_size:
    start_no = page_no * page_size
    sql += "LIMIT " + str(start_no) + ", " + str(page_size) + "  "
cursor = connection.cursor()
cursor.execute(sql)
item_tuple = cursor.fetchall()
```
