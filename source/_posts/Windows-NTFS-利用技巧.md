---
title: Windows NTFS 利用技巧
comments: false
typora-root-url: ..
typora-copy-images-to: ../images
date: 2018-06-14 13:44:46
tags:
- 内网技巧
- NTFS
categories:
- 内网安全
permalink:
---
<blockquote class="blockquote-center">满地都是六便士，<br>他却抬头看见了月亮。</blockquote>

### tip 1: 权限绕过建立文件夹(**CVE-2018-1036 / NTFS EOP**)

在windows上可以为自己的文件夹建立一些特殊的权限，例如：允许用户在该文件夹下创建文件，但不允许创建文件夹。

这种文件的例子：```c:\windows\tasks\```

![image-20180614165210177-9044235](/images/image-20180614165210177-9044235.png)

只要该文件夹允许用户创建文件，这个acl就可以绕过。将```:: $ INDEX_ALLOCATION```添加在文件名的末尾，这时候创建的就不是一个文件了，而是文件夹。

![image-20180614165607425-9044400](/images/image-20180614165607425-9044400.png)

注：如果该路径下，只允许删除文件，同样的，将```:: $ INDEX_ALLOCATION```的技巧也可以删除目录。



### tips 2: 用备用数据流绕过路径限制

没成功。

### tip 3: 使用"…"文件夹创建无找到的文件

每个文件都包含两个默认的特殊文件夹，即'.'和'…'目录，它们都指向父目录。![image-20180615104012068-9045460](/images/image-20180615104012068-9045460.png)

这位博主[sec-consult](https://www.sec-consult.com)说是用tips1的方法，可以添加，但是自己试了以后发现，不用这一步骤，也同样可以生成```…```文件。

![image-20180615104725338-9045473](/images/image-20180615104725338-9045473.png)

当然这种文件进入的方式也有些不太一样

![image-20180615155733116](/images/image-20180615155733116.png)

会发现在这个文件夹下的文件无法正常被打开。提示找不到该文件

使用powershell查找发现也无法查询。（```Get-ChildItem -Path C:\test -Filter 123.txt -Recurse -ErrorAction SilentlyContinue -Force```）

![image-20180615151030079](/images/image-20180615151030079.png)

![image-20180615150325772](/images/image-20180615150325772.png)

有趣的是，你在命令行下，…目录中却可以正常查看文件内容。

![image-20180615151538393](/images/image-20180615151538393.png)



### tips 4: '…'目录链接

目录链接是一个非常有用的NTFS功能，使用它可以创建到目标文件夹的符号链接。

但是使用文章中的方式在测试发现并不能执行。（server08、server12下均不能使用）

![image-20180615160338515](/images/image-20180615160338515.png)

创建 ". ."目录

*** echo 123 > ". .::$index_allocation" ***

两"."之前有一个空格，进入目录带引号。

![image-20180619105954854](/images/image-20180619105954854.png)





### 原文

* https://www.sec-consult.com/en/blog/2018/06/pentesters-windows-ntfs-tricks-collection/