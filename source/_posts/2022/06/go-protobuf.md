---
title: "GO: Proto Buffer"
date: 2022-06-01 21:57:43
categories: [Tech, Go]
tags:
    - go
    - grpc
---
Doc: 
https://developers.google.com/protocol-buffers/docs/gotutorial

### Proto Buffer
Download Proto Buffer:
https://developers.google.com/protocol-buffers/docs/downloads

Check if `protoc` installed
```shell
$ protoc --version
```

### Install `protoc-gen-go`
```shell
$ go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
```
Add these text to `~/.bashrc` or `~/.zshrc` (depends on what shell you use)
```text
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```
This step is important to avoid `Error "protoc-gen-go: program not found or is not executable"`

### `.proto` File
`.proto` example:
```
syntax = "proto3";

package proto;

option go_package ="../proto"; // avoid "unable to determine go import path"

message Request {
    int64 a = 1;
    int64 b = 2;
}

message Response {
    int64 result = 1;
}

service AddService {
    rpc Add(Request) returns (Response);
    rpc Multiply(Request) returns (Response);
}
```

Scaffolding:
```text
project/
└── proto/
   └── package.proto

```

### Compile
```shell
$ cd proto
$ protoc --go_out=plugins=grpc:. service.proto   
```
`plugins=grpc` is needed or the output file will lack some functions

### Install `grpc`
```shell
$ go get -u google.golang.org/grpc
```

### Reference
- How to Solve Error: unable to determine go import path: https://developpaper.com/protocol-gen-go-unable-to-determine-go-import-path-when-exporting-protocol/