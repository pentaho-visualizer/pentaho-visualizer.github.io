---
title: 嵌入式集成
keywords: datafor
tags:
sidebar: mydoc_sidebar
permalink: qianrulianjie.html
---
嵌入式集成是指通过"URL链接”将可视化与分析页面嵌入到第三方Web页面。通过集成，用户可以从第三方Web应用、移动App等应用访问Datafor可视化与分析页面。

### 嵌入的工作方式

- 您可以在支持HTML iframe标签的任何网站或应用中嵌入Datafor页面。

  - iframe代码包括指向报表的链接，并由Datafor自动生成。不需要HTML知识。

- 如果该Datafor页面与特定的用户，组或域共享，则只有那些人才能看到嵌入的报告。这些查看者必须登录到Pentaho BI服务器。
- 嵌入页面可以“隐藏工具栏”或“显示工具栏”，根据用户的权限设置，工具栏上是否有“编辑”按钮。

### 生成嵌入URL链接

1. 将嵌入页面的“布局模式”设置为“宽度自适应”

   ![image-20191128141942098](../../../images/image-20191128141942098.png)

2. 点击工具栏上的“页面嵌入”按钮

   ![image-20191128141512328](../../../images/image-20191128141512328.png)

3. 生成“URL链接”

   ![image-20191128142102457](../../../images/image-20191128142102457.png)
