---
date: 2020-02-01T11:00:00+08:00
title: 状态管理的参考文档
menu:
  main:
    parent: "pub-sub"
weight: 801
description : "Dapr状态管理的参考文档"
---

详细见：

https://github.com/dapr/docs/blob/master/reference/api/state.md

Dapr 提供了一个可靠的状态端点，允许开发人员通过API保存和检索状态。Dapr具有可插拔架构，并允许绑定到多个云/内部状态存储。

### key scheme

Dapr状态存储是键/值存储。为了确保数据兼容性，Dapr要求这些数据存储遵循固定的 key scheme。对于一般状态，key 的格式为：

```
<Dapr id>||<state key>
```

对于Actor状态，密钥格式为：

```
<Dapr id>||<Actor type>||<Actor id>||<state key>
```

细节看原文吧。