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

## 







