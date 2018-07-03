---
title: hexo与Typora
comments: false
typora-root-url: ..
typora-copy-images-to: ../images
date: 2018-06-06 20:54:29
tags:
- hexo
- Typora
categories:
- 杂货铺
permalink:
---
<blockquote class="blockquote-center">明月装饰了你的窗子,<br>你装饰了别人的梦</blockquote>



## 使用Typora来处理hexo本地图片

[Typora](https://typora.io/#download)与其他Markdown编辑器不同的是，Typora没有采用[源代码](https://zh.wikipedia.org/wiki/%E6%BA%90%E4%BB%A3%E7%A0%81)和预览双栏显示的方式，而是采用[所见即所得](https://zh.wikipedia.org/wiki/%E6%89%80%E8%A7%81%E5%8D%B3%E6%89%80%E5%BE%97)的编辑方式，实现了即时预览的功能，但也可切换至源代码编辑模式。

这都不是重点。。。重点是可以直接截图复制到图片目录下，不用手动存图片了。

### hexo 配置

- 建立图片目录，位置在source下

  ```
  mkdir source/images
  ```

- 追加[Front-matter](https://hexo.io/zh-cn/docs/front-matter.html)参数

  即所有的图片都放到 `source/images` 路径中

  我这边直接修改的post模版，配置如下:

  ```
  ---
  title: {{ title }}
  date: {{ date }}
  comments: false
  tags:
  categories:
  permalink:
  typora-root-url: ..
  typora-copy-images-to: ../images
  ---
  ```

  ### Typora 配置

- 设置Typora图片路径为相对路径

  以 Mac OS 下的 Typora 为例，打开`偏好设置` 选择 `编辑器` 页，勾选 `优先使用相对路径` 。

  ![image-20180606204632760](/images/image-20180606204632760.png)


- 上面操作完成后，使用 `hexo new  ` 新建文章：

  ```
  hexo new test1
  ```

  截图复制，Typora已经自动帮你生成好了

  ![image-20180606210817174](/images/image-20180606210817174.png)

  保存刷新本地博客页面你会看到图片出现在网页中了。

