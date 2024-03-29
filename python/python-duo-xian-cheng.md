---
description: 并发！
---

# python 多线程

## threading.local

no bb, give me code

### threading.local

```python
from threading import Thread
from threading import local
import time
# 这是一个特殊的对象
ret = local()  # 先实例化一个对象


def task(arg):  # 写个任务，加个参数
    ret = arg  # 每一个线程进来都给他开辟一个独立的空间  单纯的threading.local()的作用就是这个
    time.sleep(2)
    print(ret)  # 每个线程来，改了ret，然后取ret的值


for i in range(10):  # i是线程的值，0 1 2 3 4 5 6 7 8 9
    t = Thread(target=task, args=(i,))  # 开10个线程
    t.start()
# 打印结果 0 3 2 5 7 9 8 4 1 6
```

### threading.get\_ident

```python
from threading import get_ident,Thread
import time

storage = {}

def set(k,v):  # 来给storage设置值
    ident = get_ident()  # get_ident()能获取唯一标识，是一组数字
    if ident in storage:
        storage[ident][k] = v
    else:
        storage[ident] = {k:v}

def get(k):  # 来取storage的值
    ident = get_ident()
    return storage[ident][k]

def task(arg):
    set('val',arg)
    v = get('val')
    print(v)

for i in range(10):
    t = Thread(target=task,args=(i,))
    t.start()
```

[多线程局部变量之threading.local\(\)用法](https://www.cnblogs.com/aaronthon/p/9457330.html)

## 单例

最佳单例方案

```python
import threading

class Singleton(object):
    _instance_lock = threading.Lock()

    def __new__(cls, *args, **kwargs):
        if not hasattr(Singleton, "_instance"):
            with Singleton._instance_lock:
                if not hasattr(Singleton, "_instance"):
                    Singleton._instance = object.__new__(cls)
        return Singleton._instance

obj1 = Singleton()
obj2 = Singleton()
print(obj1, obj2)

def task(arg):
    obj = Singleton()
    print(obj)
    
for i in range(10):
    t = threading.Thread(target=task, args=[i, ])
    t.start()
```

 [Python中的单例模式的几种实现方式的及优化](https://www.cnblogs.com/huchong/p/8244279.html)

