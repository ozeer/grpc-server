## 一、安装gRPC
1、安装Go的gRPC库，在项目根目录执行以下命令
```
go get google.golang.org/grpc@latest
```

2、安装Protocol Buffers v3
下载地址
```
https://github.com/google/protobuf/releases
```

3、因为我们服务端是Go语言开发，所以我们执行以下命令安装protoc的Go插件
安装Go语言插件：
```
go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.31.0
```
该插件会根据.proto文件生成一个后缀为.pb.go的文件，包含所有.proto文件中定义的类型及其序列化方法。

安装grpc插件（）：
```
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.3.0
```
该插件会生成一个后缀为_grpc.pb.go的文件，其中包含：
- 一种接口类型(或存根) ，供客户端调用的服务方法。
- 服务器要实现的接口类型。

上述命令会默认将插件安装到$GOPATH/bin，为了protoc编译器能找到这些插件，请确保你的$GOPATH/bin在环境变量中。

4、检查上述插件安装是否成功
确认 protoc 安装完成。
```
❯ protoc --version
libprotoc 3.20.1
````
确认 protoc-gen-go 安装完成。

```
❯ protoc-gen-go --version
protoc-gen-go v1.28.0
```
如果这里提示protoc-gen-go不是可执行的程序，请确保你的 GOPATH 下的 bin 目录在你电脑的环境变量中。

确认 protoc-gen-go-grpc 安装完成。

```
❯ protoc-gen-go-grpc --version
protoc-gen-go-grpc 1.2.0
```

## 二、开始gRPC开发（以Go语言为例）
1、编写hello.proto文件，例如，下面使用 protocol buffers 定义了一个HelloService服务。
```
// 版本声明，使用Protocol Buffers v3版本
syntax = "proto3";

// 指定生成的Go代码在你项目中的导入路径
option go_package = "server/pb";

// 包名
package pb;

// 定义服务
service Greeter {
    // SayHello 方法
    rpc SayHello (HelloRequest) returns (HelloResponse) {}
}

// 请求消息
message HelloRequest {
    string name = 1;
}

// 响应消息
message HelloResponse {
    string reply = 1;
}

```

2、在Server端和Client端生成指定语言的代码（此处我们生成Go语言的gRPC代码），在.proto文件的目录下执行以下命令

```
protoc --go_out=. hello.proto
protoc --go-grpc_out=. hello.proto
```

3、编写各端业务逻辑代码
剩下的工作就是要编写业务逻辑代码。在服务端编写业务代码实现具体的服务方法，在客户端按需调用这些方法。

## 三、gRPC服务方法类型说明

在gRPC中你可以定义四种类型的服务方法。

- 普通 rpc，客户端向服务器发送一个请求，然后得到一个响应，就像普通的函数调用一样。
```
rpc SayHello(HelloRequest) returns (HelloResponse);
```
- 服务器流式 rpc，其中客户端向服务器发送请求，并获得一个流来读取一系列消息。客户端从返回的流中读取，直到没有更多的消息。gRPC 保证在单个 RPC 调用中的消息是有序的。
```
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse);
```
- 客户端流式 rpc，其中客户端写入一系列消息并将其发送到服务器，同样使用提供的流。一旦客户端完成了消息的写入，它就等待服务器读取消息并返回响应。同样，gRPC 保证在单个 RPC 调用中对消息进行排序。
```
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse);
```
- 双向流式 rpc，其中双方使用读写流发送一系列消息。这两个流独立运行，因此客户端和服务器可以按照自己喜欢的顺序读写: 例如，服务器可以等待接收所有客户端消息后再写响应，或者可以交替读取消息然后写入消息，或者其他读写组合。每个流中的消息是有序的。

## 四、普通rpc

## 五、流式rpc
### 1、服务端流式RPC

### 2、客户端流式RPC

### 3、双向流式RPC

## 六、metadata

## 七、错误处理

## 八、加密或认证

## 九、拦截器（中间件）

## 十、开发过程中的一些常见问题及解决方案

## 十一、引用
- https://www.liwenzhou.com/posts/Go/gRPC/
