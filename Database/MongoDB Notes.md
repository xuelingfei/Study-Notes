支持join，但性能不高

db.<collection>.insert({_id:1001, a: "1"})

db.<collection>.insertOne({_id:1001, a: "1"})

db.<collection>.insertMany([{_id:1001, a: "1"}], {ordered: true})
插入，有序操作效率低遇错退出，无序操作性能高遇错继续执行，会返回error的id
db.<collection>.bulkWrite([{_id:1001, a: "1"}], {ordered: true})

deleteOne():删除单条文档， 满足查询条件的第一条
deleteMany():删除多条文档，满足查询条件的第一条

db.<collection>.drop()
删除集合所有的数据和索引，释放磁盘空间
比deleteMany()效率高
db.dropDatabase()
删除数据库，释放磁盘空间


_id
不支持array
支持string, int等
默认是ObjectId()

db.<collection>.find({age:{$gt: 18}}).limit(5)
返回cursor

$all完全匹配
$size数组大小
$elemMatch查询数组中至少有一个能完全匹配所有查询条件的文档

updateOne()
updateMany()

$inc自动增加
$mul乘

$push追加元素到数组最后
$pop删除数组最后的元素

索引
加速查询，但会降低写操作性能
单个集合支持最大索引个数64
将索引cache在内存中以获得更好性能
每个index都应该被查询使用，定期检查无用索引

explain(true) 查看操作执行计划

createIndex(keys, options)
options: 1.background <boolean>; 2.unique <boolean>; 3.name <string>
getIndexes()
getIndexKeys()

单属性索引
createIndex({name:1})
联合索引
createIndex({'a': 1, 'b': -1})  (fields最好不要超过32个)
    最佳实践，ESR原则，创建联合索引时，equal最前，sort中间，range最后
    find({a:100, c: {$gte: 100},})
多键索引

地理位置索引

全文索引


TTL索引
（不能建立在_id fields上）
唯一索引
部分索引
  针对部分查询需求较大，查询次数较多的数据可以建立部分索引
哈希索引
  支持equality；不支持范围查询；不支持排序


聚合操作


视图

创建视图
db.createView()





db.collection.find().sort({"_id": -1})

TypeError: if no direction is specified, key_or_list must be an instance of list

解决方法：

db.collection.find().sort([("_id", -1)])
db.collection.find().sort([("name", 1), ("age" , 1)])

原因：在python中只能使用列表进行排序，不能使用字典















