---
description: 轻量级服务框架
---

# tornado

## JSONP

&lt;script&gt;标签的src属性并不被同源策略所约束，所以可以获取任何服务器上脚本并执行。

request不能跨域请求，而script的src文件可以跨域请求，利用这个加载策略，将数据包装进script中，从而实现跨域请求

### 同源

域名，协议，端口相同

### DEMO

#### 前端

```javascript
<script type="text/javascript">
//回调函数
function callback(data) {
    alert(data.message);
}
</script>
<script type="text/javascript" src="http://localhost:20002/test.js"></script>
```

#### js文件

```javascript
//调用callback函数，并以json数据形式作为阐述传递，完成回调
callback({message:"success"});
```

后台将数据封装成，js文件即可

[深入浅出JSONP--解决ajax跨域问题](https://www.cnblogs.com/chopper/archive/2012/03/24/2403945.html)

### tornado封装

```python
import json
from functools import wraps
 
def jsonp(func):
	"Wraps JSONfiled output for JSONP requests."
	@wraps(func)
	def decorated_func(*args, **kwargs):
		# First argument always be the requestHandler
		requestHandler = args[0]
		callback = requestHandler.get_argument('callback', False)
 
		data = json.dumps(func(*args, **kwargs))
		content = str(callback) + '(' + data + ')' if callback else data
 
		requestHandler.write(content)
		requestHandler.finish()
 
	return decorated_func



class AnalyzeDomainIp(tornado.web.RequestHandler):
	@jsonp
	def get(self, collection='todayDomain', domain=''):
		return datacenter.get_domain_detail_ip(collection, domain)
```

```javascript
$.getJSON('http://www.server.com:8081/jsonp/helloword?jsonp=?', function(data) {
              $('#get-console').html(data['josnp-get']);
          });
```

[Tornado 支持JSONP的请求封装](https://blog.csdn.net/jazywoo123/article/details/17712193)



