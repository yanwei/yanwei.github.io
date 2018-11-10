# Mac上的brew和pip网络加速

<link rel="stylesheet" type="text/css" href="https://yanwei.github.io/auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2018-11-10"}
```

## brew

Homebrew是Mac上的包管理神器。但是由于下载源在国外，速度让人无法忍受。国内的镜像有时也不太靠谱。最简单最靠谱的方法就是用代理，brew用curl下载，所以给curl挂上socks5的代理即可。

### 准备工作

安装和配置Shadowsocks。
[传送门1](../shadowsocks/shadowsocks-server.md)，
[传送门2](../shadowsocks/shadowsocks-client.md)。

### 修改curl配置

```cmd
$ vi ~/.curlrc
```

在`~/.curlrc`文件中输入代理地址即可。如果你的shadowsocks服务端速度够快的话，brew的下载速度简直就是飞起。

```conf
socks5 = "127.0.0.1:1086"
```

## pip

对于Python开发用户来讲，PIP安装软件包是家常便饭。但国外的源下载速度实在太慢，浪费时间。而且经常出现下载后安装出错问题。所以把PIP安装源替换成国内镜像，可以大幅提升下载速度，还可以提高安装成功率。

### 国内源

* 清华：https://pypi.tuna.tsinghua.edu.cn/simple
* 阿里云：https://mirrors.aliyun.com/pypi/simple
* 中国科技大学：https://pypi.mirrors.ustc.edu.cn/simple
* 华中理工大学：http://pypi.hustunique.com
* 山东理工大学：http://pypi.sdutlinux.org
* 豆瓣：http://pypi.douban.com/simple

### 使用方法

```cmd
$ vi ~/.pip/pip.conf
```

如果目录不存在，需要先创建。在`pip.conf`文件中加入以下内容：

```conf
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
trusted-host = mirrors.aliyun.com
```

现在去试试`pip install`如飞一般的速度吧。