# Google API 设计指南 - 资源名称

* 原文地址: https://cloud.google.com/apis/design/resource_names
* 当前版本: 2017-02-21
* 翻译日期: 2月24日，2017
* Copyright: Creative Commons Attribution 3.0 License

在面向对象的API 里，资源是有名称的实体，资源名称是他们的标识符。每个资源必须有其独有的资源名。资源名称由资源本身的ID ，其父资源的ID 和API 服务名称组成。 我们将在下文中接触到资源ID 和一个资源名是如何构成的。

gRPC API 应该使用无结构的URI 来用作资源名称。他们通常会遵从REST URL 的惯例并且其行为如同网络路径名称一样。他们可以轻易地被映射到REST URL上：在[标准方法](https://cloud.google.com/apis/design/standard_methods)一节可以看到更多细节。

一个集合是一种含有一列相同类型子资源的特殊资源。比如，一个目录就是一个文件资源的集合。集合的资源ID 被称为集合ID 。

资源名称将集合ID 和资源ID 按层级组织，使用正向斜杠( / )分隔。如果一个资源包含了一个子资源，则该子资源名称由父资源名紧接着子资源的ID 组成，同样使用正向斜杠分隔。

例1：一个存储服务，其有一个桶集合，每个桶又有一个对象集合：

| API服务名称 | 集合ID | 资源ID | 集合ID | 资源ID |
| --- | --- | --- | --- | --- |
| //storage.googleapis.com | /buckets | /bucket-id | /objects | /object-id |

例2：一个Email服务有一个用户集合。每个用户有一个`settings`子资源，`settings`子资源又有一些别的子资源，比如`customFrom`:

| API服务名称 | 集合ID | 资源ID | 集合ID | 资源ID |
| --- | --- | --- | --- | --- |
| //mail.googleapis.com | /users | /name@example.com| /settings | /customFrom |

## 完整资源名

一个无结构的URI，含有一个可DNS查询的API服务名称和一个资源路径。这个资源路径又被称为相对资源名称，例如：

```
"//library.googleapis.com/shelves/shelf1/books/book2"
```

API服务名称是用来让客户端定位API服务端点用的。对于仅内部使用的服务来说，它可能是一个伪DNS名称。如果API名称在上下文很容易推断出，人们往往使用相对资源名。

## 相对资源名

一个省掉了最前的"/"的URI 路径[path-noscheme](http://tools.ietf.org/html/rfc3986#appendix-A)

```
"shelves/shelf1/books/book2"
```
## 资源 ID

一个将资源从它的父资源中识别出来的非空的URI组件([segment-nz-nc](http://tools.ietf.org/html/rfc3986#appendix-A))，可参考上面的例子。
一个资源名称中的资源ID 可能由不止一个URI组件组成，如：

| 集合ID | 资源ID |
| --- | :-: |
| files | <span class="Apple-tab-span" style="white-space:pre"></span>/source/py/parser.py |

API服务应该尽可能地使用URL 友好的资源ID。 无论是由客户端还是服务器端分配，资源ID 必须被仔细地记录下来。举例来说，文件名称通常由客户端来指定，而email 消息ID 则通常是由服务器端来分配的。

## 集合 ID
## 资源名 vs URL
## 作为字符串的资源名

