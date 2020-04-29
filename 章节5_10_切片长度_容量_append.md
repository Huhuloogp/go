1、长度表示切片中元素的实际个数,容量表示切片占用空间大小

2、使用make函数定义无内容,但是不是nil的切片,意味着切片已经申请了内存空间

3、append()函数，直接使用不需要导包

4、切片容量问题

如果添加一次添加多个值,且添加后的长度大于扩容一次的大小,容量和长度相等.等到下次添加内容时如果不超出扩容大小,在现在的基础上进行翻倍（不理解看代码）		

	s := make([]string, 0)
	fmt.Println(len(s), cap(s)) //输出:0 0
	s = append(s, "老张", "佳明哥")
	fmt.Println(len(s), cap(s)) //输出:2 2
	s = append(s, "smallming")
	fmt.Println(len(s), cap(s)) //输出:3 4
	s = append(s, "4", "5", "6", "7", "8", "9")
	fmt.Println(len(s), cap(s)) //输出:9 9
	s = append(s,"10")
	fmt.Println(len(s), cap(s)) //输出:10 18
5、把一个切片的内容直接添加到另一个切片中

s = append(s, s1...) //注意此处,必须有三个点









# 一.make函数

* Go语言中可以使用make函数创建slice 、 map、 channel、 interface
* 使用make函数定义无内容,但是不是nil的切片,意味着切片已经申请了内存空间
  *  `make(类型,初始长度[,初始容量])`
  *  初始容量可以省略,默认和长度相等
```go
	slice := make([]string, 0)     //长度为0的切片,没有第三个参数表示容量和长度相等
	slice1 := make([]string, 0, 2) //长度为0,容量为2
	fmt.Println(slice, slice1)
```
* 长度表示切片中元素的实际个数,容量表示切片占用空间大小,且切片容量成倍增加.当增加到1024后按照一定百分比增加.
  * len(slice) 查看切片的长度
  * cap(slice) 查看切片的容量
```go
	slice := make([]string, 0)     //长度为0的切片,没有第三个参数表示容量和长度相等
	slice1 := make([]string, 0, 3) //长度为0,容量为2
	fmt.Println(len(slice), cap(slice))
	fmt.Println(len(slice1), cap(slice1))
```
# 二.append()函数

* append()在Go语言标准库中源码如下
```go
// The append built-in function appends elements to the end of a slice. If
// it has sufficient capacity, the destination is resliced to accommodate the
// new elements. If it does not, a new underlying array will be allocated.
// Append returns the updated slice. It is therefore necessary to store the
// result of append, often in the variable holding the slice itself:
//	slice = append(slice, elem1, elem2)
//	slice = append(slice, anotherSlice...)
// As a special case, it is legal to append a string to a byte slice, like this:
//	slice = append([]byte("hello "), "world"...)
func append(slice []Type, elems ...Type) []Type
```
* 可以向切片中添加一个或多个值,添加后必须使用切片接收append()函数返回值
```go
	s := make([]string, 0)
	fmt.Println(len(s), cap(s))//输出:0 0
	s = append(s, "老张", "佳明哥")
	fmt.Println(len(s), cap(s))//输出:2 2
	s = append(s, "smallming")
	fmt.Println(len(s), cap(s))//输出:3 4
```

* 如果添加一次添加多个值,且添加后的长度大于扩容一次的大小,容量和长度相等.等到下次添加内容时如果不超出扩容大小,在现在的基础上进行翻倍
```go
	s := make([]string, 0)
	fmt.Println(len(s), cap(s)) //输出:0 0
	s = append(s, "老张", "佳明哥")
	fmt.Println(len(s), cap(s)) //输出:2 2
	s = append(s, "smallming")
	fmt.Println(len(s), cap(s)) //输出:3 4
	s = append(s, "4", "5", "6", "7", "8", "9")
	fmt.Println(len(s), cap(s)) //输出:9 9
	s = append(s,"10")
	fmt.Println(len(s), cap(s)) //输出:10 18
```
* 也可以把一个切片的内容直接添加到另一个切片中.需要注意语法中有三个点
```go
	s := make([]string, 0)
	s1 := []string{"smallming", "佳明哥"}
	s = append(s, s1...) //注意此处,必须有三个点
	fmt.Println(s)
```