# 一.copy函数

* 通过copy函数可以把一个切片内容复制到另一个切片中
* Go语言标准库源码定义如下
  * 第一个参数是目标切片,接收第二个参数内容
  * 第二个参数是源切片,把内容拷贝到第一个参数中
```go
// The copy built-in function copies elements from a source slice into a
// destination slice. (As a special case, it also will copy bytes from a
// string to a slice of bytes.) The source and destination may overlap. Copy
// returns the number of elements copied, which will be the minimum of
// len(src) and len(dst).
func copy(dst, src []Type) int
```
* 拷贝时严格按照脚标进行拷贝.且不会对目标切片进行扩容

# 二.代码示例
* 把短切片拷贝到长切片中
```go
	s1:=[]int {1,2}
	s2:=[]int {3,4,5,6}
	copy(s2,s1)
	fmt.Println(s1)//输出:[1 2]
	fmt.Println(s2)//输出:[1 2 5 6]
```
* 把长切片拷贝到短切片中
```go
	s1:=[]int {1,2}
	s2:=[]int {3,4,5,6}
	copy(s1,s2)
	fmt.Println(s1)//输出:[3 4]
	fmt.Println(s2)//输出:[3 4 5 6]
```
* 把切片片段拷贝到切片中
```go
	s1:=[]int {1,2}
	s2:=[]int {3,4,5,6}
	copy(s1,s2[1:])
	fmt.Println(s1)//输出:[4 5]
	fmt.Println(s2)//输出:[3 4 5 6]
```

# 三.使用copy完成删除元素
* 使用copy函数可以保证原切片内容不变
```go
	s := []int{1, 2, 3, 4, 5, 6, 7}
	n := 2 //要删除元素的索引
	newSlice := make([]int, n)
	copy(newSlice, s[0:n])
	newSlice = append(newSlice, s[n+1:]...)
	fmt.Println(s)        //原切片不变
	fmt.Println(newSlice) //删除指定元素后的切片
```
