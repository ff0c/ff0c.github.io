---
title: hexo配置tips
comments: false
typora-root-url: ..
typora-copy-images-to: ../images
date: 2018-06-15 11:54:58
tags:
categories:
permalink:
---
<blockquote class="blockquote-center">胜，不妄喜；<br>败，不遑馁；</blockquote>

### tip1 :Hexo之next主题设置首页不显示全文(只显示预览)

- 进入hexo博客项目的themes/next目录

- 用文本编辑器打开_config.yml文件

- 搜索"auto_excerpt",找到如下部分：

	![image-20180615115910049](/images/image-20180615115910049.png)

- 把enable改为对应的false改为true，然后`hexo d -g`，再进主页，问题就解决了！







### error 

1. fatal: Not a git repository (or any of the parent directories): .git

	 .deploy_git下缺少.git 文件，但是检查配置文件没发现问题，我就删除了 .deploy_git这个文件，重新hexo d 发布一下就好了。

2. 