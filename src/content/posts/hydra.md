---
title: hydra使用示例
published: 2025-10-05
description: '使用hydra进行密码爆破'
image: ''
tags: ["Hydra", "Password"]
category: '网络安全'
draft: false 
lang: ''
pinned: false
author: Mikuas
---

#### Hydra
hydra 的基本命令格式
~~~bash
hydra [options] server://IP/URL
~~~
## 基本参数
* #### `-L <file>`：指定一个包含用户名列表的文件。
* #### `-l <username>`：指定一个用户名进行攻击。
* #### `-P <file>`：指定一个包含密码列表的文件。
* ####  `-p <password>`：指定一个密码进行攻击。
* ####  `-t <number>`：指定尝试的线程数（默认是 16）。
* #### `-s <port>`：指定目标的端口（如果使用的端口不是默认端口）。
* ####  `-v <level>`：指定输出的详细程度，值为 1-3，越大输出越详细。
    * ##### `1`：普通输出。
    * ##### `2`：更多详细信息。
    * ##### `3`：最大详细输出。
* #### `-o <file>`：将结果保存到文件。
* #### `-f`：每当破解成功时，立即停止攻击。
* #### `-x`：指定代理（如 SOCKS 代理）。

## 目标和协议参数

* #### `http-get://<ip>`：指定 HTTP GET 请求的目标。
* #### `http-post-form`：指定 HTTP POST 表单攻击，格式为 `"/path:field1=value1&field2=value2:F=failed"`。
* #### `ssh://<ip>`：指定 SSH 协议攻击。
* #### `ftp://<ip>`：指定 FTP 协议攻击。
* #### `smtp://<ip>`：指定 SMTP 协议攻击。
* #### `pop3://<ip>`：指定 POP3 协议攻击。
* #### `imaps://<ip>`：指定 IMAPS 协议攻击。
* #### `rdp://<ip>`：指定 RDP 协议攻击。
* #### `vnc://<ip>`：指定 VNC 协议攻击。
* #### `mysql://<ip>`：指定 MySQL 协议攻击。
* #### `postgres://<ip>`：指定 PostgreSQL 协议攻击。
* #### `smb://<ip>`：指定 SMB 协议攻击。

## 高级选项

* #### `-F`：指定使用“失败”标志来识别登录失败的情况。用于 HTTP 表单攻击。
* #### `-u`：为目标指定一个 URL（例如 HTTP 网站）以供攻击。
* #### `-I <file>`：指定输入文件（字典文件）。
* #### `-P <file>`：指定密码文件（字典文件）。
* #### `-l <username>`：指定一个单独的用户名进行攻击。
* #### `-p <password>`：指定一个单独的密码进行攻击。
* #### `-t <threads>`：指定并行线程数，默认是 16。
* #### `-S`：开启 SSL/TLS 加密连接。
* #### `-vV`：显示所有请求和响应的详细信息（调试模式）。


## 常用语法
~~~bash
# 指定服务和目标
hydra -l <userName> -p <password> <IP> <server>

# 对FTP服务进行单用户密码爆破
hydra -l admin -p 123456 192.168.1.1 ftp

# 使用用户名和密码字典
hydra -L <userNameDict> -P <passwordDict> <IP> <server>
~~~

## SSH爆破
~~~bash
hydra -L <userNameDict> -P <passwordDict> ssh@IP
# 指定端口
hydra -s PORT -L <userNameDict> -P <passwordDict> ssh://IP
~~~

## HTTP登录页面爆破
~~~bash
# HTTP GET
hydra -L <userNameDict> p <passwordDict> http-get://IP/

# HTTP POST 表单
hydra -L <userNameDict> -P <passwordDict> <IP> http-post-form "/login:name=^USER^&password=^PASS^:F=Login failed"

# 使用代理 添加
-x <socks5://proxy>

# 设置线程
-t <number>
~~~

## Telnet爆破
~~~bash
hydra -L <userNameDict> -P <passwordDict> telnet://IP
~~~