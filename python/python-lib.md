---
description: 优雅的python包
---

# Python Lib

## tenacity

用于重试逻辑，异常强大

```python
from tenacity import retry
from json.decoder import JSONDecodeError
# retry 特定错重试
# wait 等待几秒后重试
# stop 最大重试几次
@retry(retry=retry_if_exception_type(JSONDecodeError), wait=wait_fixed(5), stop=stop_after_attempt(3))
def extract(url):
    info_json = requests.get(url).content.decode()
    info_dict = json.loads(info_json)
    data = info_dict['data']
    save(data)
```

## retrying

然而弱鸡的我，选择用retrying模块

```python
from retrying import retry
@retry(stop_max_attempt_number=3)
```

## contextmanager

用来生成便携的with，多用于资源的获取与释放（连接池）

```python
@contextmanager
def pushd(new_dir):
    origin_dir = os.getcwd()
    os.chdir(new_dir)
    try:
        yield
    # except OSError, err:
    #     print 'ERROR: ', err
    # except RuntimeError, err:
    #     print 'ERROR: ', err
    finally:
        print 'changing back'
        os.chdir(origin_dir)


with pushd('/tmp'):
    print os.getcwd()
    raise RuntimeError
print os.getcwd()
```

资料参考

* [关于ContextManager](https://www.jianshu.com/p/4c89e2bf5145)
* [python中yield的用法详解——最简单，最清晰的解释](https://blog.csdn.net/mieleizhi0522/article/details/82142856)

