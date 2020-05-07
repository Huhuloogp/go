！！

1、封装主要体现在两个方面:封装数据、封装业务

2、Go语言中通过首字母大小控制访问权限，属性首字母小写然后对外提供访问方法是封装数据最常见的实现方式

3、封装数据的例子和怎样访问被封装的属性

访问被封装的属性主要是通过get方法的返回值func  (p *People)  GetName()  **string**{}

```go
type People struct {
	name string //姓名
	age  int    //体重.单位斤
}	
func (p *People) SetName(name string) {		//p是结构体变量名，是方法的标识
    //为什么是*People呢，因为peo到p People只是把peo的值给P，p修改了peo不改变，
    //但是如果是*People,则peo到p *People是把peo的地址给p，p改变了peo也改变，同时peo也可以			成为方法的标识，从而调用方法体的方法，得到return值（访问被封装的属性）
	p.name = name
}

func (p *People) GetName() string {
	return p.name
}

func (p *People) SetAge(age int) {
	p.age = age
}

func (p *People) GetAge() int {
	return p.age
}



func main(){
    peo:=new(people.People)
    peo.SetName("张三")
    fmt.Println(peo.GetName())		//得到return值（访问被封装的属性）
}
```

4、







# 一. 封装

* 封装主要体现在两个方面:封装数据、封装业务
* Go语言中通过首字母大小控制访问权限.属性首字母小写对外提供访问方法是封装数据最常见的实现方式
* 可以通过方法封装业务
  * 提出方法是封装
  * 控制结构体属性访问,对外提供访问方法也是封装
* 在面向对象中封装的好处:
  * 安全性.结构体属性访问受到限制,必须按照特定访问渠道
  * 可复用性,封装的方法实现可复用性
  * 可读写,多段增加代码可读性

# 二.代码实现
* Go语言同包任意位置可以访问全局内容,封装控制可以控制包外访问结构体中数据
```go
type People struct {
	name string //姓名
	age  int    //体重.单位斤
}

func (p *People) SetName(name string) {
	p.name = name
}
func (p *People) GetName() string {
	return p.name
}

func (p *People) SetAge(age int) {
	p.age = age
}

func (p *People) GetAge() int {
	return p.age
}
```
* 封装业务就是根据自己的需求提取代码,使用Go语言标准库中的函数过程就属性封装业务(代码)