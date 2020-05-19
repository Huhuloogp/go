1、 rows,err:=stmt.Exec()——》rows,err:=stmt.Query()

2、通过next方法进行循环遍历，for rows.Next(){





# 一.查询注意点

* Golang中执行查询与新增、删除、修改中stmt的执行方法有区别,由于需要把查询到的结果取出来,所以还需要进行取值处理
# 二.代码示例
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
         fmt.Println("关闭连接")
      }
   }()
   /*
      准备处理SQL语句
      支持占位符,防止SQL注入
    */
   stmt,err:=db.Prepare("select * from people")
   //错误处理
   if err!=nil{
      fmt.Println("预处理失败",err)
   }
   //关闭对象
   defer func() {
      if stmt!=nil{
         stmt.Close()
         fmt.Println("stmt关闭")
      }
   }()
   /*
   Exec() 参数为不定项参数,对应占位符?个数
    */
   rows,err:=stmt.Query()
   if err!=nil{
      fmt.Println("查询失败",err)
   }
   //循环遍历结果
   for rows.Next(){
      var id int
      var name,address string
      //把行内值付给变量
      rows.Scan(&id,&name,&address)
      fmt.Println(id,name,address)
   }
   defer func() {
      if rows!=nil{
         rows.Close()
         fmt.Println("关闭结果集")
      }
   }()
}
```