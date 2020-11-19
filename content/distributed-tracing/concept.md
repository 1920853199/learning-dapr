---
date: 2020-02-01T11:00:00+08:00
title: 分布式追踪的概念
menu:
  main:
    parent: "distributed-tracing"
weight: 901
description : "Dapr分布式追踪的概念"
---

> 内容节选自：https://docs.dapr.io/developing-applications/building-blocks/observability/tracing/

Dapr 使用 OpenTelemetry（以前称为OpenCensus）进行分布式追踪和指标收集。OpenTelemetry支持各种后端，包括Azure Monitor、Datadog、Instana、Jaeger、SignalFX、Stackdriver、Zipkin等。

![](images/tracing.png)

### 追踪设计

Dapr 向 Dapr sidecar 中添加了 HTTP/gRPC 中间件（middleware）。中间件拦截所有Dapr和应用流量，并自动注入相关ID以跟踪分布式事务。此设计具有以下优点：

- 无需代码检测。将自动跟踪所有流量（追踪级别可配置）。
- 跨微服务的一致追踪行为。追踪是在 Dapr Sidecar 上配置和管理的，因此它在服务中可能保持一致，这些服务是不同团队提供的，并且可能以不同的编程语言编写。
- 可配置和可扩展。通过利用 OpenTelemetry，可以将 Dapr 追踪配置为与流行的追踪后端一起使用，包括客户可能拥有的自定义后端。
- OpenTelemetry 导出器被定义为一等公民的 Dapr 组件。可以同时定义并启用多个导出器。

## W3C Correlation ID

Dapr使用标准的W3C跟踪上下文头文件。对于HTTP请求，Dapr使用 `traceparent` header。对于gRPC请求，Dapr使用 `grpc-trace-bin` header。当请求到达时没有trace ID时，Dapr会创建新的ID。否则，它将沿着调用链传递跟踪ID。

阅读W3C分布式跟踪，了解更多关于W3C Trace Context的背景。

> 这里有很大变化，早期的文档是这样描述，实现方式和现在的差异很大：
>
> 对于HTTP请求，Dapr会向请求注入 **X-Correlation-ID** header。对于gRPC调用，Dapr插入 **X-Correlation-ID** 作为 header 元数据的字段。当没有 Correlation ID 的请求到达时，Dapr将创建一个新的 Correlation。否则，它将沿调用链传递 Correlation ID。

### 配置

Dapr 跟踪由配置文件（在本地模式下）或Kubernetes配置对象（在Kubernetes模式下）配置。例如，以下配置对象启用分布式跟踪：

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: tracing
spec:
  tracing:
    enabled: true
    expandParams: true
    includeBody: true
```

Dapr 支持可插拔导出器（exporter），导出器由配置文件（在本地模式下）或Kubernetes自定义资源对象（在Kubernetes模式下）定义。例如，以下清单定义了一个 Zipkin 导出器：

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: zipkin
spec:
  type: exporters.zipkin
  metadata:
  - name: enabled
    value: "true"
  - name: exporterAddress
    value: "http://zipkin.default.svc.cluster.local:9411/api/v2/spans"
```





