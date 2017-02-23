# Google API 设计指南 - 基于资源的设计

原文地址：https://cloud.google.com/apis/design/resources
Copyright: Creative Commons Attribution 3.0 License
Current Version of the API Design Guide: 2017-02-21
翻译日期: 2月23日，2017

## 基于资源的设计

本指南的目标是帮助开发者设计出简介、一致且好用的网络API 。与此同时，此指南也有助于统一基于socket 的RPC API和基于HTTP 的REST API 的设计。

长久以来，人们通过API接口和方法，如CORBA 和Windows COM 来设计RPC API。随着时间流逝，越来越多的接口和方法被引入。最终的结果将是数目惊人且各不相同的接口和方法。为了正确的使用它们，开发者不得不得进行仔细的学习，这不仅耗时而且易错。

[REST](http://en.wikipedia.org/wiki/Representational_state_transfer)风格体系最早在2000年被提出，并被设计为配合HTTP/1.1工作。REST的核心原则是定义可被少许方法进行操作的命名资源。这些资源和方法被称为API 的名词 (nouns) 和动词 (verb)。在HTTP协议下，资源名很自然地被映射到URL上而方法则映射到HTTP方法`POST` `GET` `PUT` `PATCH` 和 `DELETE`上。

在因特网上，HTTP REST API 最近获得了巨大的成功。在2010年，将近74%的公开网络API 是HTTP REST API。

尽管HTTP REST API 在因特网上非常流行，但其传送的流量却少于传统的RPC API。例如：在美国大约一半的高峰期网络流量是视频内容，而由于性能原因，没有人会考虑使用REST API 去传送这些内容。在数据中心内部，许多公司使用基于socket的RPC API 去承载大部分网络流量，而这些流量可能比公开REST API 上的大上几个数量级。

现实中，RPC API 和 HTTP REST API 都有许多不同的使用理由。理想情况下，一个API平台应该为所有的API提供最好的支持。本指南帮助你设计和构造符合此原则的API。其使用基于资源的设计原则去设计范用API，并且规定了许多通用的设计模式去增加可用性、降低复杂度。

**注意：** 本指南解释了如何在不依赖编程语言，操作系统和网络协议的情况下将REST 原则应用于API 设计。它**并不是**一份仅仅关于构造REST API 的指南。

### 什么是REST API ？

A REST API is modeled as collections of individually-addressable resources (the nouns of the API). Resources are referenced with their resource names and manipulated via a small set of methods (also known as verbs or operations).

Standard methods for REST Google APIs (also known as REST methods) are List, Get, Create, Update, and Delete. Custom methods (also known as custom verbs or custom operations) are also available to API designers for functionality that doesn't easily map to one of the standard methods, such as database transactions.

NOTE: Custom verbs does not mean creating custom HTTP verbs to support custom methods. For HTTP-based APIs, they simply map to the most suitable HTTP verbs.

### 设计流程

The Style Guide suggests taking the following steps when designing resource- oriented APIs (more details are covered in specific sections below):

Determine what types of resources an API provides.
Determine the relationships between resources.
Decide the resource name schemes based on types and relationships.
Decide the resource schemas.
Attach minimum set of methods to resources.

### 资源 (Resources)

A resource-oriented API is generally modeled as a resource hierarchy, where each node is either a simple resource or a collection resource. For convenience, they are often called as a resource and a collection, respectively.

A collection contains a list of resources of the same type. For example, a user has a collection of contacts.
A resource has some state and zero or more sub-resources. Each sub-resource can be either a simple resource or a collection resource.
For example, Gmail API has a collection of users, each user has a collection of messages, a collection of threads, a collection of labels, a profile resource, and several setting resources.

While there is some conceptual alignment between storage systems and REST APIs, a service with a resource-oriented API is not necessarily a database, and has enormous flexibility in how it interprets resources and methods. For example, creating a calendar event (resource) may create additional events for attendees, send email invitations to attendees, reserve conference rooms, and update video conference schedules.

### 方法（Methods)

### 示例

接下来的章节通过一些实际的例子展示了如果和对于大规模的服务使用基于资源的API 设计。

#### Gmail API
#### Google Cloud Pub/Sub API


