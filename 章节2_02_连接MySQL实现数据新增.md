1、go语言连接数据库和java不一样，不需要jar包，也不用jdk，但是本地要先下载mysql，启动并创建表，golang连接数据库用户的驱动是：[go-sql-driver](https://github.com/go-sql-driver/mysql)

- 安装方式：打开命令行，执行go命令：

```
go get -u github.com/go-sql-driver/mysql
```

2、

- 构建连接, 格式是：”用户名:密码@tcp(IP:端口)/数据库?charset=utf8”
- 打开数据库,前者是驱动名，所以要导入： _ “github.com/go-sql-driver/mysql”



3、总结

本地要先下载mysql，启动并创建表

sql.Open("mysql","root:root@tcp(localhost:3306)/first")    // 



# 一.数据库准备

* 在MySQL中新建数据库名称first
* 新建表
```sql
create table people(
  id int primary key auto_increment,
  name varchar(20),
  address varchar(20)
)
```


# 二.代码实现

* 全部代码
```go
package main

import (
   "fmt"
   //驱动已经放入到标准库文件夹,由于不使用所以需要空导入,在前面添加_
   _ "github.com/go-sql-driver/mysql"
   //Golang中数据库操作包
   "database/sql"
)

func main() {
   /*
   第一个参数:连接什么数据库
   第二个参数:连接字符串
   语法:数据库登录用户名:密码@tcp(mysql主机ip:端口)/database名称
    */
   db,err:=sql.Open("mysql","root:root@tcp(localhost:3306)/first")
   db.Ping()
   //Error处理
   if err!=nil{
      fmt.Println("数据库连接失败")
   }
   //关闭连接
   defer func() {
      if db!=nil{
         db.Close()
      }
      fmt.Println("关闭连接")
   }()
   /*
      准备处理SQL语句
      支持占位符,防止SQL注入
    */
   stmt,err:=db.Prepare("insert into people values(default,?,?)")
   //错误处理
   if err!=nil{
      fmt.Println("预处理失败",err)
   }
   //关闭对象
   defer func() {
      if stmt!=nil{
         stmt.Close()
      }
   }()
   /*
   Exec() 参数为不定项参数,对应占位符?个数
    */
   res,err:=stmt.Exec("张三","海淀")
   //错误处理
   if err!=nil{
      fmt.Println("执行SQL出现错误")
   }
   //获取生成主键
   id,err:=res.LastInsertId()
   if err!=nil{
      fmt.Println("获取主键失败",err)
   }
   //受影响行数
   count,err:=res.RowsAffected()
   if err!=nil{
      fmt.Println("获取结果失败",err)
   }
   fmt.Println(id,count)
   if count>0{
      fmt.Println("新增成功")
   }else{
      fmt.Println("新增失败")
   }
}
```