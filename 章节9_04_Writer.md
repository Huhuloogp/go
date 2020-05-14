1、输出流Writer接口

type Writer interface {
	Write(p []byte) (n int, err error)
}

2、输入流时不要使用`os.Open()`因为这种方式获取的文件是只读的，OpenFile可修改

```go
fp := "D:/go.txt"
f, err := os.OpenFile(fp, os.O_APPEND, 0660)
defer f.Close()
if err != nil {
	fmt.Println("文件不存在,创建文件")
	f, _ = os.Create(fp)
}
f.Write([]byte("使用Writer接口写数据\r\n"))

//更加好用，可点进去查看，本质上还是write，但会把内容自动转换为byte类型
f.WriteString("写了\t一段\r\n内容123")	//重新封装了方法
	fmt.Println("程序执行结束")
```



# 一. 输入流

* 输入流就是把程序中数据写出到外部资源
* Go语言标准库中输出流是Writer接口
```go
// Writer is the interface that wraps the basic Write method.
//
// Write writes len(p) bytes from p to the underlying data stream.
// It returns the number of bytes written from p (0 <= n <= len(p))
// and any error encountered that caused the write to stop early.
// Write must return a non-nil error if it returns n < len(p).
// Write must not modify the slice data, even temporarily.
//
// Implementations must not retain p.
type Writer interface {
	Write(p []byte) (n int, err error)
}
```


# 二.代码操作

* 注意:输入流时不要使用`os.Open()`因为这种方式获取的文件是只读的
```go
	fp := "D:/go.txt"
	/*
	第三个参数表示文件权限
	第 1 位在权限中总是为 0
	第 2 位为 0 表示文件不可以被读， 为 1 表示可以被读
	第 3 位为 0 表示文件不可以被写， 为 1 表示可以被写
	第 4 位为 0 表示文件不可以被执行， 为 1 表示可以被执行
	整理如下:
	   0(0000): 不可读写,不能被执行
	   1(0001): 不可读写,能被执行
	   2(0010): 可写不可读,不能被执行
	   3(0011): 可写不可读,能被执行
	   4(0100): 可读不可写,不能被执行
	   5(0101): 可读不可写,能被执行
	   6(0110): 可读写,不能执行
	   7(0111): 可读写,可执行

	0666:
	第一个 0 表示这个数是 八进制
	第一个 6 表示文件拥有者有读写权限，但没有执行权限
	第二个 6 表示文件拥有者同组用户有读写权限，但没有执行权限
	第三个 6 表示其它用户有读写权限，但没有执行权限

	 */

	//第二个参数表示文件内容追加
	//第三个参数表示创建文件时文件权限
	f, err := os.OpenFile(fp, os.O_APPEND, 0660)
	defer f.Close()
	if err != nil {
		fmt.Println("文件不存在,创建文件")
		f, _ = os.Create(fp)
	}

	/*
	内容中识别特殊字符
	\r\n 换行
	\t 缩进
	 */

	/*
	使用文件对象重写的Writer接口,参数是[]byte
	 */
	f.Write([]byte("使用Writer接口写数据\r\n"))

	/*
	使用stringWriter接口的方法,参数是字符串,使用更方便
	 */
	f.WriteString("写了\t一段\r\n内容123")
	fmt.Println("程序执行结束")
```