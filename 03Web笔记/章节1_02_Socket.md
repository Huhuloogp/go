1、Socket在TCP/IP网络分层中并不存在,是对TCP或UDP封装，





# 一.Socket简介

* 在标准库的net包中可供了可移植的网络I/O接口,其中就包含了Socket
* Socket在TCP/IP网络分层中并不存在,是对TCP或UDP封装
* 如果非要给Socket一个解释
  * 实现网络上双向通讯连接的一套API
  * 常称Socket为"套接字"
* Socket分类:
  * 按照连接时间
    * 短连接
    * 长连接(HTTP 1.1开始也支持长连接,Socket替换方案)
  * 按照客户端和服务器端数量
    * 点对点
    * 点对多
    * 多对多
* 网络通信内容都是包含客户端和服务端,服务端运行在服务器中,而客户端运行在客户端中,可以是浏览器,可以是桌面程序,也可以是手机App.客户端和服务端进行数据交互遵守特定的协议.

# 二.Go语言对Socket的支持

* TCPAddr结构体表示服务器IP和端口
  * IP是`type IP []byte`
  * Port是服务器监听的接口
```go
// TCPAddr represents the address of a TCP end point.
type TCPAddr struct {
	IP   IP
	Port int
	Zone string // IPv6 scoped addressing zone
}
```
* TCPConn结构体表示连接,封装了数据读写操作
```go
// TCPConn is an implementation of the Conn interface for TCP network
// connections.
type TCPConn struct {
	conn
}
```
* TCPListener负责监听服务器端特定端口
```go
// TCPListener is a TCP network listener. Clients should typically
// use variables of type Listener instead of assuming TCP.
type TCPListener struct {
	fd *netFD
}
```

# 三.客户端向服务端发送消息

* 服务端代码
```go
package main

import (
   "net"
   "fmt"
)

func main() {
   //创建TCPAddress变量,指定协议tcp4,监听本机8899端口
   addr, _ := net.ResolveTCPAddr("tcp4", "localhost:8899")

   //监听TCPAddress设定的地址
   lis, _ := net.ListenTCP("tcp4", addr)

   fmt.Println("服务器已启动")

   //阻塞式等待客户端消息,返回连接对象,用于接收客户端消息或向客户端发送消息
   conn, _ := lis.Accept()

   //把数据读取到切片中
   b := make([]byte, 256)
   fmt.Println("read之前")
   //客户端没有发送数据且客户端对象没有关闭,Read()将会阻塞,一旦接收到数据就不阻塞
   count, _ := conn.Read(b)
   fmt.Println("接收到的数据:", string(b[:count]))
   //关闭连接
   conn.Close()
   fmt.Println("服务器结束")
}
```
* 客户端代码
```go
package main

import (
   "net"
   "fmt"
)

func main() {
   //服务器端ip和端口
   addr, _ := net.ResolveTCPAddr("tcp4", "localhost:8899")
   //申请连接客户端
//第二个参数:本地地址  第三个参数:远程地址
   conn, _ := net.DialTCP("tcp4", nil, addr)
   //向服务端发送数据
   count, _ := conn.Write([]byte("客户端传递的数据"))
   fmt.Println("客户端向服务端发送的数据量为:", count)
   //通过休眠测试客户端对象不关闭,服务器是否能接收到对象
   //time.Sleep(10 * time.Second)
   ////关闭连接
   conn.Close()
   //fmt.Println("客户端结束")

}
```