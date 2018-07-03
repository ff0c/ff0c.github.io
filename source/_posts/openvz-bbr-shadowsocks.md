---
title: openvz-bbr-shadowsocks
comments: false
typora-root-url: ..
typora-copy-images-to: ../images
date: 2018-06-11 17:15:22
tags:
- shadowsocks
- openvz+bbr
categories:
- 杂货铺
permalink:
---
<blockquote class="blockquote-center">好东西不用你去记,<br>它自会留下很深的印象</blockquote>

## Openvz+bbr + Shadowsocks

有些时候需要代理访问网站，可VPS的访问速度不佳，可以试试bbr进行网络的加速。

### BBR安装

下载安装脚本

```
wget https://raw.githubusercontent.com/kuoruan/shell-scripts/master/ovz-bbr/ovz-bbr-installer.sh 
chmod +x ovz-bbr-installer.sh 
```

如果glibc版本太低，会提示

更新 glibc 2.15以上(后面记得加 —force —nodeps 原因可以看[这里](https://blog.csdn.net/wulantian/article/details/8804696),套件被重复安装两次以上，可以用前面命令忽略)

```
wget http://ftp.redsleeve.org/pub/steam/glibc-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/glibc-common-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/glibc-devel-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/glibc-headers-2.15-60.el6.x86_64.rpm \
http://ftp.redsleeve.org/pub/steam/nscd-2.15-60.el6.x86_64.rpm --force --nodeps

rpm -Uvh glibc-2.15-60.el6.x86_64.rpm \
glibc-common-2.15-60.el6.x86_64.rpm \
glibc-devel-2.15-60.el6.x86_64.rpm \
glibc-headers-2.15-60.el6.x86_64.rpm \
nscd-2.15-60.el6.x86_64.rpm
```

安装后查看版本

```
ldd --version
```

![image-20180615121828848](/images/image-20180615121828848.png)

在VPS配置面板开启TUN/TAP

![image-20180611192948287](/images/image-20180611192948287.png)

执行脚本

```
./ovz-bbr-installer.sh
```

查看是否安装成功

```
ping 10.0.0.2 # 是否可以ping通
```



### Shadowsocks 安装

下载安装脚本

```
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log
```

接下来步骤就很简单了，配置密码、端口（使用bbr加速的那个端口）、加密方式、等待，成功如下。开心的网上冲浪吧～

![image-20180611200210983](/images/image-20180611200210983.png)

### 客户端

https://github.com/shadowsocks

### 其他

卸载

```
./ovz-bbr-installer.sh uninstall
```

多端口

```
vim /usr/local/haproxy-lkl/etc/port-rules
```

启动、停止、重启

```
systemctl {start|stop|restart} haproxy-lkl
service haproxy-lkl {start|stop|restart}
```

 ### 引用

* [shadowsocks](https://teddysun.com/486.html)
* [openvz+bbr](https://blog.kuoruan.com/116.html)