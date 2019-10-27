---
description: 常识性不足
---

# Python 坑

## datetime.datetime.today\(\)

本想获得今天，发现下面有个today就用了，后期发现，此today等价于now

应该用datetime.date.today\(\)

```python
import datetime

print datetime.date.today()
print datetime.datetime.today()
print datetime.datetime.now()

#2019-09-04
#2019-09-04 00:12:32.559675
#2019-09-04 00:12:32.559686
```

## 迭代器

迭代器只能使用一次，用后即废，指针到达了末尾

```python
def test():
    for i in range(3):
        yield i

nums=test()

print 'first iteration'
for i in nums:
    print i

print 'second iteration'
for i in nums:
    print i

#first iteration
#0
#1
#2
#second iteration
```

## 队列

python队列是线程安全的，无需自行加锁了，这就给我们很多操作的空间

PS：happybase 内部连接池用了队列，但还加的锁，就感觉设计者有点笨，或者是我没考虑到？

