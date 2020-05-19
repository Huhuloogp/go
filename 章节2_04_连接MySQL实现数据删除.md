1、在Go语言中要求如果要删除的数据不存在RowsAffected()返回0



# 一.删除注意点

* 删除和修改、新增结构一样,区别为SQL语句
* 在Go语言中要求如果要删除的数据不存在RowsAffected()返回0
# 二.代码实现
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
   stmt,err:=db.Prepare("delete from people where id=?")
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
   res,err:=stmt.Exec(1)
   //错误处理
   if err!=nil{
      fmt.Println("执行SQL出现错误")
   }
   //受影响行数
   count,err:=res.RowsAffected()
   if err!=nil{
      fmt.Println("获取结果失败",err)
   }
   if count>0{
      fmt.Println("删除成功")
   }else{
      fmt.Println("删除失败")
   }
}
```