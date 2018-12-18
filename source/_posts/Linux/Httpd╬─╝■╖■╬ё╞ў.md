---
date: 2018-11-09 20:58
status: public
title: Httpd文件服务器
categories: Linux
tags: Http
---

## 文件服务器搭建方法
- Httpd(appache2)
- vsftpd
- python SimpleHTTPserver

## 搭建Httpd文件服务器
#### 1.安装
> Centos: yum install httpd
> Ubuntu: apt-get install apache2

#### 2.配置
> centos为例

**查看httpd版本**：`httpd -v`
**查看httpd配置文件路径：** `httpd -V`
**配置文件默认目录:** `/ect/httpd/conf/httpd.conf`
**修改配置文件：**
1.端口号

Listen 80` 
可以改为Listen 8000 或任意符合要求的端口

2.文件存放目录

`DocumentRoot "/home/xblin"`

> tips： 目录文件一定要有读的权限，最好放在/home下，否则会出现you don't have permission.

`<Directory "/home/xblin">  #这个同上也要改`

**启动Httpd服务：**

```Shell
service httpd stop
service httpd start
service httpd restart
```
#### 3.Web查看
网页登陆链接 http://机器ip：端口
eg： `http://10.32.115.38:8000`
*第一次登陆会弹出欢迎界面*
注释  `/ect/httpd/conf.d/welcome.conf` 
*再次登陆就能看到文件列表*

#### 4.文件下载
wget http://机器ip：端口/文件名
eg： `wget http://10.32.115.38:8000/aa.txt`

## 扩展进阶

- 设置密码登陆
- 前端界面设计
