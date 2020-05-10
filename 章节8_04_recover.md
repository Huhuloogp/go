```
func main() {
	defer func() {
		if error:=recover();error!=nil{
			fmt.Println("出现了panic,使用reover获取信息:",error)
		}
	}()
	fmt.Println("11111111111")
	panic("出现panic")
	fmt.Println("22222222222")
}


```

1、recover()表示恢复程序的panic(),让程序正常运行

2、recover()一般用在defer内部

3、recover()只能恢复当前函数级或**当前函数调用函数中的panic()**，恢复后调用当前级别函数结束,**但是调用此函数的函数可以继续执行**

func demo1(){
	fmt.Println("demo1上半部分")
	demo2()
	fmt.Println("demo1下半部分")
}
func demo2(){
	defer func() {
		recover()//此处进行恢复				//当前函数调用的函数里面有panic
	}()
	fmt.Println("demo2上半部分")
	demo3()
	fmt.Println("demo2下半部分")
}
func demo3(){
	fmt.Println("demo3上半部分")
	panic("在demo3出现了panic")					//此处出现了panic,按道理main()里面的demo1将会停止，但是遇到了recover
	fmt.Println("demo3下半部分")
}

func main() {
	fmt.Println("程序开始")
	demo1()
	fmt.Println("程序结束")
}







recover

* recover()表示恢复程序的panic(),让程序正常运行
* recover()是和panic(v)一样都是builtin中函数,可以接收panic的信息,恢复程序的正常运行
```go
// The recover built-in function allows a program to manage behavior of a
// panicking goroutine. Executing a call to recover inside a deferred
// function (but not any function called by it) stops the panicking sequence
// by restoring normal execution and retrieves the error value passed to the
// call of panic. If recover is called outside the deferred function it will
// not stop a panicking sequence. In this case, or when the goroutine is not
// panicking, or if the argument supplied to panic was nil, recover returns
// nil. Thus the return value from recover reports whether the goroutine is
// panicking.
func recover() interface{}
```
* recover()一般用在defer内部,如果没有panic信息返回nil,如果有panic,recover会把panic状态取消
```go
func main() {
	defer func() {
		if error:=recover();error!=nil{
			fmt.Println("出现了panic,使用reover获取信息:",error)
		}
	}()
	fmt.Println("11111111111")
	panic("出现panic")
	fmt.Println("22222222222")
}
```

* 输出
```
11111111111
出现了panic,使用reover获取信息: 出现panic
```

# 二.函数调用过程中panic和recover()
* recover()只能恢复当前函数级或当前函数调用函数中的panic(),恢复后调用当前级别函数结束,但是调用此函数的函数可以继续执行.
* panic会一直向上传递,如果没有recover()则表示终止程序,但是碰见了recover(),recover()所在级别函数表示没有panic,panic就不会向上传递
```go
func demo1(){
	fmt.Println("demo1上半部分")
	demo2()
	fmt.Println("demo1下半部分")
}
func demo2(){
	defer func() {
		recover()//此处进行恢复
	}()
	fmt.Println("demo2上半部分")
	demo3()
	fmt.Println("demo2下半部分")
}
func demo3(){
	fmt.Println("demo3上半部分")
	panic("在demo3出现了panic")
	fmt.Println("demo3下半部分")
}
func main() {
	fmt.Println("程序开始")
	demo1()
	fmt.Println("程序结束")
}
```