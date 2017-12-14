---
title: Zxing生成二维码中文识别乱码
date: 2017-12-14 17:31:17
categories: "Android"
tags: "异常"
---

最近开发项目需要用到zxing来实现二维码名片，但是生成二维码之后扫面出来中文总是显示不出来，在网上查了很多的资料，最后发现是识别二维码时字符编码的问题，具体解决方法如下：

1.zxingx下载地址：[https://github.com/zxing/zxing](https://github.com/zxing/zxing)

2.在源码目录找到下图的类

![](https://i.imgur.com/1nyuPOR.png)

将`static final String DEFAULT_BYTE_MODE_ENCODING = "ISO-8859-1";`修改为`static final String DEFAULT_BYTE_MODE_ENCODING = "UTF-8";`

这句话就是修改默认的编码模式，重新编译一下代码，应该中文乱码的问题就解决了。
