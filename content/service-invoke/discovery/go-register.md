---
date: 2020-02-01T11:00:00+08:00
title: go-sdk中的服务注册
menu:
  main:
    parent: "service-invoke-discovery"
weight: 531
description : "Dapr go-sdk中的服务注册"
---



### AppCallback Server启动时没有注册

go-sdk 中的 `service/grpc/service.go` gRPC 版本的 AppCallback Server启动时没有做服务注册。

```go
func (s *Server) Start() error {
	gs := grpc.NewServer()
	pb.RegisterAppCallbackServer(gs, s) // 仅仅是注册 AppCallback Service 到 grpc server，不是服务注册
	return gs.Serve(s.listener)
}
```

`service/http/service.go`  HTTP 版本的  AppCallback Server启动时没有做服务注册。

```go
func (s *Server) Start() error {
	s.registerSubscribeHandler() // 仅仅是注册 SubscribeHandler，不是服务注册
	server := http.Server{
		Addr:    s.address,
		Handler: s.mux,
	}
	return server.ListenAndServe()
}
```

