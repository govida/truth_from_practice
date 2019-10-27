---
description: peewee那些坑
---

# Peewee

## Select & Primary\_key

如果model没有显式申明主键，peewee会自作主张的给你加入id这个key

> peewee.InternalError: \(1054, u"Unknown column 't1.id' in 'field list'"\)

## 创建记录

### 三种便捷创建

* create：会创建实例，不需要execute
* save：会创建实例，不需要execute，但需要额外save，创建新记录方面劣于create
* insert：不会创建额外实例，需要execute，返回刚刚插入的主键

### 批量创建

```python
from peewee import chunked
with mysql_db.atomic():    # 官档建议用事务包裹
    for batch in chunked(data, 100):    # 一次100条， chunked() 返回的是可迭代对象
        Owner.insert_many(batch).execute()
```

## 查询

#### 单条记录

* get：找不到，会报错
* get\_or\_create：找不到，会自动创建
* get\_or\_none：找不到，会安全返回none

## 惰性执行

默认不用到的时候不会execute，select还好，后续一般会用到这些记录，会自动触发，其他的修改语句就有些爆炸了

* insert，记得execute
* update，记得execute
* delete，记得execute

## print

写完后可以print一下具体的sql长什么样，帮助Debug

## 动态生成or

```python
import operator
clauses = [
    (User.username == 'something'),
    (User.something == 'another thing'),
    ...
]
User.select().where(reduce(operator.or_, clauses))
```

这不应该算是peewee特性，应该是python特性，暂时还没深入了解

[绿蚁酒](https://stackoverflow.com/questions/22238194/python-peewee-dynamically-or-clauses)

## 事务管理

db.atomic\(\)

[peewee 事务](https://www.cnblogs.com/miaojiyao/articles/5235738.html)，差点意思不展开搞了

## on\_conflict & Upsert

原型

```sql
# update
INSERT INTO table (a,b,c) VALUES (1,2,3)
  ON DUPLICATE KEY UPDATE c=c+1;
# preserve
INSERT INTO table (a,b,c) VALUES (1, 2, 3)
   ON DUPLICATE KEY UPDATE b = VALUES(b), c = VALUES(c)
```

demo

```python
User
  .insert(username=username, last_login=now, login_count=1)
  .on_conflict(
    conflict_target=[User.username],
    preserve=[User.last_login],                       # 若冲突，你想保留不变的字段
    update={User.login_count: User.login_count + 1})  # 若冲突，你想更新什么
  .execute()
# 注： preserve 和 update 按情况用，一般设置一个用就行了。
```

参考 [Upsert support](https://github.com/coleifer/peewee/issues/1307)

## peewee.ImproperlyConfigured: MySQL driver not installed!

> pip install pymysql

peewee不会自动的装载mysql driver...

参考引用：

* [PY =&gt; Python-ORM之peewee：CRUD完整解析（二）](https://segmentfault.com/a/1190000020265522)



