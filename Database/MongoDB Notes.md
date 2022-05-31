# MongoDB Notes

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [安装 MongoDB](#安装-mongodb)

</details>

## 安装 MongoDB

1. 下载，官网下载地址为 <https://www.mongodb.com/try/download/community>
2. 安装，可选择 "Custom" 选项来自定义安装目录
3. 配置环境变量 `D:\MongoDB\bin`

## 创建数据库和集合

支持 join，但性能不高

db.<collection>.insert({\_id:1001, a: "1"})

db.<collection>.insertOne({\_id:1001, a: "1"})

db.<collection>.insertMany([{_id:1001, a: "1"}], {ordered: true})
插入，有序操作效率低遇错退出，无序操作性能高遇错继续执行，会返回 error 的 id
db.<collection>.bulkWrite([{_id:1001, a: "1"}], {ordered: true})

deleteOne():删除单条文档， 满足查询条件的第一条
deleteMany():删除多条文档，满足查询条件的第一条

db.<collection>.drop()
删除集合所有的数据和索引，释放磁盘空间
比 deleteMany()效率高
db.dropDatabase()
删除数据库，释放磁盘空间

\_id
不支持 array
支持 string, int 等
默认是 ObjectId()

db.<collection>.find({age:{$gt: 18}}).limit(5)
返回 cursor

$all完全匹配
$size 数组大小
$elemMatch 查询数组中至少有一个能完全匹配所有查询条件的文档

updateOne()
updateMany()


索引
加速查询，但会降低写操作性能
单个集合支持最大索引个数 64
将索引 cache 在内存中以获得更好性能
每个 index 都应该被查询使用，定期检查无用索引

explain(true) 查看操作执行计划

createIndex(keys, options)
options: 1.background <boolean>; 2.unique <boolean>; 3.name <string>
getIndexes()
getIndexKeys()

单属性索引
createIndex({name:1})
联合索引
createIndex({'a': 1, 'b': -1}) (fields 最好不要超过 32 个)
最佳实践，ESR 原则，创建联合索引时，equal 最前，sort 中间，range 最后



TTL 索引
（不能建立在\_id fields 上）
唯一索引
部分索引
针对部分查询需求较大，查询次数较多的数据可以建立部分索引
哈希索引
支持 equality；不支持范围查询；不支持排序



$inc自动增加
$mul 乘

$push追加元素到数组最后
$pop 删除数组最后的元素


常用管道

    $group：将集合中的文档分组，可用于统计结果
    $match：过滤数据，只输出符合条件的文档
    $project：修改输入文档的结构，如重命名、增加、删除字段、创建计算结果
    $sort：将输入文档排序后输出
    $limit：限制聚合管道返回的文档数
    $skip：跳过指定数量的文档，并返回余下的文档

常用表达式

    $sum：计算总和，$sum:1同count表示计数
    $avg：计算平均值
    $min：获取最小值
    $max：获取最大值
    $push：在结果文档中插入值到一个数组中
    $first：根据资源文档的排序获取第一个文档数据
    $last：根据资源文档的排序获取最后一个文档数据