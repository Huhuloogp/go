1、 

```
	mylist := list.New()
	fmt.Println(mylist)       //输出list中内容
	//输出&{{0xc00006e330 0xc00006e330 <nil> <nil>} 0}
	//{0xc00006e330 0xc00006e330 <nil> <nil>}是root elements,0是长度为0
	<nil> <nil>前指针为nil，后指针为nil因为只有一个链表
	fmt.Println(mylist.Len()) //查看链表中元素的个数
	fmt.Printf("%p", mylist)  //输出地址
```

2、

```
	mylist.PushBack("a")	//添加到最后,List["a"]
	mylist.PushFront("b") 	//添加到最前面,List["b","a"]
	mylist.InsertAfter("c", mylist.Front()) 
	//向第一个元素后面添加元素,List["b","c","a"]
	mylist.InsertBefore("d", mylist.Back()) 
	//向最后一个元素前面添加元素,List["b","c","d","a"]
	
```

3、取出链表中的元素，只能从头向后找,或从后往前找,获取元素内容	

```
for i:=0;i<n;i++{
    mylist.Front().Next()
}

要考虑链表n的情况
	n := 5
	var curr *list.Element			//声明一个链表指针
	if n > 0 && n <= mylist.Len() {
		if n == 1 {					//防止链表只有一个元素
			curr = mylist.Front()
		} else if n == mylist.Len() {	//防止链表是最后一个元素
			curr = mylist.Back()
		} else {
			curr = mylist.Front()
			for i := 1; i < n; i++ {
				curr = curr.Next()
			}
		}
```









# 一.操作List

* 直接使用container/list包下的New()新建一个空的List
```go
	mylist := list.New()
	fmt.Println(mylist)       //输出list中内容
	fmt.Println(mylist.Len()) //查看链表中元素的个数
	fmt.Printf("%p", mylist)  //输出地址
```
* Go语言标准库中提供了很多向双向链表中添加元素的函数
```go
	//添加到最后,List["a"]
	mylist.PushBack("a")
    //添加到最前面,List["b","a"]
	mylist.PushFront("b") 
	//向第一个元素后面添加元素,List["b","c","a"]
	mylist.InsertAfter("c", mylist.Front()) 
	//向最后一个元素前面添加元素,List["b","c","d","a"]
	mylist.InsertBefore("d", mylist.Back()) 
```
* 取出链表中的元素
```go
	fmt.Println(mylist.Back().Value)  //最后一个元素的值
	fmt.Println(mylist.Front().Value) //第一个元素的值

	//只能从头向后找,或从后往前找,获取元素内容
	n := 5
	var curr *list.Element
	if n > 0 && n <= mylist.Len() {
		if n == 1 {
			curr = mylist.Front()
		} else if n == mylist.Len() {
			curr = mylist.Back()
		} else {
			curr = mylist.Front()
			for i := 1; i < n; i++ {
				curr = curr.Next()
			}
		}
	} else {
		fmt.Println("n的数值不对")
	}
	//遍历所有值
	for e := mylist.Front(); e != nil; e = e.Next() {
		fmt.Println(e.Value)
	}
```
* 移动元素的顺序
```go
	mylist.MoveToBack(mylist.Front()) //把第一个移动到后面
	mylist.MoveToFront(mylist.Back()) //把最后一个移动到前面
	mylist.MoveAfter(mylist.Front(),mylist.Back())//把第一个参数元素,移动到最后一个参数元素后面
	mylist.MoveBefore(mylist.Front(),mylist.Back())//把第一个参数元素,移动到最后一个参数元素前面
```
* 删除元素
```go
mylist.Remove(mylist.Front())
```