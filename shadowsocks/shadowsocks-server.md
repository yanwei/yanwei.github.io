# 科学上网指南-服务器篇

<link rel="stylesheet" type="text/css" href="../auto-number-title.css" >

```json
{"Author":"yanwei", "At":"Shanghai", "LastUpdate":"2019-9-4"}
```

## 说明

本文介绍基于Shadowsocks的解决方案。参考[Shadowsocks官网](https://shadowsocks.org/en/download/servers.html)。

本文所描述的操作在阿里云和AWS虚拟主机 **Ubuntu 16.04.3 LTS** 版本上测试通过。其他操作系统版本和环境下可能略有差异。

## 准备条件

一台境外服务器。实测阿里云海外ECS和亚马逊AWS均可以。

亚马逊AWS对新用户有1年免费试用活动，是免费翻墙的最佳选择。[传送门](https://aws.amazon.com/cn/free/)

如果选择阿里云的话，目前有一年300 RMB不到的入门配置，也是很划算的。

## 在Linux上安装

### 用apt命令直接安装

```
$ sudo apt install shadowsocks
```

### 用Python pip命令安装

安装Python：（大多Linux发行版自带了Python，可跳过）

```
$ sudo apt install python
```

安装pip：

```
$ sudo apt install python-pip
```

用pip安装Shadowsocks：

```
$ sudo pip install shadowsocks
```

## 配置

配置Shadowsocks服务：

```
$ sudo vi /etc/shadowsocks.json
```

如果文件不存在，则自动创建。在shadowsocks.json中设置Shadowsocks服务的参数，包括端口和密码等：

```
{
    "server":"0.0.0.0",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "server_port":7777,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

如果想配置多组不同的端口和密码，也是可以的：

```
{
    "server":"0.0.0.0",
    "local_address":"127.0.0.1",
    "local_port":1080,
    "port_password":{
        "7777":"password1",
        "8888":"password2"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

> 注：最近GFW似乎能识别用aes-256-cfb加密的数据包并墙掉相应的端口，这种情况下可以试试其他加密方式。客户端配置需要注意和服务器保持一致。

## 防火墙

阿里云和AWS都有安全组策略，不要忘记把上一步配置的端口加到安全组的公网入站规则中去。Shadowsocks用的是TCP连接，配置的时候注意下。具体操作方式参考两个平台的文档。

## 启动和停止Shadowsocks服务

```
$ ssserver -c /etc/shadowsocks.json -d start
$ ssserver -c /etc/shadowsocks.json -d stop
```

## 设置自动启动

```
$ sudo vi /etc/rc.local
```

增加一行或改成如下内容：

```
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

ssserver -c /etc/shadowsocks.json -d start

exit 0
```

> 从Ubuntu 18.04版本开始，rc.local文件默认不存在。`sudo vi /etc/rc.local`会创建一个新文件。需要增加文件的可执行权限，重启后才能生效。

```
$ sudo chmod +x /etc/rc.local
```

重新启动服务器即可。
