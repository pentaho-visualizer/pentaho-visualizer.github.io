---
title: "安装Datafor"
keywords: 安装
tags: []
sidebar: mydoc_sidebar
permalink: anzhuang.html
---

### 下载Datafor

下载地址：

[https://github.com/datafor123/pentaho-plugin ](https://github.com/datafor123/pentaho-plugin )

###  安装Datafor

1. 将datafor.zip解压到  “pentaho-solutions\system” 目录

   <img src="../../../images/image-20191121164424482.png" alt="image-20191121164424482" style="zoom: 67%;" />

2. 修改“pentaho-solutions\system” 目录下的 ImportHandlerMimeTypeDefinitions.xml 文件，添加如下代码：

   ```xml
   <MimeTypeDefinition mimeType="application/json" hidden="false">
        <extension>datafor</extension>
   </MimeTypeDefinition>
   ```
   ![image-20191121164830471](../../../images/image-20191121164830471.png)

3. 修改“pentaho-solutions\system”目录下的applicationContext-spring-security.xml文件，添加如下代码：

   ```xml
   <sec:intercept-url pattern="\A/plugin/datafor/api/openshare.*\Z" access="Anonymous,Authenticated" />
   ```
   ![image-20191121165155937](../../../images/image-20191121165155937.png)

4. 重启Pentaho PBA，安装完成

