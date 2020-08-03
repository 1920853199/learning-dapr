---
date: 2020-08-01T11:00:00+08:00
title: Dapr FAQ
menu:
  main:
    parent: "document-user"
weight: 218
description : "Dapr FAQ"
---

https://github.com/dapr/docs/blob/master/FAQ.md

## 网络和服务网格

### 了解Dapr如何与服务网格一起工作

Dapr是一个分布式应用运行时。与服务网格专注于网络问题不同，Dapr专注于提供构建块，使开发者更容易构建微服务。Dapr是以开发者为中心的，而服务网格是以基础设施为中心的。

**Dapr is developer-centric versus service meshes being infrastructure-centric.**

Dapr可以与任何服务网格（如Istio和Linkerd）一起使用。服务网格是一个专门的网络基础设施层，旨在将服务相互连接起来，并提供深入的遥测。**服务网格不会为应用引入新功能**。

这就是Dapr的作用。Dapr是一个建立在http和gRPC上的语言无关的编程模型，它通过开放的API为异步pub-sub、有状态服务、服务发现和调用、actor和分布式跟踪提供分布式系统构建块。Dapr为应用程序的运行时引入了新的功能。服务网格和Dapr都可以作为应用程序的Sidecar服务运行，一个提供网络功能，另一个提供分布式应用能力。

观看[这段视频](https://www.youtube.com/watch?v=xxU68ewRmz8&feature=youtu.be&t=140)，了解Dapr和服务网格如何协同工作。

### 了解 Dapr 如何与服务网格接口 (SMI) 互操作

SMI是一个抽象层，它为不同的服务器网格技术提供了一个通用的API表层。Dapr可以利用任何服务网格技术，包括SMI。

### Dapr、Istio和Linkerd之间的区别

Istio是一个开源的服务网格实现，专注于服务之间的Layer7路由、流量管理和mTLS认证。Istio使用sidecar来拦截进出容器的流量，并对它们执行一套网络策略。

Istio不是一个编程模型，也不关注应用层面的功能，如状态管理、pub-sub、绑定等。这就是Dapr的优势所在。

## Actor

### Dapr与Orleans和Service Fabric Reliable Actors的关系如何？

Dapr中的Actor是基于和Orleans开始的相同的虚拟Actor概念，这意味着它们在被调用时被激活，并在一段时间后被垃圾回收。如果你熟悉Orleans，Dapr的C# actor就会很熟悉。Dapr C# actor是基于 Service Fabric Reliable Actors（也是来自于Orleans），让你能够把Service Fabric中的Reliable Actors迁移到其他托管平台，比如Service Fabric Mesh、Kubernetes或其他内部环境。另外Dapr的意义不仅仅是actor。它为您提供了一套最佳实践构建块，以构建到任何微服务应用程序中。

### Dapr与actor框架有何不同？

虚拟actor功能是Dapr在运行时提供的构建块之一。由于Dapr是编程语言无关的，提供 http/gRPC API，所以可以从任何语言中调用actor。这使得用一种语言编写的actors可以调用用另一种语言编写的actors。

Dapr运行时SDK有特定语言的actor框架。例如.NET SDK就有C#角色。你会发现所有的SDK都有一个适合语言的actor框架。

## 开发语言SDK和框架

### 如果我想使用特定的编程语言或框架，Dapr是否有任何SDK？

为了使Dapr在不同语言下的使用更加自然，它包括了针对Go、Java、JavaScript、.NET和Python的语言专用SDK。这些SDK通过类型化的语言API，而不是调用http/gRPC API来暴露 Dapr构建块中的功能，如保存状态、发布事件或创建actor。这使得你可以用自己选择的语言来编写无状态和有状态的函数和actor的组合。由于这些SDK共享Dapr运行时，你可以获得跨语言的actor和函数支持。

Dapr可以与任何开发框架集成。例如，在Dapr .NET SDK中，你可以找到ASP.NET Core集成，它带来了有状态的路由控制器，可以响应其他服务的pub/sub事件。