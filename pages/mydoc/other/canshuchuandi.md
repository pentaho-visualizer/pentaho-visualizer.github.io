---
title: "和第三方系统参数传递"
keywords: datafor
tags:
sidebar: mydoc_sidebar
permalink: canshuchuandi.html
---
### 概述
url传参功能，主要应对如下场景。
-   场景1：嵌入型应用场景下，第三方系统通过url打开datafor页面，通过url向datafor传递参数。 
-   场景2：系统内，datafor页面跳转链接到另外一个datafor页面（场景2中url上需要传递一些高级的参数值）


Datafor提供三套参数名命令机制，两套参数值传参机制。明文传参，函数传参。参数名可以有3中命名方式，各命名方式作用范围各不相同。

除此之外，datafor文档还接受跨文档消息传输机制传递的参数；并支持通过该机制和第三方系统的页面之间，进行实时的信息交互。

### 参数名命名机制

#### LevelName方式

参数名取自level的name属性。url对应目标页里的数据组件，只要绑定模型里有level的name属性和参数名匹配上，参数值便会在这个组件上起过滤作用。因此，这种方式的url过滤，虽然过滤目标最不精确，但过滤影响的范围最广。

例如:
param_Province=广东;湖北
完整url示例

```
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?param_Province=广东;湖北
```

#### levelUniqueName方式
参数名取自level的uniqueName属性。这种参数名具备id性质，因此过滤目标会比levelName方式更精确。它可以保证过滤只作用到同一个模型中的指定level上，不会跨hierarchy多个level同时被作用；但有可能跨cube生效，因为不同的cube，可能会包含相同id的level。

例如：

param_[Geo.Default].[Province]=广东;湖北
完整url示例

```
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?param_[Geo.Default].[Province]=广东;湖北
```

#### cubeUniqueName+levelUniqueName方式

参数名取自level所在cube的uniqueName以及level本身的uniqueName。

例如：

param_[gosalesdw-geo].[gosalesdw-geo].[gosalesdw-geo].[Sales]~[Geo.Default].[Province]=广东;湖北

完整url示例

```
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad? param_[gosalesdw-geo].[gosalesdw-geo].[gosalesdw-geo].[Sales]~[Geo.Default].[Province]=广东;湖北
```

#### 特殊参数名

Datafor文件默认开放了一套特殊参数，通过这些特殊的url参数，可以实现一些特殊功能的空值。

##### __compact

是否去掉页面和页面容器之间的默认间距，取值true或者false。例如

```
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?__compact=true
```

当datafor页面需要嵌入第三方系统的iframe等容器时，该参数可以消除或者保留页面和容器之间的默认间距。

###  参数值传递机制
#### 明文式传参
参数值不加密直接以明文文本方式赋值在参数名后面。例如：

```
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?param_Province=广东;湖北
```

参数值“广东;湖北”即明文传值

#### 函数式传参（开发中）
MDX_EXP(param)
param参数是一段mdx表达式的base64。mdx表达式是指符合mdx规范的mdx函数、或者mdx集合等。例如:
{member1:member2}
{member1,member2}
sum(set, numeric_expression)等
这种类型的过滤将注入到响应level的filters属性里
### 多值传递
多值传递，是指通过一个参数，同时传递多个值。不管是明文式传输，还是函数式传输，多值传输都是被允许的。多值传递分两种类型，离散传值和范围传值。
#### 离散传值
多值之间通过分号进行分割拼接。例如param_ Province=广东;湖北
#### MDX表达式传值
结合MDX_EXP函数，传递一个mdx表达式过滤条件。
比如一个集合{[Time].[2003]:[Time].[2007]}，这个表达式需要先经过base64编码，接着进行encodeURI编码，然后再作为MDX_EXP函数参数传入url
#### 混合组合
一个MDX表达式传值可构成一个离散值，继续和其他参数值通过离散传值方式组合。例如
param_YEARS=2005;MDX_EXP(…);2015
### 跨文档消息传输XDM（开发中）
#### 接收外部交互信息
外部第三方系统，在集成了datafor页面之后，可通过XDM向datafor实时地发送一些交互信息，Datafor会对被支持的信息作出及时的响应。这些支持被处理的信息包括：
- 页面初始状态赋值
- 页面样式控制
- 跳转指令的执行
##### 可交互前提
需在页面设置面板里，指定当前页面认可的域名，可以指定多个。
##### 交互信息格式
在当前域名（第三方系统）被datafor页面任何的前提下，在页面中通过调用XDM标准方法向datafor页面发送的跨文档消息，消息格式为

```
{
	action: ‘’,  //指令类型
	timestamp: ‘’, //时间戳
	content: {}  //指令附加消息
}
```

为兼容起见，消息最好通过JSON.stringify进行序列化处理。

#### 接收datafor的交互信息
##### 可交互前提
需在页面设置面板里，指定当前页面认可的域名，可以指定多个。
##### 交互信息格式
```
{
	eventName: ‘’ //事件名response或者click等其他
	timestamp: ‘’  //响应时间戳。当eventName为response时，外部通过这个时间戳是否与请求时时间戳一致，来判断这个消息是否为指定请求的响应消息
}
```


