---
title: 如何在Linux(CentOS)下安装Niginx和MySql
published: 2025-06-05
description: '在CentOS下安装Nginx和MySql'
image: 'guide/d5.webp'
tags: ["Linux", "Nginx", "MySql"]
category: 'Linux'
draft: false 
lang: ''
pinned: false
author: Mikuas
---

## 安装Nginx
### 安装nginx[通过EPEL 仓库]
```bash
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
# sudo nano /etc/yum.repos.d/nginx.repo
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
# 安装Nginx
sudo yum install nginx -y
```

## 安装MySql 5.7
```bash
# 更新系统包
sudo yum update

# 配置yum仓库
# 更新密钥
rpm -import https: /repo.mysql.com/RPM-GPG-KEYmysql-2023
```
### 安装Mysql yum库
```bash
rpm -Uvh http: /repo.mysql.com /mysql57-communityrelease-el7-7.noarch.rpm
```
### yum安装Mysql
```bash
yum -y install mysql-community-server
systemctl start mysqld # 启动
systemctl enable mysqld # 开机自启

# 通过grep命令，在/var/log/mysqld.log文件中，过滤temporary password关键字，得到初始密码
grep 'temporary password' /var/log/mysqld.log

# 如果没有密码就启动mysql服务重试
```

###  登陆MySQL数据库系统
```bash
mysql -uroot -p
# 解释
# -u，登陆的用户，MySQL数据库的管理员用户同Linux一样，是
root
# -p，表示使用密码登陆
# 执行完毕后输入刚刚得到的初始密码，即可进入MySQL数据库
```

### 更改root密码
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';
-- 密码需要符合：大于8位，有大写字母，有特殊符号，不能是连续的简单语句如123，abc
```

### 配置root的简单密码
```sql
-- 如果你想设置简单密码，需要降低Mysql的密码安全级别
set global validate_password_policy=LOW; -- 密码安全级别低

set global validate_password_length=4; -- 密码长度最低4位即可

-- 然后就可以用简单密码了（课程中使用简单密码，为了方便，生产中不要这样）

ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```
```sql
-- 配置root运行远程登录
-- 授权root远程登录
grant all privileges on *.* to root@"IpAddress" identified by 'password' with grant option;
/**
# IP地址即允许登陆的IP地址，也可以填写%，表示允许任何地址
# 密码表示给远程登录独立设置密码，和本地登陆的密码可以不同
**/
-- 刷新权限，生效
flush privileges;
```
## 安装MySql 8.0
### 更新密钥
```bash
rpm -import https: /repo.mysql.com/RPM-GPG-KEYmysql-2023
```
### 安装Mysql8.x版本 yum库
```bash
rpm -Uvh https: /dev.mysql.com/get/mysql80-
community-release-el7-2.noarch.rpm
```
### yum安装Mysql
```bash
yum -y install mysql-community-server

systemctl start mysqld # 启动

systemctl enable mysqld # 开机自启
```
### 获取MySql初始密码
```bash
# 通过grep命令，在/var/log/mysqld.log文件中，过滤temporary password关键字，得到初始密码
grep 'temporary password' /var/log/mysqld.log
```
### 登录MySql
```bash
mysql -uroot -p
# 解释
# -u，登陆的用户，MySQL数据库的管理员用户同Linux一样，是
root
# -p，表示使用密码登陆
# 执行完毕后输入刚刚得到的初始密码，即可进入MySQL数据库
```
### 修改root密码
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';

-- 密码需要符合：大于8位，有大写字母，有特殊符号，不能是连续的简单语句如123，abc

set global validate_password.policy=0; -- 密码安全级别低

set global validate_password.length=4; -- 密码长度最低4位即可
```
### 配置root远程登录
```sql
-- 第一次设置root远程登录，并配置远程密码使用如下SQL命令

create user 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
-- 密码需要符合：大于8位，有大写字母，有特殊符号，不能是连续的简单语句如123，abc

-- 后续修改密码使用如下SQL命令
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'new_password';

```