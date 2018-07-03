---
title: 基础知识之linux特殊权限位seuid、setgid、sticky
comments: false
typora-root-url: ..
typora-copy-images-to: ../images
date: 2018-07-02 11:18:00
tags:
- 基础知识
- linux
categories:
- 杂货铺
permalink:
---
<blockquote class="blockquote-center">从前的日色变得慢<br>车，马，邮件都慢<br>一生只够爱一个人</blockquote>

## 介绍

### setuid

在一个程序或命令后添加setuid后(chmod u+s /file)，这样属主就有了s权限，意味着任何用户执行子程序时，其进程的属主不再是发起者本人，而是这个程序的属主。最典型的例子就是passwd这个命令。普通用户运执行passwd命令来修改自己的密码，其实最终更改的是/etc/passwd这个文件。我们知道/etc/passwd文件是用户管理的配置文件，只有root权限的用户才能更改。按照常规逻辑，普通用户是修改不了/etc/passwd此文件的，但是在passwd这个命令上添加了setuid这个特殊权限位，普通账号临时变成root，就能间接修改自己账号的密码了。

```
[root@FF0X ~]# ls -l /usr/bin/passwd  # (s就是suid位)
-rwsr-xr-x. 1 root root 27832 6月  10 2014 /usr/bin/passwd
```

设置setuid的方法：

```
chmod u(+|-)s /path/somefile
chmod 4664 /path/somefile
注意：
s:表示属主原来有执行权限
S:表示属主原来没有执行权限
```

举个例子：

![image-20180702153111129](/images/image-20180702153111129.png)

```
[root@FF0X tmp]# cat test.txt
hello word!
[root@FF0X tmp]# chmod o-r test.txt (删除其他用户读取权限)
[root@FF0X tmp]# ll
-rw-r----- 1 root   root   12 7月   2 15:10 test.txt
[root@FF0X tmp]# su - test
# 切换用户

[test@FF0X tmp]$ cat test.txt
cat: test.txt: Permission denied
# 权限不足无法读取

[root@FF0X tmp]# cp /bin/cat /tmp/
[root@FF0X tmp]# chmod u+s cat
-rwsr-xr-x 1 root   root   54048 7月   2 15:15 cat
# 给cat添加setuid，这样cat就有了s权限，意味着任何用户在用这个程序，进程不再是发起者本人，而是这个程序属主。

[test@FF0X tmp]$ ./cat test.txt
hello word!
```



### setgid

属组有s权限，在执行此程序，此进程的属组不再是运行者本人所属的基本组，而是此程序文件的属组。setgid曲线如果给文件设置，是让运行此文件的其他用户具有这个文件的属组特性。给目录设置Set gid权限，任何用户在该目录下创建的文件，则该文件属组都和目录的属组一致。

![image-20180702165426499](/images/image-20180702165426499.png)

### sticky

谁开发，谁管理；谁污染，谁治理。

![image-20180702192623610](/images/image-20180702192623610.png)

