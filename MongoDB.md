# MongoDB
[一文读懂非关系型数据库](https://cloud.tencent.com/developer/article/1030365)
### 先明确几个NoSQL数据库的概念： 
```markdown
database-------------数据库
collection-----------集合（对应RDB的table表）
document-------------文档（RDB的row）
field----------------字段（RDB的column）
index----------------索引
primary key----------主键，MongoDB自动将_id字段设置为主键
```
### 然后安装MongogDB```brew install mongodb```

## 1. 启动MongoDB服务：
执行：```sudo mongod```

## 2. 链接Mongo数据库：
新开一个terminal，输入：```mongo```

## 3. 各种指令：  
### 1. 创建数据库database：
``` use DATABASE_NAME```有的话就```switched to db DATABASE_NAME```
### 2. 删除数据库：
先切换到你要删除的那个数据库：```use DATABASE_NAME```，然后执行：```db.dropDatabase()```就删除了。
### 3. 创建集合collection：
先切换到你要删除的那个数据库：```use DATABASE_NAME```，然后执行：```db.createCollection("COLLECTION_NAME， OPTIONS")```。有四种options：```capped```，```autoIndexedId```，```size```和```max```。
### 4. 删除集合：
执行：```db.COLLECTION_NAME.drop()```
### 5. 增！ 插入文档document：
说白了就是插入一条数据呗。。

```javascript
db.COLLECTION_NAME.insert({
		title:'Mongo 教程',
		by:'bruce',
		url:'www.dddd.com',
		tags:['mongo','database','nosql'],
		like:100
})
```
RDB中的```select * from TABLE_NAME```变成了```db.COLLECTION_NAME.find()```。

如果有一个JSON格式的一个变量叫```document```，我们可以直接插入collection：```db.COLLECTION_NAME.insert(document)```

### 6. 删！ 删除文档document：
```javascript
db.COLLECTION_NAME.remove(
	<query> //删除的条件
	{
		justOne:<bool>, // true只删除一个，false删除所有符合条件的
		writeConcern;<document> // 抛出异常级别
	}
)
```
删除所有的：```db.COLLECTION_NAME.remove({})```

删除特定：```db.COLLECTION_NAME.remove({'title':'Mongo 教程})```
### 7. 改！ 更新文档document：
```javascript
db.DOLLCETION_NAME.update(
	<query>,   // 条件, 类似于where
	<update>,   //
	{
		upsert:<boolean>, 
		multi:<boolean>,
		writeConcern;<document> // 抛出异常级别
	}
)
```
```db.COLLECTION_NAME.save()```通过传入文档来替换已有文档。
### 8. 查！ 查询文档document：
```db.COLLECTION_NAME.find(query, projection)```

```query```指定条件








