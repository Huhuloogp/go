！！

1、双重for循环中continue默认影响最内侧循环,与最外层循环无关

Go语言执行标签写法,可以通过定义标签让continue控制影响哪个for循环

break同理

2、continue是断一个小循环，break断一个大循环

# 一.continue 

* continue关键字控制结束本次 **循环体 **结束,执行表达式三.
```go
	for i := 0; i < 5; i++ {
		fmt.Println("开始")
		if i == 2 || i == 3 {
			continue
		}
		fmt.Println("结束")
	}
```
* 在双重for循环中continue默认影响最内侧循环,与最外层循环无关
```go
func main() {
	for k := 0; k < 2; k++ {
		for i := 0; i < 3; i++ {
			if i == 1 {
				continue
			}
			fmt.Println(k, i, "结束")
		}
	}
```
* Go语言执行标签写法,可以通过定义标签让continue控制影响哪个for循环
```go
	myfor:for k := 0; k < 2; k++ {
		for i := 0; i < 3; i++ {
			if i == 1 {
				continue myfor
			}
			fmt.Println(k, i, "结束")
		}
	}
```
# 二. break
* break可以中断for循环,无论for循环还有几次执行,立即停止
```go
	for i := 0; i < 5; i++ {
		if i == 2 {
			break
		}
		fmt.Println(i)
	}
```
* 在双重for循环中,break默认也影响到最近的for循环
```go
	for i := 0; i < 2; i++ {
		for j := 0; j < 2; j++ {
			if j == 1 {
				break
			}
			fmt.Println(i, j)
		}
	}
```
* break也可以通过定义标签,控制break对哪个for循环生效
```go
	myfor:for i := 0; i < 2; i++ {
		for j := 0; j < 2; j++ {
			if j == 1 {
				break myfor
			}
			fmt.Println(i, j)
		}
	}
```