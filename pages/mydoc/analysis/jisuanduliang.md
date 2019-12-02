---
title: 计算度量
tags: [计算度量]
last_updated: 2018年12月28日
keywords: 
sidebar: mydoc_sidebar
permalink: jisuanduliang.html
folder: mydoc
---

**计算度量**是指创建一个“计算度量”，“计算度量”包含了计算逻辑，因此这个值可能是个动态值。计算度量使用[MDX](https://mondrian.pentaho.com/documentation/mdx.php)（多维查询表达式）语言定义。

### 创建“计算度量”

1. 在度量选择面板，点击“创建计算度量”按钮

<p align="left">
    <img src="https://dataforhelp.github.io/images/jisuanziduan/jisuanduliang/jisuanduliang-1.png"  width="200" >
</p>

![image-title-here](https://dataforhelp.github.io/images/jisuanziduan/jisuanduliang/jisuanduliang-1.png){:class="img-responsive"}

2. 编辑计算度量公式

<p align="left">
    <img src="https://dataforhelp.github.io/images/jisuanziduan/jisuanduliang/jisuanduliang-2.png"  width="600" >
</p>

### 举例

```
[Measures].[Profit] / [Measures].[Units Shipped]
```

