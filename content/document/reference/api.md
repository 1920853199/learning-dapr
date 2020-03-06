---
date: 2020-02-01T11:00:00+08:00
title: Dapr API 参考文档
menu:
  main:
    parent: "document-reference"
weight: 231
description : "Dapr API 参考文档"
---

> 内容摘选自：https://github.com/dapr/docs/tree/master/reference/api

通过 HTTP1.1/2.0 和 gRPC 等通用协议，以平台和语言无关的方式，提供构建块。

### 目标

对Dapr API的任何更改/建议均应遵守以下规定：

- 必须与平台无关
- 必须与编程语言无关
- 必须向后兼容

备注：与平台无关和与编程语言无关容易理解，但是项目一开始就刻意强调必须向后兼容，会很束手束脚吧？