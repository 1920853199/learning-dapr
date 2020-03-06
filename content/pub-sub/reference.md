---
date: 2020-02-01T11:00:00+08:00
title: 发布订阅消息的参考文档
menu:
  main:
    parent: "pub-sub"
weight: 602
description : "Dapr 发布订阅消息的参考文档"
---

详细见：

https://github.com/dapr/docs/blob/master/reference/api/pubsub.md

发布消息到topic，只要发一个HTTP POST 请求到 dapr sidecar：

```
POST http://localhost:<daprPort>/v1.0/publish/<topic>
```

要接受消息，首先应用在这个地址接受 sidecar 请求，告知 dapr 自己要订阅的 topic：

```
GET http://localhost:<appPort>/dapr/subscribe

应答如：

"["TopicA","TopicB"]"
```

然后应用在这个地址接受sidecar 转发的订阅消息：

```
POST http://localhost:<appPort>/TopicA
```

### 消息封套

Dapr Pub/Sub 遵守 Cloud Events  0.3 版本。