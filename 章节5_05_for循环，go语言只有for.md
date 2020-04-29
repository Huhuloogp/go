!！！

1、for循环遍历数组等时常与range结合使用

	arr := [3]string{"smallming", "张", "佳明哥"}
	for i, n := range arr {				//i可以换为_
		//其中n=arr[i]
		fmt.Println(i, n)
	}




# 一.for循环

* 循环:让程序多次执行相同的代码块
* for循环是Go语言中唯一一个循环结构
* for循环经典语法
  * 先执行表达式1
  * 执行表达式判断是否成立,如果成立执行循环体
  * 循环体执行完成后,执行表达式3
  * 再次执行表达式2,判断是否成立.
```
for 表达式1;表达式2;表达式3{
  //循环体
}
```
* for循环用的最多的地方就是遍历数组或切片等
# 二.代码示例
* 经典for循环结构中 , for关键字后面有三个表达式,且每个表达式都可以省略
```go
	for i := 0; i < 5; i++ {
		fmt.Println(i)
	}
	//等价于
	j := 0
	for ; j < 5; {
		fmt.Println(j)
		j++
	}
```
* for关键字后面也可以只有一个表达式,表示如果条件成立执行循环体代码
```go
	for i := 0; i < 5; i++ {
		fmt.Println(i)
	}
	//等价于
	j := 0
	for j < 5 {
		fmt.Println(j)
		j++
	}
```
# 三.使用for循环遍历数组
* 可以通过数组的长度判断循环结束条件遍历整个数组
```go
	arr := [3]string{"smallming", "张", "佳明哥"}
	for i := 0; i < len(arr); i++ {
		fmt.Println(arr[i])
	}
```
* for循环遍历数组等时常与range结合使用
  * range函数返回两个值,第一个是脚标,第二个是内容
```go
	arr := [3]string{"smallming", "张", "佳明哥"}
	for i, n := range arr {
		//其中n=arr[i]
		fmt.Println(i, n)
	}
```
