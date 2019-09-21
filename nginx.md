---
description: 服务器小白
---

# nginx

## 安装

[linux环境下安装nginx步骤](https://www.cnblogs.com/wyd168/p/6636529.html)，差点意思~

## \[error\] invalid PID number "" in "/usr/local/nginx/logs/nginx.pid"

没初始化config

> /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

## 配置https

```bash
server {
    listen              443 ssl;
    server_name         www.example.com;
    ssl_certificate     www.example.com.chained.crt;
    ssl_certificate_key www.example.com.key;
}
```

[nginx配置https](https://www.cnblogs.com/zzdylan/p/8878227.html)

国内域名巨坑，现在都得备案才能用。。

