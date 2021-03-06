1、sort包中最上层是一个名称为Interface的接口,只要满足sort.Interface类型都可以排序（Ints()，Floats64()，Strings()，IntSlice()，Float64Slice()，StringSlice()类型。。）

```
num := [] int{1, 7, 5, 2, 6}
	sort.Ints(num) //升序		转换为满足sort.Interface类型的Ints()数组
	fmt.Println(num)
	
	sort.Sort(sort.Reverse(sort.IntSlice(num))) //降序
	//sort.IntSlice(num)转换为满足sort.Interface类型的IntSlice()切片
	sort.Reverse(sort.IntSlice(num)),告诉系统要降序reverse的是num
	sort.Sort(sort.Reverse(sort.IntSlice(num))) 才是真正的排序
	
	fmt.Println(num)
```

2、float同理

3、String类型排序，是根据首字母，判断优先级英文字母大于中文

4、sort.StringSlice(s，“你是”)，返回“你是”字符串的索引，使用要求，切片要是已经进行升序的切片







# 一.sort包

* Go语言标准库中sort提供了排序API
* sort包提供了多种排序算法,这些算法是内部实现的,每次使用sort包排序时,会自动选择最优算法实现
  * 插入排序
  * 快速排序
  * 堆排
* sort包中最上层是一个名称为Interface的接口,只要满足sort.Interface类型都可以排序
```go
// A type, typically a collection, that satisfies sort.Interface can be
// sorted by the routines in this package. The methods require that the
// elements of the collection be enumerated by an integer index.
type Interface interface {
	// Len is the number of elements in the collection.
	Len() int
	// Less reports whether the element with
	// index i should sort before the element with index j.
	Less(i, j int) bool
	// Swap swaps the elements with indexes i and j.
	Swap(i, j int)
}
```
* Go语言标准库默认提供了对int、float64、string进行排序的API
* 很多函数的参数都是sort包下类型,需要进行转换.

# 二.排序实现
* 对int类型切片排序
```go
	num := [] int{1, 7, 5, 2, 6}
	sort.Ints(num) //升序
	fmt.Println(num)
	sort.Sort(sort.Reverse(sort.IntSlice(num))) //降序
	fmt.Println(num)
```
* 对float64类型切片排序
```go
	f := [] float64{1.5, 7.2, 5.8, 2.3, 6.9}
	sort.Float64s(f) //升序
	fmt.Println(f)
	sort.Sort(sort.Reverse(sort.Float64Slice(f))) //降序
	fmt.Println(f)
```
* 对string类型切片排序
  * 按照编码表数值进行排序
  * 多字符串中按照第一个字符进行比较
  * 如果第一个字符相同,比较第二个字符
```go
	s := []string{"我", "我是中国人", "a", "d", "国家", "你", "我a"}
	sort.Sort(sort.StringSlice(s)) //升序
	fmt.Println(s)
	//查找内容的索引,如果不存在,返回内容应该在升序排序切片的哪个位置插入
	fmt.Println(sort.SearchStrings(s, "你是"))
	sort.Sort(sort.Reverse(sort.StringSlice(s)))
	fmt.Println(s)
```

