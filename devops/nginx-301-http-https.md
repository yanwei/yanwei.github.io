# Nginx利用301实现http自动跳转到https

<link rel="stylesheet" type="text/css" href="../auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2018-01-24"}
```

80端口访问时，直接返回301重定向到https链接即可。Nginx配置示例如下：

```nginx
server {
    listen 443 ssl;
    server_name example.com;

    ssl on;
    ssl_certificate /etc/nginx/sslkey/your-ssl.pem;
    ssl_certificate_key /etc/nginx/sslkey/your-ssl.key;
    ...
}

server {
    listen 80;
    server_name www.example.com example.com;
    return 301 https://example.com$request_uri;
}
```