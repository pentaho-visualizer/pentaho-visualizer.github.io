---
title: "URL传参"
keywords: datafor
tags:
sidebar: mydoc_sidebar
permalink: canshuchuandi.html
---
### 概述
url传参功能，主要应对如下场景。
-   场景1：第三方系统通过url打开Datafor页面，通过url向Datafor传递参数，进行数据过滤等操作。 
-   场景2：Datafor页面跳转、链接到另外一个Datafor页面。

使用URL参数进行传值时，参数名有3种命名方式，参数支持2种赋值方式。参数名的不同命名方式，作用范围和精准度稍有差异。参数的不同赋值方式，复杂度有所不同，但最终目标都是对页面上某个数据维度（维度下的某个level），进行过滤筛选。

所有的参数名，均已param_打头。

下文中，出现的Cube、Hierarchy、Level、Caption、Name、uniqueName等词汇，均为多维分析词汇术语。

### 参数名的命名

#### Caption方式

参数名取值level的caption属性。表示该参数将对页面中所有cube的Caption与之匹配的level生效，如果cube中有多个维度多个level的caption都匹配成功，参数将同时对这些level一同生效。因此这种参数过滤范围最广泛，目标最不精确。

```
例如:
param_Province=广东;湖北
完整url示例
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?param_省份=广东;湖北
```

#### LevelName方式
参数名取自level的name属性，匹配level的机制和Caption方式类似。

```
示例
param_Province=广东;湖北
完整url示例
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?param_Province=广东;湖北
```
#### uniqueName方式
参数名取自level的uniqueName属性。
如上两种方式，因为会匹配整个cube中所有与之匹配的level。因此，对于cube中存在不同维度下有重名level的场景，可能会导致数据过滤不准确，甚至过滤后查无数据等情况。
此时，如果期望参数的影响范围精确到cube的指定level位置，推荐使用这种参数命名方式。
这种参数名具备id性质，在cube范围内目标更精确，它可以保证过滤只作用到同一个cube中的指定level上。
```
示例
param_[Geo.Default].[Province]=广东;湖北
完整url示例
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?param_[Geo.Default].[Province]=广东;湖北
```
#### cubeUniqueName+levelniqueName方式
参数名取自level所在cube的uniqueName、追加level本身的uniqueName。
上述uniqueName方式，能解决cube内level重名的问题；但是，当页面存在多个cube，且不同cube下可能会包含相同id的level时，过滤就无法精准到指定位置了。此时，使用这种命名方式，可以精确到指定cube的指定level。 
```
示例
param_[gosalesdw-geo].[gosalesdw-geo].[gosalesdw-geo].[Sales]~[Geo.Default].[Province]=广东;湖北
完整url示例
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad? param_[gosalesdw-geo].[gosalesdw-geo].[gosalesdw-geo].[Sales]~[Geo.Default].[Province]=广东;湖北
```
#### 特殊参数名
Datafor文件默认开放了一套特殊参数，通过这些特殊的url参数，可以实现一些特殊功能的空值。
##### __compact
是否去掉页面和页面容器之间的默认间距，取值true或者false。

```
例如
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?__compact=true
```
当datafor页面需要嵌入第三方系统的iframe等容器时，该参数可以消除或者保留页面和容器之间的默认间距。

### 参数的赋值
#### 明文式传参
参数值不加密直接以明文方式赋值在参数名后面。
```
例如
http://localhost:8080/report/plugin/datafor/api/open/98af3850-aef1-4b4f-8ba4-967e601377ad?param_Province=广东;湖北
```
#### 函数式传参
参数值是一段mdx表达式的base64加密文本。mdx表达式是指符合mdx规范的mdx函数、或者mdx集合等表达式。
```
例如
{member1:member2}
{member1,member2}
DateToMember()等

示例
http://localhost:8080/pentaho/plugin/datafor/api/integrate/L2hvbWUvYWRtaW4vY2hhbm5lbF9wcm92aW5jZV9saXN0LmRhdGFmb3I=?__compact=true&param_MONTH%20KEY=JTdCJTIybG9jYXRpb24lMjIlM0ElMjJmaWx0ZXJzJTIyJTJDJTIydHlwZSUyMiUzQSUyMkZpbHRlciUyMiUyQyUyMnZhbHVlJTIyJTNBJTIyJTVCR28lMjB0aW1lJTIwZGltLiVFNSVBNCVBOSU1RC5DdXJyZW50TWVtYmVyJTIwaW4lMjAlN0IlN0JEYXRlVG9NZW1iZXIoJTVCR28lMjB0aW1lJTIwZGltLiVFNSVBNCVBOSU1RC4lNUJNT05USCUyMEtFWSU1RCUyQyUyMCclM0UlM0QnJTJDJTIwMTEzMzM2NjQwMDAwMCUyQydBc2lhJTJGU2hhbmdoYWknJTJDJ2VuJyklM0FEYXRlVG9NZW1iZXIoJTVCR28lMjB0aW1lJTIwZGltLiVFNSVBNCVBOSU1RC4lNUJNT05USCUyMEtFWSU1RCUyQyclM0MlM0QnJTJDJTIwMTE2NDkwMjQwMDAwMCUyQydBc2lhJTJGU2hhbmdoYWknJTJDJ2VuJyklN0QlN0QlMjIlN0Q=
```
### 多值传递
多值传递，是指通过一个参数，同时传递多个值。不管是明文式赋值，还是函数式赋值，多值传输都是被允许的。多值传递分两种类型，离散传值和范围传值。
#### 离散传值
多值之间通过分号进行分割拼接。
```
例如 param_ Province=广东;湖北
```
#### 连续传值
通过构造一个mdx集合表达式，然后按照约定构建出一个JSON对象，再进行encodeURI编码，base64加密等。
```
例如:
{[Time].[2005]:[Time].[2007]}，
对象构建成
{
	"location": "mdx",
	"type": "Filter",
	"value": "{[Go time dim.天].[2004]:[Go time dim.天].[2005]}"
}
```
然后URI编码以及Base65加密后，为
```
JTdCJTIybG9jYXRpb24lMjIlM0ElMjJtZHglMjIlMkMlMjJ0eXBlJTIyJTNBJTIyRmlsdGVyJTIyJTJDJTIydmFsdWUlMjIlM0ElMjIlN0IlNUJHbyUyMHRpbWUlMjBkaW0uJUU1JUE0JUE5JTVELiU1QjIwMDQlNUQlM0ElNUJHbyUyMHRpbWUlMjBkaW0uJUU1JUE0JUE5JTVELiU1QjIwMDUlNUQlN0QlMjIlN0Q=	
```
完整示例
```
http://localhost:8080/pentaho/plugin/datafor/api/edit/L2hvbWUvYWRtaW4vYWFhLmRhdGFmb3I=?__debug=true&param_%E5%B9%B4%E4%BB%BD=JTdCJTIybG9jYXRpb24lMjIlM0ElMjJtZHglMjIlMkMlMjJ0eXBlJTIyJTNBJTIyRmlsdGVyJTIyJTJDJTIydmFsdWUlMjIlM0ElMjIlN0IlNUJHbyUyMHRpbWUlMjBkaW0uJUU1JUE0JUE5JTVELiU1QjIwMDQlNUQlM0ElNUJHbyUyMHRpbWUlMjBkaW0uJUU1JUE0JUE5JTVELiU1QjIwMDUlNUQlN0QlMjIlN0Q=
```
### 跨文档消息传输XDM（开发中）
#### 接收外部交互信息
外部第三方系统，在集成了datafor页面之后，可通过XDM向datafor实时地发送一些交互信息，Datafor会对被支持的信息作出及时的响应。这些支持被处理的信息包括：
1.	页面初始状态赋值
2.	页面样式控制
3.	跳转指令的执行
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

