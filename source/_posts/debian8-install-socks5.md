---
title: debian 8 安装 socks5 代理
date: 2018-08-03 17:32:46
tags: [debian, socks]
categories: linux 工具
---
### 安装 build工具

```
$ apt-get install -y gcc make build-essential libpam-dev libldap2-dev libssl-dev
```

### 下载 ss5 代码

```
$ wget https://superb-sea2.dl.sourceforge.net/project/ss5/ss5/3.8.9-8/ss5-3.8.9-8.tar.gz
如果404，到项目主页下载 https://sourceforge.net/projects/ss5/files/ss5/3.8.9-8/
```

### 编译安装

```
$ tar -zxf ss5-3.8.9-8.tar.gz
$ cd ss5-3.8.9-8
$ ./configure
#... 输出信息
$ make && make install 
 which ss5 
/usr/sbin/ss5  # 说明 ss5 已经安装成功
```

### 查看配置

```
$ cd /etc/opt/ss5
$ ls
ss5.conf  ss5.ha  ss5.passwd
```

#### 添加用户名密码

```
$ vim /etc/opt/ss5/ss5.passwd

# 一行一个用户名和一个密码
xiaoming 123456
xiaohong 12345678
```

#### ss5.conf 配置

在87 行 去掉注释，最有一个 `-` 改为 `u` ，如下所示

`auth 0.0.0.0/0 - u`

203 行 去掉注释，第一个 `-` 改为`u` 如下所示

`permit u 0.0.0.0/0 - 0.0.0.0/0 - - - - -`

这里 u 的意思使用ss5.passwd帐号密码登录，- 表示默认任何人都可使用

#### 端口配置

ss5 默认使用1080端口，并允许任何人使用，如果要修改默认端口，请修改

`vim /etc/sysconfig/ss5`

在 `/etc/sysconfig/ss5` 这个文件中，添加下面这一行命令，-b后面的参数代表监听的ip地址和端口号  
```
# Add startup option here 　　
SS5_OPTS=" -u root -b 0.0.0.0:8080"
```

### 启动

```
$ ss5 -t -p /opt/ss5.pid
$ ps -f `cat /opt/ss5.pid `
UID        PID  PPID  C STIME TTY      STAT   TIME CMD
nobody   21580     1  0 10:38 ?        S      0:00 ss5 -t -p /opt/ss5.pid
# 这里说明程序已经启动了 ，我们下面再看下端口有没有 ，我没有改端口，你要是改了端口就 grep 你改的端口
$ netstat -ant | grep 1080
tcp        0      0 0.0.0.0:1080            0.0.0.0:*               LISTEN
# 可以看到端口已经启动了
```
