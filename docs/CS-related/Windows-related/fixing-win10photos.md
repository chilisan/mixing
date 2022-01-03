---
title: 修复Win10默认照片查看器
date: 2021-08-22_Sun
status: 长青期
tags: 
- win10
source: http://www.windows764.org/news/13945.html
aliases: []
---

快捷键 Win+R 打开「运行」，输入"regedit"，进入如下目录：
```
计算机\HKEY LOCAL MACHINE\SOFTWARE\Microsoft\Windows Photo Viewer\Capabilities\FileAssociations
```
再改目录下点击鼠标右键，新建字符串值。
如果希望照片查看器打开".jpg"格式图片，则输入".jpg"，数值数据填入"PhotoViewer.FileAssoc.Tiff"。
以此类推，另照片查看器支持多种格式。