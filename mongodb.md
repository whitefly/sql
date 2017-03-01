```sql

use new_db  --创建新数据库

db.new_coll.insert({name:'周昂',sex:'男',hobbit:'洗澡睡觉打豆豆'}) --选择一个collect(没有自动创建)插入,key默认为字符串,可以不用打单引号

row={name:'张煦',sex:'男',hobbit:'打鬼泣',score:'优'}
db.new_coll.insert(row)  --用变量来插入

db.new_coll.find() -- 查找数据

db.new_coll.find().pretty() -- 竖着显示数据

db.new_coll.find({'name':'张煦'}) -- where 等于

db.new_coll.update({'hobbit':'洗澡睡觉打豆豆'},{$set:{'name':'赵忠祥'}}) --更新符合条件的第一条数据

db.new_coll.update({'hobbit':'洗澡睡觉打豆豆'},{$set:{'name':'赵忠祥'}},{multi:true}) --更新符合条件的全部数据

db.new_coll.update({'name':'赵忠祥'},{$set:{'english':22}})  --增加一个属性

db.new_coll.remove({'name':'周昂'}) --删除


db.new_coll.find({'english':{$gt:60}}).pretty() -- >操作

db.new_coll.find({'english':{$lt:30}}).pretty() -- <操作   

db.new_coll.find({'english':{$ne:78}}).pretty() -- !=操作

db.new_coll.find({'english':{$gte:45}}).pretty() -- >=操作
db.new_coll.find({'english':{$gt:60},'sex':'男'}).pretty()   -- and多条件查询 {}传入多个k_v 

db.new_coll.find({$or:[{'english':{$gt:60}},{'hobbit':'约炮'}]}).pretty()  --or多条件查询 $or接[],[]中一个{}只写一个条件

db.new_coll.find({'english':{$gt:30,$lt:80}})--30<english<80


db.new_coll.find({'name':{$in:['周末','张琴琴']}}) --in 操作

db.new_coll.find({'name':{$nin:['周末','张琴琴']}}) --notin 操作
```
