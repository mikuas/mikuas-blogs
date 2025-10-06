---
title: 2024年江西省职业院校技能竞赛A模块Linux解析
published: 2024-09-26
description: '配置Nginx安全策略, 防护墙策略'
image: ''
tags: ["网络安全", "Nginx", "Firewalld"]
category: '网络安全'
draft: true 
lang: ''
pinned: false
author: Mikuas
---

1. 密码策略
```bash
# 编辑 nano /etc/security/pwquality.conf
# 修改:
minlen = 8
dcredit = -1
ucredit = -1
lcredit = -1
ocredit = -1

# 如果有 取消前面的注释即可

# 或者编辑 nano /etc/pam.d/system-auth
# 添加或修改下行:
password requisite pam_pwquality.so retry=3 minlen=8 dcredit=-1 ucredit=-1 lcredit=-1 ocredit=-1

# 解释
1. minlen: 设置密码的最小长度
2. dcredit: 需要数字字符的数量(负值)
3. ucredit: 需要大写字母的数量(负值)
4. lcredit: 需要小写字母的数量(负值)
5. ocredit: 需要特殊字符的数量(负值)
6. enforce_for_root: 确保即使是root用户设置密码，也应强制执行复杂性策略。
7. minclass=3至少包含小写字母、大写字母、数字、特殊字符等4类字符中等3类
```

## Nginx安全策略
### 安装Nginx
```bash
# 安装nginx[通过EPEL 仓库]

# 更新系统
sudo yum update -y
```
### 安装EPEL仓库
```bash
sudo yum install epel-release -y
```
### 安装nginx
```bash
sudo yum install nginx -y
```

### 安装nginx[通过官方 Nginx 仓库安装]
```bash
# 更新系统
sudo yum update -y

# 添加官方Nginx仓库

sudo nano /etc/yum.repos.d/nginx.repo

# 添加以下内容
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```
### 导入Nginx GPG密钥
```bash
sudo rpm --import https://nginx.org/keys/nginx_signing.key
```
### 安装Nginx
```bash
sudo yum install nginx -y
```
### 1. 禁止目录浏览和隐藏服务器版本和信息显示
```bash
# 修改文件
nano /etc/nginx/nginx.conf
在`server`块或`location`块中,确保autoindex设置为off
server {
    listen 80;
    server_name example.com;
    root /usr/share/nginx/html;

    location / {
        autoindex off; # 禁止目录浏览
    }
    # 隐藏服务器版本信息
    server_token off;
}
```

### 2. 限制HTTP请求方式，只允许GET、HEAD、POST
```bash
# 在location 中添加:
limit_except GET HEAD POST {
        deny all; # 拒绝其他 HTTP 方法
}
# 或者添加
if ($request_mothod !~^(GET|HEAD|POST)$) {
    return 403; 
}

```

### 3. 设置客户端请求主体读取超时时间为10
```bash
# 在 http 里添加:
http {
    ...
    client_body_timeout 10s; # 设置客户端请求主体读取超时时间为10秒
    ...
}
```

### 4. 设置客户端请求头读取超时时间为10
```bash
# 在 http 里添加:
http {
    ...
    client_header_timeout 10s; # 设置客户端请求主体读取超时时间为10秒
    ...
}
```

### 5. 将Nginx服务降权，使用www用户启动服务
```bash
# 如果没有www用户就创建
sudo useradd  www
# 修改Nginx配置文件(/etc/nginx/nginx.conf)

# 把顶部的user 改为 www 用户
user www;

```

## 中间件服务加固SSHD\VSFTPD SSH服务加固

### 1. 修改ssh服务端口为2222
```bash
# 编辑/etc/ssh/sshd_config文件

# 把 #Port 22 修改为 Port 2222 重启sshd服务

# 如过不能重启把 /etc/selinux/config 里的SELINUX设置为 SELINUX=disable 

# 还不行就重启系统, 重启前ssh服务需开机自启

# 验证
sudo netstat -anltp | grep 2222

```

### 2. ssh禁止root用户远程登录
```bash
# 找到 PermitRootLogin 改为 no 有注释取消注释
```

### 3. 设置root用户的计划任务, 每天早上7:50自动开启ssh服务, 22:50关闭,每周六的7:30重新启动ssh服务
```bash
# crontab -e 添加:
50 7 * * * systemctl start sshd # 每天 7:50 启动 SSH 服务

50 22 * * * systemctl stop sshd # 每天 22:50 关闭 SSH 服务

30 7 * * 6 systemctl restart sshd # 每周六 7:30 重新启动 SSH 服务

# 查看
crontab -l
```

### 4. 修改SSHD的PID档案存放地
```bash
# 找到 PidFile /var/run/sshd.pid 取消前面的注释(vi|vim的/PidFile可快速查找)
```

## VSFTPD服务加固

### 1. 设置运行vsftpd的非特权系统用户为pyftp
```bash
# 编辑 /etc/vsftpd/vsftpd.conf

# 找到 nopriv_user=ftpsecure 改为 pyftp
```

### 2. 限制客户端连接的端口范围在50000-60000
```bash
# 找到或在文件底部添加:
pasv_min_port=50000 # 被动模式使用的最小端口号
pasv_max_port=60000 # 被动模式使用的最大端口号
```

### 3. 限制本地用户登录活动范围限制在home目录
```bash
# 找到chroot_local_user 改为YES 有注释取消注释
```

## 防火墙策略
### 1. 设置防火墙允许本机转发除ICMP协议以外的所有数据包
```bash
# 允许转发所有流量
iptables -A FORWARD -p all -j ACCEPT

# 阻止 ICMP 协议的转发
iptables -A FORWARD -p icmp -j DROP
```

### 2. 为防止SSH服务被暴力枚举，设置iptables防火墙策略仅允许172.16.10.0/24网段内的主机通过SSH连接本机；
```bash
# 允许来自 172.16.10.0/24 网段的 SSH 连接
iptables -A INPUT -p tcp -s 172.16.10.0/24 --dport 22 -j ACCEPT

# 拒绝所有其他 SSH 连接
iptables -A INPUT -p tcp --dport 22 -j DROP
```

### 3. 为防御拒绝服务攻击，设置iptables防火墙策略对传入的流量进行过滤，限制每分钟允许3个包传入，并将瞬间流量设定为一次最多处理6个数据包（超过上限的网络数据包将丢弃不予处理）
```bash
# 允许每分钟最多 3 个包传入
iptables -A INPUT -p tcp -m limit --limit 3/minute --limit-burst 6 -j ACCEPT

# --limit 3/minute：限制每分钟最多 3 个包
# --limit-burst 6：瞬时流量最大处理 6 个数据包

# 丢弃超过限制的包
iptables -A INPUT -p tcp -j DROP
```

### 4. 只允许转发来自172.16.0.0/24局域网段的DNS解析请求数据包。
```bash
# 允许来自 172.16.0.0/24 网段的 DNS 请求
iptables -A INPUT -p udp -s 172.16.0.0/24 --dport 53 -j ACCEPT

# 拒绝所有其他的 DNS 请求
iptables -A INPUT -p udp --dport 53 -j DROP
```
0：突发限制参数，允许在短时间内处理的数据包数量超过平均速率限制（这里同样是 1000 个数据包）。这意味着在一开始，连续的 1000 个数据包可以被接受，直到达到突发值限制。之后，只有符合设定的平均速率（这里是 1000 个/秒）的数据包才会被接受
