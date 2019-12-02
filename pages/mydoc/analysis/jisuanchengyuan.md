---
title: 计算成员
tags: [计算成员]
last_updated: 2018年12月28日
keywords: 
sidebar: mydoc_sidebar
permalink: jisuanchengyuan.html
folder: mydoc
---

**计算成员**是指在维度字段中创建一个“值”，这个“值”包含了统计逻辑，因此这个值可能是个动态值。计算成员使用[MDX](https://mondrian.pentaho.com/documentation/mdx.php)（多维查询表达式）语言定义。

### 创建“计算成员”

1. 在维度成员选择面板，选中一个成员，点击查看数据按钮 {% include inline_image.html
    file="/jisuanziduan/jisuanchengyuan/button.png" alt="SDK button" %}

![计算成员  align="left"](https://dataforhelp.github.io/images/jisuanziduan/jisuanchengyuan/jisuanchengyuan-1.png)

2. 在计算成员公式编辑窗口添加计算成员

![新增计算成员](https://dataforhelp.github.io/images/jisuanziduan/jisuanchengyuan/jisuanchengyuan-2.png)

### 例子

```
-如果用户没有电子邮件这样的属性，则返回默认文本
CoalesceEmpty（[User] .CurrentMember.get（'Email'），“没有可用的公共电子邮件”）
```

