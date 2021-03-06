！！

1、死锁是把主协程锁了才是死锁，子协程不算

```go
func main() {
   ch := make(chan int)
   go func() {	
      ch <- 1			//子协程里面的死锁
      fmt.Println("执行goroutine")
   }()
   time.Sleep(5e9)
   fmt.Println("程序执行结束")
}
```

2、

var 名称 chan 类型
var 名称 chan <- 类型 //只写
var 名称 <- chan 类型//只读
名称:=make(chan int) //无缓存channel
名称:=make(chan int,0)//无缓存channel
名称:=make(chan int,100)//有缓存channel

3、在主goroutine中向无缓存channel添加内容**或**在主goroutine中向channel添加内容且添加内容的个数已经大于channel缓存个数就会产生死锁

```go
func main() {
   ch := make(chan int, 3) //缓存大小3,里面消息个数小于等于3时都不会阻塞goroutine
   ch <- 1
   ch <- 2
   ch <- 3
   ch <- 4 //此行出现死锁,超过缓存大小数量,可以取出来一个再ch<-4，这样就不死锁
}
```

# 一. 死锁

* 在主goroutine中向无缓存channel添加内容或在主goroutine中向channel添加内容且添加内容的个数已经大于channel缓存个数就会产生死锁
```
fatal error : all goroutines are asleep -deadlock!
```
* 死锁:在程序中多个进程(Golang中goroutine)由于相互竞争资源而产生的阻塞(等待)状态,而这种状态一直保持下去,此时称这个线程是死锁状态
* 在Golang中使用无缓存channel时一定要注意.以下是一个最简单的死锁程序
  * 主协程中有ch<-1,无缓存channel无论添加还是取出数据都会阻塞goroutine,当前程序无其他代码,主goroutine会一直被阻塞下去,此时主goroutine就是死锁状态
```go
func main() {
   ch := make(chan int)
   ch <- 1
}
```
* 而下面代码就不会产生死锁
  * 通过代码示例可以看出,在使用无缓存channel时,特别要注意的是在主协程中有操作channel代码
```go
package main

import (
   "time"
   "fmt"
   
)

func main() {
   ch := make(chan int)
   go func() {
      ch <- 1
      fmt.Println("执行goroutine")
   }()
   time.Sleep(5e9)
   fmt.Println("程序执行结束")
}
```


# 二. 有缓存通道

* 创建一个有缓存通道
```go
func main() {
   ch := make(chan int, 3) //缓存大小3,里面消息个数小于等于3时都不会阻塞goroutine
   ch <- 1
   ch <- 2
   ch <- 3
   ch <- 4 //此行出现死锁,超过缓存大小数量
}
```
* 在Golang中有缓存channel的缓存大小是不能改变的,但是只要不超过缓存数量大小,都不会出现阻塞状态
```go
package main

import "fmt"

func main() {
   ch := make(chan int, 3) //缓存大小3,里面消息个数小于等于3时都不会阻塞goroutine
   ch <- 1
   fmt.Println(<-ch)
   ch <- 2
   fmt.Println(<-ch)
   ch <- 3
   ch <- 4
   fmt.Println(len(ch))//输出2,表示channel中有两个消息
   fmt.Println(cap(ch))//输出3,表示缓存大小总量为3
}
```

