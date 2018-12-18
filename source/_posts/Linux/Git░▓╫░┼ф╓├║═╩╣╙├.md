---
date: 2018-11-15 10:40
status: public
title: Git安装配置和使用
categories: Linux
password: a1b2c3.
tags: Linux
---

## 密钥

**公钥与私钥：**

1.公钥加密，私钥解密
2.私钥数字签名，公钥验证

## SSH密码登陆原理（公钥加密）

**用户登陆远程机：**

1.远程主机收到用户的登录请求，把自己的公钥发给用户
2.用户使用这个公钥，将登陆密码加密后，发给远程机
3.远程机用自己的私钥，解密登陆密码，确定正确，同意用户登陆

**解决存在风险： 中间人攻击**

截获用户登陆请求，冒充远程机，伪造公钥发给用户，获取远程机密码
1.known_hosts文件 （区别于authorized_keys文件）
当用户接收远程主机的公钥后，它就会被保存在文件`~/.ssh/known_hosts`之中。下次再连接这台主机，系统就会认出它的公钥已经保存在本地了，可以辨别中间人的伪造公钥。
2.口令登陆
第一次登陆远程机，用户没有其公钥，为防止中间人攻击，系统会出现下面提示：
```sh
$ ssh user@host

　　The authenticity of host 'host (12.18.429.21)' can't be established.

　　RSA key fingerprint is 98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d.

　　Are you sure you want to continue connecting (yes/no)?
```
提示：无法确认host主机的真实性，只知道它的公钥指纹，是否继续登陆？
上面　`98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d` 就是远程机的公钥指纹，为了安全登陆，可以确认远程机的公钥指纹，一般远程机会贴在自己的网站上，以便用户自行核对。

## SSH免密登陆原理(RSA加密技术)

Step1：SSH密钥配置
1.在A上生成密钥对
2.将A的公钥拷贝到B上
3.在B上将A的公钥写入到授权列表文件authorized_keys中

![](http://wx4.sinaimg.cn/large/007fPWmPly1fy41o2n2plj30d3097wfo.jpg)


Step2: SSH免密登陆原理（git登陆原理，B相当于git服务器）
1.A请求登陆B
2.B查看授权列表
3.B把A的公钥加密一随机字符串发给A
4.A用私钥解密B发送过来的字符串
5.A用私钥加密结果发送给B（数字签名）
6.B用A的公钥验证A发过来的解密字符串
7.验证通过登陆成功

![](http://wx1.sinaimg.cn/large/007fPWmPly1fy41o2x097j30j608babp.jpg)




## SSH生成迷密钥对

`ssh-keygen -t rsa -f ~/.ssh/id_rsa.${name} -P "" -q`

-t参数：加密方式rsa
-f参数：文件名及保存路径 eg： id_rsa.xblin id_rsa.xblin.pub
-P参数：提取密钥的密码，默认为无


### 配置config文件

`~/.ssh/config ` 
SSH默认读取id_rsa这个私钥
修改配置指定路径和名字：

1.Host          #主机地址  
2.User          #认证用户
3.IdentifyFile  #认证私钥路径 

> Tips : Host * 匹配所有的主机

eg：

```
Host icode.baidu.com
User linxubin
IdentityFile ~/.ssh/id_rsa.linxubin
```

> Important: config文件权限必须是644

### Shell脚本代码实现

```sh
#!/bin/bash
usage(){
    echo 
}

ssh_config(){
    name=$1
    Remote_Host=$2

    yum install -y expect

    if [[ ! -f ~/.ssh/id_rsa.${name}.pub ]]; then
        ssh-keygen -t rsa -f ~/.ssh/id_rsa.$1 -P "" -q  #no passphrase

        echo "Host *" >> ~/.ssh/config
        echo "User ${name}" >> ~/.ssh/config
        echo "IdentityFile ~/.ssh/id_rsa.${name}" >> ~/.ssh/config

        chmod 644 ~/.ssh/config  # chmod must 644
    fi

    ssh root@${Remote_Host} "cat >> /root/.ssh/authorized_keys" < /root/.ssh/id_rsa.${name}.pub
}

ssh_config $1 $2
```

## Github ssh配置

### 一. git clone命令
本地主机clone远程github仓库不需要建立信任关系

### 二. ssh建立信任关系
1.生成密钥对： `ssh-keygen -t rsa ` 
2.gitconfig配置 

```
$ git config --global user.name "xxx"   ##github用户名
$ git config --global user.email xxx@example.com  ##github注册邮箱
```

- /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件
- ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。

3.指定生成密钥的名字
- 生成密钥对（指定名字） `ssh-keygen -t rsa -f ~/.ssh/id_rsa.lxbgithub`
- 配置 ~/.ssh/config 文件

```
Host github.com
User xblin
IdentityFile ~/.ssh/id_rsa.lxbgithub
```

----
