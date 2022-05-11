# MySQL Notes

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [常用 SQL 命令速查](#常用-SQL-命令速查)
- [创建数据库](#创建数据库)
- [创建表](#创建表)
- [JOIN](#JOIN)
- [COUNT 和 GROUP BY](#COUNT-和-GROUP-BY)

</details>

## 常用 SQL 命令速查

| 命令              | 功能                 |
| ----------------- | -------------------- |
| `CREATE DATABASE` | 创建新数据库         |
| `ALTER DATABASE`  | 修改数据库           |
| `CREATE TABLE`    | 创建新表             |
| `ALTER TABLE`     | 变更（改变）数据库表 |
| `DROP TABLE`      | 删除表               |
| `CREATE INDEX`    | 创建索引（搜索键）   |
| `DROP INDEX`      | 删除索引             |
| `INSERT INTO`     | 向数据库中插入新数据 |
| `SELECT`          | 从数据库中提取数据   |
| `UPDATE`          | 更新数据库中的数据   |
| `DELETE`          | 从数据库中删除数据   |
| `ORDER BY`        | 排序                 |
| `GROUP BY`        | 分组                 |
| `DISTINCT`        | 去重                 |
| `DESC`            | 降序                 |

## 创建数据库

示例：创建名为 `test` 的数据库

```sql
CREATE DATABASE `test` CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_general_ci';
```

## 创建表

示例：创建名为 `user` 的表

```sql
CREATE TABLE `user` (
	`id` INT ( 11 ) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR ( 255 ) COLLATE utf8_bin NOT NULL,
	`age` INT ( 11 ) NOT NULL,
	`password` VARCHAR ( 255 ) COLLATE utf8_bin NOT NULL,
	`email` VARCHAR ( 255 ) COLLATE utf8_bin NOT NULL,
PRIMARY KEY ( `id` )
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4 COLLATE = utf8mb4_bin AUTO_INCREMENT = 1;
```

## JOIN

- 不同的 SQL JOIN

  - `INNER JOIN` - 如果表中有至少一个匹配，则返回行
  - `LEFT JOIN` - 即使右表中没有匹配，也从左表返回所有的行
  - `RIGHT JOIN` - 即使左表中没有匹配，也从右表返回所有的行
  - `FULL OUTER JOIN` - 只要其中一个表中存在匹配，则返回行

  一张图看懂 SQL 的各种 JOIN 用法
  ![一张图看懂 SQL 的各种 JOIN 用法](./imgs/sql-join.png)
  参考链接：<https://www.runoob.com/sql/sql-join.html>

- JOIN 可以多次关联同一张表

  示例：

  ```sql
  SELECT
      example.id,
      example.`name`,
      creater.user_name,
      updater.user_name
  FROM
      example
      LEFT JOIN user AS creater ON creater.id = example.create_user_id
      LEFT JOIN user AS updater ON updater.id = example.update_user_id
  ```

## COUNT 和 GROUP BY

示例：

```sql
SELECT deptno,
count(*) 总人数,
count(CASE WHEN job ='SALESMAN' THEN 1 END) 销售人数,
count(CASE WHEN job ='MANAGER' THEN 1 END) 主管人数
FROM emp
GROUP BY deptno; --如果不 group by，会认为所有数据是一组，返回一个数据
```

参考链接：<https://www.runoob.com/sql/sql-func-count.html>
