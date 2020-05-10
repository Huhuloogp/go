1、panic有点类似与其他编程语言的throw,抛出异常.当执行到panic后终止剩余代码执行.并打印错误栈信息

2、注意panic不是立即停止程序(os.Exit(0)),defer还是执行的.

3、os.Exit(0)，停止程序，不管你后面有什么

4、



# 一. panic

* panic是builtin中函数
```go
// The panic built-in function stops normal execution of the current
// goroutine. When a function F calls panic, normal execution of F stops
// immediately. Any functions whose execution was deferred by F are run in
// the usual way, and then F returns to its caller. To the caller G, the
// invocation of F then behaves like a call to panic, terminating G's
// execution and running any deferred functions. This continues until all
// functions in the executing goroutine have stopped, in reverse order. At
// that point, the program is terminated and the error condition is reported,
// including the value of the argument to panic. This termination sequence
// is called panicking and can be controlled by the built-in function
// recover.
func panic(v interface{})
```
* panic有点类似与其他编程语言的throw,抛出异常.当执行到panic后终止剩余代码执行.并打印错误栈信息
```go
func main() {
   fmt.Println("1")
   panic("panic执行了,哈哈")
   fmt.Println("2")
}
```
* 执行结果
```
1
panic: panic执行了,哈哈

goroutine 1 [running]:
main.main()
	D:/gowork/c/main.go:7 +0x80
```
* 注意panic不是立即停止程序(os.Exit(0)),defer还是执行的.


```go
func main() {
   defer func(){
      fmt.Println("defer执行")
   }()
   fmt.Println("1")
   panic("panic执行了,哈哈")
   fmt.Println("2")
}
```

