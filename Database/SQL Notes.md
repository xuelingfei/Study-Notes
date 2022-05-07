# SQL Notes

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [Python 连接 MySQL](#Python-连接-MySQL)
- [Python 调用 SQL 语句分页](#Python-调用-SQL-语句分页)

</details>

## 一些常用的 SQL 命令

    SELECT - 从数据库中提取数据
    UPDATE - 更新数据库中的数据
    DELETE - 从数据库中删除数据
    INSERT INTO - 向数据库中插入新数据
    CREATE DATABASE - 创建新数据库
    ALTER DATABASE - 修改数据库
    CREATE TABLE - 创建新表
    ALTER TABLE - 变更（改变）数据库表
    DROP TABLE - 删除表
    CREATE INDEX - 创建索引（搜索键）
    DROP INDEX - 删除索引
ORDER BY 排位
GROUP BY 分组
DISTINCT 去重
DESC 降序


## Python 连接 MySQL

通过 pymysql
```py
# 连接数据库
connect = pymysql.Connect(
    host='localhost',
    port=3306,
    user='root',
    passwd='123',
    db='test',
    charset='utf8'
)

sql = "SELECT * FROM database"
cursor = connect.cursor()
cursor.execute(sql)
item_tuple = cursor.fetchall()
wea_list = []
for item in item_tuple:
    wea_list.append(item[2])

# 关闭连接
cursor.close()
connect.close()





import pymysql.cursors

# 连接数据库
connect = pymysql.Connect(
    host='localhost',
    port=3310,
    user='user',
    passwd='123',
    db='test',
    charset='utf8'
)
# 事务处理	
sq11 = "UPDATE staff SET saving = saving + 1000 WHERE user id = '1001'"
sq12 = "UPDATE staff SET expend = expend + 1000 WHERE user id = '1001'"
sq13 = "UPDATE staff SET income = income + 2000 WHERE user id = '1001'"

try:
    cursor.execute(sq11)  # 储蓄增加1000
    cursor.execute(sq12)  # 支出增加1000
    cursor.execute(sq13)  # 收入增加2000
except Exception as e:
    connect.rollback()  # 事务回滚
    print('事务处理失败', e)
else:
    connect.commit()  # 事务提交
print('事务处理成功', cursor.rowcount)

# 关闭连接
cursor.close()
connect.close()
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




SELECT
	team.id,
	team.teamName,
	member.nickname,
	leader.nickname 
FROM
	team
	LEFT JOIN `user` AS member ON member.userId = team.members
	LEFT JOIN `user` AS leader ON leader.userId = team.leaderId 
WHERE
	team.id =1

可以多次关联同一张表



group by 和 count
select sta.organization_id id,
count(case when pd.status <> -1 then 1 end) sum,
count(case when pd.status = 1 then 1 end) shtg,
count(case when pd.status = 0 then 1 end) dsh
from pms_dailyproductionreports pd
group by sta.organization_id




sql中对日期的筛选

#几个小时内的数据
DATE_SUB(NOW(), INTERVAL 5 HOUR)
#今天
select * from 表名 where to_days(时间字段名) = to_days(now());
#昨天
SELECT * FROM 表名 WHERE TO_DAYS( NOW( ) ) - TO_DAYS( 时间字段名) <= 1
#7天
SELECT * FROM 表名 where DATE_SUB(CURDATE(), INTERVAL 7 DAY) <= date(时间字段名)
#近30天
SELECT * FROM 表名 where DATE_SUB(CURDATE(), INTERVAL 30 DAY) <= date(时间字段名)
#本月
SELECT * FROM 表名 WHERE DATE_FORMAT( 时间字段名, '%Y%m' ) = DATE_FORMAT( CURDATE( ) , '%Y%m' )
#上一月
SELECT * FROM 表名 WHERE PERIOD_DIFF( date_format( now( ) , '%Y%m' ) , date_format( 时间字段名, '%Y%m' ) ) =1
#查询本季度的数据
SELECT * FROM order WHERE QUARTER(时间字段名)=QUARTER(NOW())
#查询上季度的数据
SELECT * FROM order WHERE QUARTER(时间字段名)=QUARTER(DATE_SUB(NOW(),INTERVAL 1 QUARTER))
#查询当年（今年）的数据
SELECT * FROM `order` WHERE YEAR(order_time)=YEAR(NOW())
#查询去年的数据
SELECT * FROM `order` WHERE YEAR(order_time)=YEAR(DATE_SUB(NOW(),INTERVAL 1 YEAR))

https://www.cnblogs.com/weifeng123/p/11014114.html