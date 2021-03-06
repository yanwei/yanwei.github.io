# Nginx + uWSGI + Flask + Python 简单上手指南

<link rel="stylesheet" type="text/css" href="../../auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2018-12-29"}
```

History:

* 2018-12-29: 支持Ubuntu 18.04.1 LTS。rc.local启动方式不再支持，改成systemd方式

## 说明

[Flask](http://flask.pocoo.org/)是一个轻量级的Python Web服务框架。相比Django，Flask具有简单，轻量的特点，是初学者和中小项目的好选择。

无论是开发小型Web网站，还是基于RESTFul开发应用程序接口（API），Python+Flask都是不错的解决方案。

本文基于Python3，介绍如何搭建一个简单的Web服务。文中所描述的操作在AWS虚拟主机 **Ubuntu 18.04.1 LTS** 版本上测试通过。其他操作系统版本和环境下可能略有差异。

> 注：为简化说明，本文尽可能使用各种软件的默认安装和配置，更多的性能优化和个性化配置不在本文讨论范围内。

## 安装Python3

安装Python3和pip，并把pip升级到最新：

```bash
$ sudo apt install python3 python3-pip
$ pip3 install --upgrade pip
```

检查安装是否成功：

```bash
$ python3 -V
$ pip3 -V
```

## 安装Flask

```bash
$ pip3 install flask
```

编写第一个Flask程序：

```bash
$ vi test.py
```

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

运行程序：

```bash
$ python3 test.py
```

输出如下：

```text
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

在浏览器中打开`http://127.0.0.1:5000/`即可看到`Hello World!`。

> 上述方式虽然可以启动Flask本地服务，但是性能很差，仅适合用于本地调试。生产环境中推荐用uWSGI作为服务容器。

## 安装uWSGI

### 安装

```bash
$ pip3 install uwsgi
```

> 不要用apt安装uWSGI，ubuntu上默认安装的uWSGI会启动默认的Python2

测试uWSGI启动Web应用：

```bash
$ uwsgi --socket :8080 --wsgi-file test.py --master
```

### 配置

也可以使用配置文件，免去每次启动都要输入很多参数的麻烦：

```bash
$ vi uwsgi.ini
```

```ini
[uwsgi]
socket = :8080
chdir = /var/www/html/test/
wsgi-file = test.py
callable = app
master = true
py-autoreload = 1
```

### 启动服务

```bash
$ uwsgi --ini uwsgi.ini
```

本例中的uWSGI启动了一个本地服务，在8080端口进行监听。接下来配置Nginx，把公网上80端口的请求转发到8080端口上来。

### 设置自动启动

Ubuntu 18.04中使用systemd管理系统服务（包括自启动的程序）

在systemd目录下创建`uwsgi.serivce`文件

```bash
$ sudo vi /etc/systemd/system/uwsgi.service
```

```text
[Unit]
Description=uwsgi
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/uwsgi --ini /var/www/html/test/uwsgi.ini
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

> 注意：uwsgi必须用完整路径，否则无法启动

把`uwsgi.service`改成可执行文件：

```bash
$ sudo chmod +x /etc/systemd/system/uwsgi.service
```

启动服务：

```bash
$ systemctl start uwsgi.serivce
```

加入自启动：

```bash
$ systemctl enable uwsgi.service
```

参考：https://www.jianshu.com/p/b5fa86d54685

## 安装Nginx

```bash
$ sudo apt install nginx
```

假定你用`test.com`作为服务的域名：

```bash
$ sudo vi /etc/nginx/sites-available/test.com
```

```
server {
    listen 80;
    root /var/www/html/test/;
    server_name test.com www.test.com;

    location / {
        include     uwsgi_params;
        uwsgi_pass  127.0.0.1:8080;
    }
}
```

在sites-enabled目录下创建test.com文件的软链接：

```bash
$ cd /etc/nginx/sites-enabled
$ ln -s ../sites-available/test.com
```

重新加载nginx服务配置：

```bash
$ nginx -s reload
```

最后，在你的DNS服务商把`test.com`域名指向本服务器，就可以在浏览器里测试你的第一个Web服务啦。

> 记得在防火墙或安全组配置中开放对80端口的访问哦。
