！！

1、在Go语言标准库中io包下是Reader接口表示输入流,只要实现这个接口就属于输入流

type Reader interface {
	Read(p []byte) (n int, err error)
}

2、使用strings包下的NewReader创建字符串流

```go
r := strings.NewReader("hello 世界")
b := make([]byte, r.Size())//创建字节切片,存放流中数据,根据流数据大小创建切片大小
n, err := r.Read(b)//把流中数据读取到切片中
if err != nil {
	fmt.Println("读取失败,", err)
	return
}
fmt.Println("读取数据长度,", n)
fmt.Println("流中数据",string(b))//以字符串形式输入切片中数据
```

3、最常用的是文件流,把外部文件中数据读取到程序中

```go
f, err := os.Open("D:/go.txt")		//第一步先打开文件	
defer f.Close()
if err != nil {
	fmt.Println("文件读取失败,", err)
	return
}
fileInfo, err := f.Stat()//获取文件信息
if err != nil { 
	fmt.Println("文件信息获取失败,", err)
	return
}
b := make([]byte, fileInfo.Size())//根据文件中数据大小创建切片
b, err = f.Read(b)//读取数据到切片中
if err != nil {
	fmt.Println("文件流读取失败:", err)
	return
}
fmt.Println("文件中内容为:", string(b))//以字符串形式输入切片中数据
```
# 一.输入流

* 流(stream)是应用程序和外部资源进行数据交互的纽带

* 流分为输入流和输出流,输入和输出都是相对于程序,把外部数据传入到程序中叫做输入,反之叫做输出流

* 输入流(Input Stream),输入流(Output Stream) 平时所说的I/O流

* 在Go语言标准库中io包下是Reader接口表示输入流,只要实现这个接口就属于输入流
```go
// Reader is the interface that wraps the basic Read method.
//
// Read reads up to len(p) bytes into p. It returns the number of bytes
// read (0 <= n <= len(p)) and any error encountered. Even if Read
// returns n < len(p), it may use all of p as scratch space during the call.
// If some data is available but not len(p) bytes, Read conventionally
// returns what is available instead of waiting for more.
//
// When Read encounters an error or end-of-file condition after
// successfully reading n > 0 bytes, it returns the number of
// bytes read. It may return the (non-nil) error from the same call
// or return the error (and n == 0) from a subsequent call.
// An instance of this general case is that a Reader returning
// a non-zero number of bytes at the end of the input stream may
// return either err == EOF or err == nil. The next Read should
// return 0, EOF.
//
// Callers should always process the n > 0 bytes returned before
// considering the error err. Doing so correctly handles I/O errors
// that happen after reading some bytes and also both of the
// allowed EOF behaviors.
//
// Implementations of Read are discouraged from returning a
// zero byte count with a nil error, except when len(p) == 0.
// Callers should treat a return of 0 and nil as indicating that
// nothing happened; in particular it does not indicate EOF.
//
// Implementations must not retain p.
type Reader interface {
	Read(p []byte) (n int, err error)
}
```

# 二.代码演示
* 可以使用strings包下的NewReader创建字符串流
```go
	r := strings.NewReader("hello 世界")
	b := make([]byte, r.Size())//创建字节切片,存放流中数据,根据流数据大小创建切片大小
	n, err := r.Read(b)//把流中数据读取到切片中
	if err != nil {
		fmt.Println("读取失败,", err)
		return
	}
	fmt.Println("读取数据长度,", n)

	fmt.Println("流中数据",string(b))//以字符串形式输入切片中数据
```
* 最常用的是文件流,把外部文件中数据读取到程序中
```go
	f, err := os.Open("D:/go.txt")//打开文件
	defer f.Close()
	if err != nil {
		fmt.Println("文件读取失败,", err)
		return
	}
	fileInfo, err := f.Stat()//获取文件信息
	if err != nil { 
		fmt.Println("文件信息获取失败,", err)
		return
	}
	b := make([]byte, fileInfo.Size())//根据文件中数据大小创建切片
	_, err = f.Read(b)//读取数据到切片中
	if err != nil {
		fmt.Println("文件流读取失败:", err)
		return
	}
	fmt.Println("文件中内容为:", string(b))//以字符串形式输入切片中数据
```