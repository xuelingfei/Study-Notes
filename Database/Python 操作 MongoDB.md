# Python 操作 MongoDB

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [Python 通过 PyMongo 模块连接 MongoDB](#python-通过-pymongo-模块连接-mongodb)

</details>

## Python 通过 PyMongo 模块连接 MongoDB

1. 安装 PyMongo

   `python -m pip install PyMongo` 或 `pip install pymongo`

2. 连接 MongoDB

   - 创建连接

     ```py
     from pymongo import MongoClient

     client = MongoClient('localhost', 27017)
     ```

     或者使用 MongoDB URI 格式

     ```py
     client = MongoClient('mongodb://localhost:27017/')
     ```

   - 创建或连接一个数据库

     使用要创建的数据库的名称连接 MongoClient，如果数据库不存在，MongoDB 将创建数据库并建立连接。

     - 属性风格
       ```py
       db = client.test_database
       ```
     - 字典风格
       ```py
       db = client['test-database']
       ```

     在 MongoDB 中，数据库在获取内容之前不会创建，在实际创建数据库（和集合）之前，MongoDB 会一直等待您创建至少有一个文档（记录）的集合（表）。可以通过列出系统中的所有数据库来检查数据库是否存在。

     ```py
     print(client.list_database_names())
     ```

   - 创建或获取一个集合

     使用要创建的集合的名称连接数据库对象，如果集合不存在，MongoDB 会创建该集合。

     - 属性风格
       ```py
       collection = db.test_collection
       ```
     - 字典风格
       ```py
       collection = client['test-collection']
       ```

     在 MongoDB 中，集合在获得内容之前不会被创建。可以通过列出所有集合来检查数据库中集合是否存在。

     ```py
     print(db.list_collection_names())
     ```

   - 删除集合或数据库
     - 删除集合用 `drop()` 方法，如果删除成功返回 true，如果删除失败（集合不存在）则返回 false。
       ```py
       collection.drop()
       ```
     - 删除数据库用 `drop_database()` 方法
       ```py
       client.drop_database()
       ```

3. CURD

   - 新增

     - 插入单个文档用 `insert_one()`，该方法返回 InsertOneResult 对象，拥有属性 `inserted_id`，用于保存插入文档的 id。如果您没有指定 `_id` 字段，那么 MongoDB 将为您添加一个，并为每个文档分配一个唯一的 ID。
       ```py
       x = collection.insert_one({'name': 'Tom'})
       print(x.inserted_id)
       ```
     - 插入多个文档用 `insert_many()`，该方法返回 InsertManyResult 对象，该对象拥有属性 `inserted_ids`，用于保存插入文档的 id 列表。
       ```py
       x = collection.insert_many([{'name': 'John'}, {'name': 'Jack'}])
       print(x.inserted_ids)
       ```

   - 更新

     - 更新单个文档用 `update_one()`，该方法第一个参数是筛选条件，用于定位要更新的文档；第二个参数是定义文档新值的对象。如果筛选到多个文档，则仅更新第一个匹配项。
       ```py
       collection.update_one({'name': 'Jack'}, {'$set': {'age': '5'}})
       ```
     - 更新多个文档用 `update_many()`，该方法可以更新符合筛选条件的所有文档。例如，更新名字以字母 "J" 开头的所有文档
       ```py
       x = collection.update_many({'name': {'$regex': '^J'}}, {'$set': {'age': '6'}})
       print(x.modified_count, 'documents updated.')
       ```

   - 删除

     - 删除单个文档用 `delete_one()`，该方法可以删除符合筛选条件的第一个文档。
       ```py
       collection.delete_one({'name': 'Tom'})
       ```
     - 删除多个文档用 `delete_many()`，该方法可以删除符合筛选条件的所有文档。例如，删除名字以字母 "J" 开头的所有文档
       ```py
       x = collection.update_many({'name': {'$regex': '^J'}})
       print(x.deleted_count, 'documents deleted.')
       ```
       要删除集合中的所有文档，请把空的查询对象传递给 `delete_many()` 方法：
       ```py
       x = collection.update_many({})
       print(x.deleted_count, 'documents deleted.')
       ```

   - 查询

     - 查询单个文档用 `find_one()`，返回符合筛选条件的第一个文档。

     ```py
     collection.find_one({'name': 'Tom'})
     ```

     - 查询多个文档用 `find()`，返回符合筛选条件的所有文档。  
       `find()` 方法第一个参数是筛选条件，不传或传 None 或传空字典时返回集合中的所有文档。
       ```py
       collection.find({'name': {'$regex': '^J'}})
       ```
       `find()` 方法的第二个参数是描述包含在结果中的字段的对象，可以使结果只返回某些字段。此参数是可选的，如果省略，则所有字段都将包含在结果中。此参数不允许在同一对象中同时指定 `0` 和 `1` 值（除非其中一个字段是 `_id` 字段）。如果指定值为 `0` 的字段，则所有其他字段的值为 `1`，反之亦然。`0` 和 `1` 可替换为 `False` 和 `True`。
       ```py
       collection.find({}, {'_id': 0, 'name': 1, 'age': 1})
       collection.find({}, {'name': 1, 'age': 0})  # 报错
       ```

   - 高级查询

     `find()` 方法的第一个参数是 query 对象，如需进行高级查询，可以使用修饰符作为查询对象中的值。

     | 修饰符    | 含义                                                     |
     | --------- | -------------------------------------------------------- |
     | `$gt`     | 大于                                                     |
     | `$gte`    | 大于等于                                                 |
     | `$lt`     | 小于                                                     |
     | `$lte`    | 小于等于                                                 |
     | `$ne`     | 不等于                                                   |
     | `$not`    | 筛选条件 非                                              |
     | `$and`    | 数组中的筛选条件 与                                      |
     | `$or`     | 数组中的筛选条件 或                                      |
     | `$nor`    | 数组中的筛选条件 都不                                    |
     | `$in`     | 该字段在指定数组范围内                                   |
     | `$nin`    | 不在范围内                                               |
     | `$all`    | 指定数组在该字段范围内                                   |
     | `$size`   | 对应数组字段的长度                                       |
     | `$slice`  | 截取数组字段的前几项（用在 `find()` 方法的第二个参数中） |
     | `$regex`  | 正则表达式                                               |
     | `$exists` | 属性是否存在                                             |
     | `$type`   | 数据类型判断                                             |
     | `$mod`    | 数字模操作                                               |
     | `$where`  | 以 JavaScript 表达式或函数作为查询语句（比常规查询慢）   |

     部分示例如下

     ```py
     collection.find({'interest': {'$size': 2}})  # 查询只有两个兴趣的人
     collection.find({}, {'_id': 0, 'name': 1, 'interest': {'$slice': 2}})  # 查询每个人兴趣的前两项
     collection.find({}, {'_id': 0, 'name': 1, 'interest': {'$slice': -1}})  # 查询每个人兴趣的最后一项
     collection.find({'age': {'$exists': True}})  # age 属性存在
     collection.find({'age': {'$type': 'int'}})  # age 类型为 int
     collection.find({'age': {'$mod': [5, 0]}})  # age 模 5 余 0
     collection.find({'$where': "this.age > 1"})  # 年龄大于 1
     ```

   - 排序 `sort()`

     ```py
     import pymongo

     collection.find().sort('age')  # 同 sort('age', 1)
     collection.find().sort('age', 1)  # 升序（从小到大）
     collection.find().sort('age', pymongo.ASCENDING)  # 升序
     collection.find().sort('age', -1)  # 降序（从大到小）
     collection.find().sort('age', pymongo.DESCENDING)  # 降序
     collection.find().sort([('age', 1), ('name', 1)])  # 多字段排序
     ```

   - 计数 `count()`

     ```py
     collection.find().count()  # 统计所有数据条数
     ```

   - 限定条数 `limit()` 和偏移 `skip()`

     `limit()` 方法只接收一个数字参数，限制返回文档的数目；`skip()` 方法也只接收一个数字参数，表示偏移位置，即忽略指定数目的记录。可用 `limit()` 方法和 `skip()` 方法实现分页。在数据库数量非常庞大的时候，如千万、亿级别，最好不要使用大的偏移量来查询数据，因为这样很可能导致内存溢出。

     ```py
     collection.find().limit(10).skip(10)
     ```

4. 索引 `index`

   - 创建索引

     - 创建单个索引用 `create_index()`，该方法接收一个字段名字符串或一个二元元组构成的列表作为参数，元组第一项为要创建的索引字段名，第二项指定升序（`1` 或 `pymongo.ASCENDING`）还是降序（`-1` 或 `pymongo.DESCENDING`），通过多个元组构成列表作为参数使该方法可使用多个字段创建索引。该方法的返回值是创建的索引名称。  
       在使用 `create_index()` 创建索引时，也可指定特定的参数(options)，常用可选参数如下：

       | 参数名               | 类型    | 描述                                                                                                                                                                  |
       | -------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
       | `name`               | String  | 索引名称。如果未指定，MongoDB 会通过连接索引的字段名和排序方式生成一个索引名称。例如 `create_index([('x', -1)]` 在不指定 name 时会生成默认的索引名称 `x_-1`           |
       | `background`         | Boolean | 建索引过程会阻塞其它数据库操作，`background` 可指定以后台方式创建索引。默认值为 `False`。                                                                             |
       | `unique`             | Boolean | 建立的索引是否唯一。指定为 `True` 时创建唯一性约束索引。默认值为 `False`。默认情况下，MongoDB 在创建集合时会生成唯一索引字段 `_id`。                                  |
       | `expireAfterSeconds` | Integer | 用于创建过期 (TTL) 集合，指定一个以秒为单位的数值来设定集合的生存时间。设定时间后，MongoDB 将自动从该集合中删除文档。 索引字段必须是 UTC 日期时间，否则数据不会过期。 |

       示例：

       ```py
       collection.create_index('age')
       collection.create_index([('name', 1), ('age', -1)])
       collection.create_index([('age', pymongo.ASCENDING)], background=True)
       ```

     - 创建多个索引用 `create_indexes()`，该方法接收一个由 IndexModel 实例组成的列表作为参数，返回创建的索引名称列表。示例如下：

       ```py
       >>> from pymongo import IndexModel, ASCENDING, DESCENDING
       >>> index1 = IndexModel([('hello', DESCENDING),
       ...                      ('world', ASCENDING)], name='hello_world')
       >>> index2 = IndexModel([('goodbye', DESCENDING)])
       >>> db.test.create_indexes([index1, index2])
       ["hello_world", "goodbye_-1"]
       ```

   - 查看索引信息

     查看索引信息可以用 `list_indexes()` 或 `index_information()`，前者返回一个此集合的索引文档的游标，后者返回一个包含此集合的索引信息的字典。示例如下

     ```py
     >>> print(type(collection.list_indexes()))
     <class 'pymongo.command_cursor.CommandCursor'>
     >>> for index in collection.list_indexes():
     ...     print(index)
     ...
     SON([('v', 2), ('key', SON([('_id', 1)])), ('name', '_id_')])
     SON([('v', 2), ('key', SON([('age', 1)])), ('name', 'age_1')])
     SON([('v', 2), ('key', SON([('name', -1)])), ('name', 'name_-1')])
     >>> print(type(collection.index_information()))
     <class 'dict'>
     >>> print(collection.index_information())
     {'_id_': {'v': 2, 'key': [('_id', 1)]}, 'age_1': {'v': 2, 'key': [('age', 1)]}, 'name_-1': {'v': 2, 'key': [('name', -1)]}}
     ```

5. 聚合 `aggregate()`

   1. 描述

      执行数据库级聚合。

   2. 语法

      ```py
      db.<collection>.aggregate(pipeline: Sequence[Mapping[str, Any]], session: Optional[ClientSession] = None, **kwargs: Any)
      ```

   3. 参数

      - `pipeline` -- 聚合管道阶段的列表。聚合管道参见 [MongoDB Notes.md](mongodb-notesmd)
      - `session` (可选) -- 一个 ClientSession。
      - `**kwargs` (可选) -- 额外的聚合命令参数。

      所有可选的聚合命令参数都应作为关键字参数传递给此方法。有效选项包括但不限于：

      - `allowDiskUse` (bool) -- 允许写入临时文件。当设置为 `True` 时，聚合阶段可以将数据写入 `–dbpath` 目录的 `_tmp` 子目录。默认值为 `False`。
      - `maxTimeMS` (int) -- 允许操作运行的最长时间，以毫秒为单位。
      - `batchSize` (int) -- 每批返回的最大文档数。如果连接的 mongod 或 mongos 不支持使用游标返回聚合结果，则忽略。
      - `collation` (可选) -- Collation 的一个实例。

   4. 返回值

      关于结果集的 CommandCursor( `pymongo.command_cursor.CommandCursor` )。

   5. 示例

      插入一些实例数据，以便执行聚合。

      ```py
      result = db.test.insert_many([{"x": 1, "tags": ["dog", "cat"]},
                                    {"x": 2, "tags": ["cat"]},
                                    {"x": 2, "tags": ["mouse", "cat", "dog"]},
                                    {"x": 3, "tags": []}])
      print(result.inserted_ids)
      ```

      执行一个简单的聚合来计算标签数组中每个标签在整个集合中出现的次数。 为了实现这一点，我们需要将三个操作传递给管道。 首先，我们需要展开标签数组，然后按标签分组并求和，最后按计数排序。

      ```py
      >>> from bson.son import SON
      >>> pipeline = [
      ...     {"$unwind": "$tags"},
      ...     {"$group": {"_id": "$tags", "count": {"$sum": 1}}},
      ...     {"$sort": SON([("count", -1), ("_id", -1)])}
      ... ]
      ...
      >>> import pprint
      >>> pprint.pprint(list(db.things.aggregate(pipeline)))
      [{'_id': 'cat', 'count': 3},
      {'_id': 'dog', 'count': 2},
      {'_id': 'mouse', 'count': 1}]
      ```

      要为此聚合运行解释计划，可以使用 command() 方法：

      ```py
      >>> db.command('aggregate', 'things', pipeline=pipeline, explain=True)
      {'ok': 1.0, 'stages': [...]}
      ```
