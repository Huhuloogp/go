# 一. 修改注意点

* 修改和新增类似,只是不需要获取LastInsertId()和SQL语句是修改语句
* 在Golang中要求:如果修改前和修改后的值相同,RowsAffected()返回0
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
   stmt,err:=db.Prepare("update people set name=?,address=? where id=?")
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
   res,err:=stmt.Exec("李四","朝阳",1)
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
      fmt.Println("修改成功")
   }else{
      fmt.Println("修改失败")
   }
}
```

