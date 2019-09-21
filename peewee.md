---
description: peewee那些坑
---

# Peewee

## ID

如果model没有显式申明主键，peewee会自作主张的给你加入id这个key，比如查询（select）

## 惰性执行

默认不用到的时候不会execute，select还好，后续一般会用到这些记录，会自动触发，其他的修改语句就有些爆炸了

* insert，记得execute
* update，记得execute
* delete，记得execute
* create，默认会execute不用担心

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
    preserve=[User.last_login],
    update={User.login_count: User.login_count + 1})
  .execute()
```

参考 [Upsert support](https://github.com/coleifer/peewee/issues/1307)



