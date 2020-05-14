1、、

func NopCloser(r io.Reader) io.ReadCloser
func ReadAll(r io.Reader) ([]byte, error)		//从流中读取数据，并直接返回一个字节切片
func ReadFile(filename string) ([]byte, error)	//直接把文件中的数据转换为切片
func WriteFile(filename string, data []byte, perm os.FileMode) error	//直接向文件中写内容
func ReadDir(dirname string) ([]os.FileInfo, error)	//直接把某个目录下所有文件返回info
func TempDir(dir, prefix string) (name string, err error)
func TempFile(dir, prefix string) (f *os.File, err error)



2、func WriteFile(filename string, data []byte, perm os.FileMode)

writefile方便是方便，这个有局限性，写入的数据会覆盖之前的数据

3、

# 一. ioutil包

* ioutil包下提供了对文件读写的工具函数,通过这些函数快速实现文件的读写操作
* ioutil包下提供的函数比较少,但是都是很方便使用的函数
```
func NopCloser(r io.Reader) io.ReadCloser
func ReadAll(r io.Reader) ([]byte, error)
func ReadFile(filename string) ([]byte, error)
func WriteFile(filename string, data []byte, perm os.FileMode) error
func ReadDir(dirname string) ([]os.FileInfo, error)
func TempDir(dir, prefix string) (name string, err error)
func TempFile(dir, prefix string) (f *os.File, err error)
```
# 二.代码演示
* 打开完文件后可以使用ReadAll把文件中所有内容都读取到
```go
	f, err := os.Open("D:/go.txt")
	defer f.Close()
	if err != nil {
		fmt.Println(err)
		return
	}
	b, err := ioutil.ReadAll(f)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("文件中内容:\n", string(b))
```
* 也可以直接读取文件中内容
```go
	b, err := ioutil.ReadFile("D:/go.txt")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(string(b))
```
* 写文件也很简单,直接使用WriteFile函数即可,但是源码中已经规定此文件只能是可写状态,且不是尾加数据
```go
	err := ioutil.WriteFile("D:/abc.txt", []byte("内容123123"), 0666)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("数据写入成功")
```
* 还提供了快速获取某个文件夹中所有文件信息的函数
```go
	fs,_:=ioutil.ReadDir("D:/")
	for _,n := range fs {
		fmt.Println(n.Name())
	}
```