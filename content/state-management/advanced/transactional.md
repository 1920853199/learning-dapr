---
date: 2020-02-01T11:00:00+08:00
title: 状态管理的事务性
menu:
  main:
    parent: "state-management-advanced"
weight: 883
description : "状态管理中的事务性"
---

## 设计分析

如果 state store 要支持事务，则要求实现 TransactionalStore 接口：

```go
type TransactionalStore interface {
   // Init方法是和普通store接口一致的
   Init(metadata Metadata) error
   // 增加的是 Multi 方法
   Multi(request *TransactionalStateRequest) error
}
```

Runtime ExecuteStateTransaction 方法会调用 state store 的 multi 方法。

## 实现分析

### Redis实现

dapr redis state store的事务实现，是通过 redis-go 封装的 TxPipeline 实现的。

TODO：

- redis-go 如何实现的
- redis如何实现事务？multi？

