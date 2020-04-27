！！！

1、default放哪里都行，但是无论放哪里，都是在所有case执行完再执行

2、当条件是范围（switch后面不加东西），当条件是固定值，switch后面加固定值变量名

3、穿透和中断







# 一.switch结构介绍

* switch也属于条件判断的语句
* 支持多种写法,和if .. else if ...else 结构的功能类似,但是里面的细节需要注意的地方更多
* switch基本语法
```
switch [定义变量;] [变量]{
  case [条件/具体值]:
  	//代码
  case [条件/具体值]:
  	//代码
  default:
    //代码
}
```
* switch每个case分支默认只执行一个且是从上向下执行
* default上下位置没有影响,当且仅当所有case都不成立时才执行default

# 二.switch用法(一)
* 当变量只有固定的几个值时可以使用switch结构
```go
func main() {
	num := 16
	switch num {
	case 2:
		fmt.Println("2进制")
	case 8:
		fmt.Println("8进制")
	case 10:
		fmt.Println("10进制")
	case 16:
		fmt.Println("16进制")
	default:
		fmt.Println("内容不正确")
	}
	fmt.Println("程序结束")
}
```
* switch也支持在条件位置定义变量,变量有效范围为当前switch
```go
func main() {
	switch num := 16; num {
	case 2:
		fmt.Println("2进制")
	case 8:
		fmt.Println("8进制")
	case 10:
		fmt.Println("10进制")
	case 16:
		fmt.Println("16进制")
	default:
		fmt.Println("内容不正确")
	}
	fmt.Println("程序结束")
}
```
# 三.switch用法(二)
* 当条件是范围而不是固定值时
```go
func main() {
	score := 71
	switch {
	case score >= 90:
		fmt.Println("优秀")
	case score >= 80:
		fmt.Println("良好")
	case score >= 70:
		fmt.Println("中等")
	case score >= 60:
		fmt.Println("及格")
	default:
		fmt.Println("不及格")
	}
	fmt.Println("程序结束")
}
```
# 四.switch用法(三)
* case条件支持多个值,每个值使用逗号分开
```go
func main() {
	month := 5
	switch month {
	case 1, 3, 5, 7, 8, 10, 12:
		fmt.Println("31天")
	case 2:
		fmt.Println("28或29天")
	default:
		fmt.Println("30天")
	}
	fmt.Println("程序结束")
}
```
# 五. 穿透和中断
* switch结构中某个最多只能执行一个case,使用fallthrough可以让下一个case/default继续执行
```go
func main() {
	switch num := 1; num {
	case 1:
		fmt.Println("1")
		fallthrough
	case 2:
		fmt.Println("2")
	case 3:
		fmt.Println("3")
		fallthrough
	case 4:
		fmt.Println("4")
	default:
		fmt.Println("不是1,2,3,4")
	}
	fmt.Println("程序结束")
}
```
* break可以用在switch和循环中,表示立即结束,无论当前结构后面还有多少代码
```go
func main() {
	switch num := 1; num {
	case 1:
		fmt.Println("1")
		break
		fmt.Println("break后面代码都不执行")
		fallthrough
	case 2:
		fmt.Println("2")
	case 3:
		fmt.Println("3")
		fallthrough
	case 4:
		fmt.Println("4")
	default:
		fmt.Println("不是1,2,3,4")
	}
	fmt.Println("程序结束")
}
```