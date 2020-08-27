---
date: 2020-02-01T11:00:00+08:00
title: 服务调用的初始化
menu:
  main:
    parent: "service-invoke-sources"
weight: 581
description : "Dapr服务调用的初始化流程和源码分析"
---



```
func RegisterDaprServer(s *grpc.Server, srv DaprServer) {
	s.RegisterService(&_Dapr_serviceDesc, srv)
}
```

_Dapr_serviceDesc 中有dpar各个方法的定义，包括 InvokeService

```go
var _Dapr_serviceDesc = grpc.ServiceDesc{
	ServiceName: "dapr.proto.runtime.v1.Dapr",
	HandlerType: (*DaprServer)(nil),
	Methods: []grpc.MethodDesc{
		{
			MethodName: "InvokeService",
			Handler:    _Dapr_InvokeService_Handler,
		},
		......
	},
	Streams:  []grpc.StreamDesc{},
	Metadata: "dapr/proto/runtime/v1/dapr.proto",
}
```



