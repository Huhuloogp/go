！！

1、goto能在不同函数中使用吗？？或直接跳到另外一个函数？

2、goto不在循环体内使用可以？

3、goto也可以中断一个循环，但是一般都用break，日常不建议使用goto，有可能降低效率





# 一.goto

* goto是Go语言中的一个关键字
* goto让编译器执行时跳转到特定位置
  * Loop是标记名(Label),名称任意,习惯上名称为Loop
```go
	fmt.Println("执行程序")
	i := 6
	if i == 6 {
		goto Loop
	}
	fmt.Println("if下面输出")
Loop:
	fmt.Println("loop")
```
* 可以有多个,但是标签(Labal)定义了就必须使用
```go
	fmt.Println("执行程序")
	i := 6
	if i == 6 {
		goto Loop
	}
	fmt.Println("if下面输出")
Loop:
	fmt.Println("loop")
Loop1: //报错:label Loop1 defined and not used
	fmt.Println("Loop1")
```
* goto也可以用于跳出循环,执行指定标签位置代码
```go
	for i := 0; i < 5; i++ {
		fmt.Println(i)
		if i == 2 {
			goto abc
		}
	}
	fmt.Println("for循环执行结束")
abc:
	fmt.Println("abc")
	fmt.Println("程序结束")
```