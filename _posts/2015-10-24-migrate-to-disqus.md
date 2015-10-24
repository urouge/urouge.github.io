---
layout: post
title: 多说评论迁移至Disqus
description: 
tag: duoshuo,disqus
---

将多说评论迁移至 Disqus 的原因大家都有体会，这里主要介绍迁移过程中遇到的问题和细节。

多说和 Disqus 评论的格式和字段内容区别有些大，前者是json格式，后者要求的是xml，总的来说，还是 Disqus 的评论结构比较简洁。Disqus官方有导入文件的<a href="https://help.disqus.com/customer/en/portal/articles/472150-custom-xml-import-format" target="_blank">说明文档</a>，剩下的工作自然是理清两者之间的关系即可，

## 如何使用

经过一番折腾用PHP写出了以下迁移评论的程序，使用过程如下：

1. 在多说后台下载评论文件，默认文件名是 **export.json**;
2. 下载文件<a href="{{ site.url }}/assets/migrate.php">**migrate.php**</a>，位置与**export.json**同级;
3. 打开终端，进入文件 **migrate.php** 所在目录，执行 **php -f migrate.php** 即可在同级目录生成 **disqus.xml** 文件，前提是将 php 的可执行程序添加至环境变量;
4. 在 Disqus 后台选择**Generic(WXR)**导入即可，地址是<br />https://{你的站点名}.disqus.com/admin/discussions/import/platform/wordpress/

## 一些说明

1. 每个人网站程序都会有所区别，导致在多说中设置 thread 也会不同，此外，在多说导出包含文章和评论的文件中竟然发现了一些广告文章，导致我不得不在程序中筛选出所需要的文章，下面的程序中进行的文章筛选结果是多说后台有正常文章记录(thread_key非空)以及有评论的文章，有需要的话可以根据自己网站的实际情况增加筛选条件;
2. 在 Disqus 导入评论后，程序会以队列的方式进行处理，可以在<a href="https://import.disqus.com/" target="_blank">这里</a>查看队列进程，甚至也可以在这里导入评论文件

## 遇到的问题

导入 disqus.xml 文件后，会出现的几条**Unable to find parent post**提示，结果是那几条评论没有成功导入，提示很简单，说的是没有找到该评论的父评论，但是我单独将那几条评论导入后是可以成功导入的，目前不知道是什么原因。另外，导入评论后的头像是默认头像。
