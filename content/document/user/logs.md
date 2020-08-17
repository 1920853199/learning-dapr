---
date: 2020-08-01T11:00:00+08:00
title: Dapr Logs
menu:
  main:
    parent: "document-user"
weight: 213
description : "Dapr Logs"
---

https://github.com/dapr/docs/blob/master/best-practices/troubleshooting/logs.md

本节将帮助您了解Dapr中日志的工作原理，配置和查看日志。

### 概述

日志有不同的、可配置的语义等级。下面列出的等级对于系统组件和Dapr sidecar进程/容器都是一样的：

1. 错误/error
2. 警告/waring
3. 信息/info
4. 调试/debug

error产生最小的输出量，而debug产生最大的输出量。默认级别是 info，它为正常情况下的 Dapr 操作提供了均衡的信息量。

要设置输出级别，可以使用 --log-level 命令行选项。例如：

```bash
./daprd --log-level error
./placement --log-level debug
```

这将启动Dapr运行时二进制文件，其日志级别为 error，Dapr Actor Placement  服务的日志级别为 debug。

### 在单机模式下配置日志

如上所述，每个Dapr二进制文件都需要一个 --log-level 的参数。例如，启动 placement 服务时，日志级别为 warning:

```
./placement --log-level warning
```

当使用Dapr CLI运行应用程序时，要设置日志级别，需要传递 log-level 参数:

```
dapr run --log-level warning node myapp.js
```

### 在单机模式下查看日志
当使用Dapr CLI运行Dapr时，应用的日志输出和运行时的输出都会被重定向到同一个会话，以方便调试。例如，这是运行Dapr时的输出:

```
dapr run node myapp.js
ℹ️  Starting Dapr with id Trackgreat-Lancer on port 56730
✅  You're up and running! Both Dapr and your app logs will appear here.

== APP == App listening on port 3000!
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="starting Dapr Runtime -- version 0.3.0-alpha -- commit b6f2810-dirty"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="log level set to: info"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="standalone mode configured"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="app id: Trackgreat-Lancer"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="loaded component statestore (state.redis)"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="loaded component messagebus (pubsub.redis)"
== DAPR == 2019/09/05 12:26:43 redis: connecting to localhost:6379
== DAPR == 2019/09/05 12:26:43 redis: connected to localhost:6379 (localAddr: [::1]:56734, remAddr: [::1]:6379)
== DAPR == time="2019-09-05T12:26:43-07:00" level=warning msg="failed to init input bindings: app channel not initialized"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="actor runtime started. actor idle timeout: 1h0m0s. actor scan interval: 30s"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="actors: starting connection attempt to placement service at localhost:50005"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="http server is running on port 56730"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="gRPC server is running on port 56731"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="dapr initialized. Status: Running. Init Elapsed 8.772922000000001ms"
== DAPR == time="2019-09-05T12:26:43-07:00" level=info msg="actors: established connection to placement service at localhost:50005"
```



### 在Kubernetes上配置日志

本节介绍了如何配置Dapr system pod和运行在Kubernetes上的Dapr sidecar的日志级别。

#### 设置sidecar的日志级别

您可以通过在您的 pod spec 模板中提供以下注释，为每个 sidecar 单独设置日志级别: 

```yaml
annotations:
  dapr.io/log-level: "debug"
```

#### 设置系统pod日志级别

当使用 Helm 3.x 将 Dapr 部署到集群时，您可以为每个 Dapr 系统组件单独设置日志级别。

##### 设置 operator 日志级别

```
helm install dapr dapr/dapr --namespace dapr-system --set dapr_operator.logLevel=error
```

##### 设置 placement 服务日志级别

```
helm install dapr dapr/dapr --namespace dapr-system --set dapr_placement.logLevel=error
```

##### 设置 sidecar injector的日志级别

```
helm install dapr dapr/dapr --namespace dapr-system --set dapr_sidecar_injector.logLevel=error
```

### 在Kubernetes上查看日志

Dapr日志被写入stdout和stderr。本节将指导您如何查看Dapr系统组件以及Dapr sidecar的日志。

#### sidecar 日志

当部署在Kubernetes中时，Dapr sidecar injetor 将把一个名为 daprd 的Dapr容器注入到你的带注解的pod中。为了查看sidecar的日志，只需通过运行 `kubectl get pods` 找到相关的pod：

```
NAME                                        READY     STATUS    RESTARTS   AGE
addapp-74b57fb78c-67zm6                     2/2       Running   0          40h
```

接下来，获取Dapr sidecar容器的日志:

```
kubectl logs addapp-74b57fb78c-67zm6 -c daprd

time="2019-09-04T02:52:27Z" level=info msg="starting Dapr Runtime -- version 0.3.0-alpha -- commit b6f2810-dirty"
time="2019-09-04T02:52:27Z" level=info msg="log level set to: info"
time="2019-09-04T02:52:27Z" level=info msg="kubernetes mode configured"
time="2019-09-04T02:52:27Z" level=info msg="app id: addapp"
time="2019-09-04T02:52:27Z" level=info msg="application protocol: http. waiting on port 6000"
time="2019-09-04T02:52:27Z" level=info msg="application discovered on port 6000"
time="2019-09-04T02:52:27Z" level=info msg="actor runtime started. actor idle timeout: 1h0m0s. actor scan interval: 30s"
time="2019-09-04T02:52:27Z" level=info msg="actors: starting connection attempt to placement service at dapr-placement.dapr-system.svc.cluster.local:80"
time="2019-09-04T02:52:27Z" level=info msg="http server is running on port 3500"
time="2019-09-04T02:52:27Z" level=info msg="gRPC server is running on port 50001"
time="2019-09-04T02:52:27Z" level=info msg="dapr initialized. Status: Running. Init Elapsed 64.234049ms"
time="2019-09-04T02:52:27Z" level=info msg="actors: established connection to placement service at dapr-placement.dapr-system.svc.cluster.local:80"
```



#### System 日志

Dapr运行以下系统 pods

- Dapr operator
- Dapr sidecar injector
- Dapr placement service

##### 查看 operator 日志

```
kubectl logs -l app=dapr-operator -n dapr-system
time="2019-09-05T19:03:43Z" level=info msg="log level set to: info"
time="2019-09-05T19:03:43Z" level=info msg="starting Dapr Operator -- version 0.3.0-alpha -- commit b6f2810-dirty"
time="2019-09-05T19:03:43Z" level=info msg="Dapr Operator is started"
```

> 注：如果Dapr安装在与 dapr-system 不同的命名空间，只需将上述命令中的命名空间替换为所需的命名空间即可。

##### 查看sidecar injector 日志

```
kubectl logs -l app=dapr-sidecar-injector -n dapr-system
time="2019-09-03T21:01:12Z" level=info msg="log level set to: info"
time="2019-09-03T21:01:12Z" level=info msg="starting Dapr Sidecar Injector -- version 0.3.0-alpha -- commit b6f2810-dirty"
time="2019-09-03T21:01:12Z" level=info msg="Sidecar injector is listening on :4000, patching Dapr-enabled pods"
```

> 注：如果Dapr安装在与dapr-system不同的命名空间，只需将上述命令中的命名空间替换为所需的命名空间即可。

##### 查看 placement service 日志

```
kubectl logs -l app=dapr-placement -n dapr-system
time="2019-09-03T21:01:12Z" level=info msg="log level set to: info"
time="2019-09-03T21:01:12Z" level=info msg="starting Dapr Placement Service -- version 0.3.0-alpha -- commit b6f2810-dirty"
time="2019-09-03T21:01:12Z" level=info msg="placement Service started on port 50005"
time="2019-09-04T00:21:57Z" level=info msg="host added: 10.244.1.89"
```

> 注：如果Dapr安装在与dapr-system不同的命名空间，只需将上述命令中的命名空间替换为所需的命名空间即可。

### 非k8s环境

上面的例子是专门针对Kubernetes的，但对于任何一种基于容器的环境来说，原理都是一样的：只需抓取Dapr sidecar和/或系统组件（如果适用）的容器ID并查看其日志。

