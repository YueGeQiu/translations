# Google API 设计指南 - 面向资源的设计

原文地址：https://cloud.google.com/apis/design/resources
Copyright: Creative Commons Attribution 3.0 License
Current Version of the API Design Guide: 2017-02-21
翻译日期: 2月23日，2017

## 面向资源的设计

本指南的目标是帮助开发者设计出简介、一致且好用的网络API 。与此同时，此指南也有助于统一基于socket 的RPC API和基于HTTP 的REST API 的设计。

长久以来，人们通过API接口和方法，如CORBA 和Windows COM 来设计RPC API。随着时间流逝，越来越多的接口和方法被引入。最终的结果将是数目惊人且各不相同的接口和方法。为了正确的使用它们，开发者不得不得进行仔细的学习，这不仅耗时而且易错。

[REST](http://en.wikipedia.org/wiki/Representational_state_transfer)风格体系最早在2000年被提出，并被设计为配合HTTP/1.1工作。REST的核心原则是定义可被少许方法进行操作的命名资源。这些资源和方法被称为API 的名词 (nouns) 和动词 (verb)。在HTTP协议下，资源名很自然地被映射到URL上而方法则映射到HTTP方法`POST` `GET` `PUT` `PATCH` 和 `DELETE`上。

在因特网上，HTTP REST API 最近获得了巨大的成功。在2010年，将近74%的公开网络API 是HTTP REST API。

尽管HTTP REST API 在因特网上非常流行，但其传送的流量却少于传统的RPC API。例如：在美国大约一半的高峰期网络流量是视频内容，而由于性能原因，没有人会考虑使用REST API 去传送这些内容。在数据中心内部，许多公司使用基于socket的RPC API 去承载大部分网络流量，而这些流量可能比公开REST API 上的大上几个数量级。

现实中，RPC API 和 HTTP REST API 都有许多不同的使用理由。理想情况下，一个API平台应该为所有的API提供最好的支持。本指南帮助你设计和构造符合此原则的API。其使用面向资源的设计原则去设计范用API，并且规定了许多通用的设计模式去增加可用性、降低复杂度。

**注意：** 本指南解释了如何在不依赖编程语言，操作系统和网络协议的情况下将REST 原则应用于API 设计。它**并不是**一份仅仅关于构造REST API 的指南。

### 什么是REST API ？

REST API 是一系列个体可描述 (individually-addressable) 的资源（API的名词）的模型。资源可以通过他们的[资源名称](https://cloud.google.com/apis/design/resource_names)来提及，并可以通过一个小集合内的方法（即API的动词）来操作。

REST Google API 的标准方法（也被称为REST方法）包括List, Get, Create Update 和Delete。当功能不能轻松地映射到标准方法时，如数据库事务，API设计者也可以使用自定义方法（也被称为自定义动词或自定义操作）。

**注意：** 自定义动词并不意味着创建自定义HTTP动词来实现自定义方法。对于基于HTTP的API，自定义动词会被映射到合适的HTTP动词上。

### 设计流程

The Style Guide suggests taking the following steps when designing resource- oriented APIs (more details are covered in specific sections below):

本指南建议按照下列步骤来设计面向资源的API（更多细节会在以后具体的章节所描述）。

* 确定API提供的资源类型
* 查明不同资源间的关系
* 根据资源的类型和关系，决定资源名称的规范
* 决定资源的范式 (schema)
* 为资源加上方法的最小集合


### 资源 (Resources)

A resource-oriented API is generally modeled as a resource hierarchy, where each node is either a simple resource or a collection resource. For convenience, they are often called as a resource and a collection, respectively.

A collection contains a list of resources of the same type. For example, a user has a collection of contacts.
A resource has some state and zero or more sub-resources. Each sub-resource can be either a simple resource or a collection resource.
For example, Gmail API has a collection of users, each user has a collection of messages, a collection of threads, a collection of labels, a profile resource, and several setting resources.

While there is some conceptual alignment between storage systems and REST APIs, a service with a resource-oriented API is not necessarily a database, and has enormous flexibility in how it interprets resources and methods. For example, creating a calendar event (resource) may create additional events for attendees, send email invitations to attendees, reserve conference rooms, and update video conference schedules.

### 方法（Methods)

The key characteristic of a resource-oriented API is that it emphasizes resources (data model) over the methods performed on the resources (functionality). A typical resource-oriented API exposes a large number of resources with a small number of methods. The methods can be either the standard methods or custom methods. For this guide, the standard methods are: List, Get, Create, Update, and Delete.

Where API functionality naturally maps to one of the standard methods, that method should be used in the API design. For functionality that does not naturally map to one of the standard methods, custom methods may be used. Custom methods offer the same design freedom as traditional RPC APIs, which can be used to implement common programming patterns, such as database transactions or data analysis.

### 示例

接下来的章节通过一些实际的例子展示了如果和对于大规模的服务使用基于资源的API 设计。

#### Gmail API
Gmail API 服务实现了Gmail API 并向使用者暴露了大部分Gmail 的功能。其定义了下列资源模型：

Gmail API 服务: gmail.googleapis.com

* 用户集合: `users/*`  每个用户又拥有下列资源：

    * 消息资源集合: `users/*/messages/*`
    * 用户帖子资源集合: `users/*/threads/*`
    * 标签资源集合: `users/*/labels/*`
    * 修改历史资源集合: `users/*/history/*`
    * 代表用户资料的资源: `users/*/profile`
    * 代表用户设置的资源: `users/*/settings`

#### Google Cloud Pub/Sub API

pubsub.googleapis.com 服务实现了Google Cloud Pub/Sub API, 其定义了下列资源模型： 

* API服务: pubsub.googleapis.com
* 主题资源集合: `projects/*/topics/*`
* 订阅资源集合: `projects/*/subscriptions/*`

**注意：**  其它Pub/Sub API 实现可能采用不同的资源名称范式


