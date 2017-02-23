# Google API 设计指南 - 前言

原文地址：https://cloud.google.com/apis/design/
Copyright: Creative Commons Attribution 3.0 License
Current Version of the API Design Guide: 2017-02-21
翻译日期: 2月23日，2017

## 前言
 这是一份适用于网络API的通用指南。本指南自2014 年起在Google 内部使用，并且是我们设计[Cloud API](https://cloud.google.com/apis/docs/overview) 和其它[Google API](https://cloud.google.com/apis/docs/overview) 时所遵循的依据。我们将这份指南分享出来供外部的开发者参考，使我们之间的共同开发变得轻松。
 
 外部开发者可能会在设计配合[Google Cloud Endpoints](https://cloud.google.com/endpoints/docs/grpc) 使用的gRPC API 时觉得本指南尤其有用, 且我们强烈推荐此类开发者遵从这些设计原则。不过我们并不强求任何非谷歌的开发者遵循本原则并且你完全可以在不参照本指南的前提下使用Cloud Endpoints 和/或gRPC 。
 
 本指南对REST API 和RPC API 均为适用，并对gRPC API 有特别的关注。gPRC API 使用[Protocol Buffers](https://cloud.google.com/apis/design/proto3)去定义API 表层和[API Service Configuration](https://github.com/googleapis/googleapis)去配置其API 服务，包括HTTP 映射，日志和监控。Google API 和gRPC Cloud Endpoints 使用HTTP 映射功能进行JSON/HTTP 到Protocol Buffers/RPC的[转码](https://cloud.google.com/endpoints/docs/transcoding)。
 
 本指南是一份不断变化的文档，不断被采用、接纳的新风格和设计模式会不断地被添加进来。在这种指导精神下，本指南不会终结且在追寻API 设计的艺术及匠心上将一直都会有进步空间。

##文档用语

不同级别的要求类词语: 
绝对要求："MUST", "REQUIRED", "SHALL"
绝对不要："MUST NOT", "SHALL NOT", 
一般应该："SHOULD", "RECOMMENDED", 
一般不要："SHOULD NOT",
可能, 可选： "MAY", "OPTIONAL" 

在本文中使用解释参照其在[RFC 2119](https://www.ietf.org/rfc/rfc2119.txt)中的描述。
在本文档中，这些关键词由**粗体**高亮标示。

## 参考资料

https://www.ibm.com/developerworks/cn/linux/l-cn-gpb/
http://www.ruanyifeng.com/blog/2007/03/rfc2119.html


