1、看标题

2、JSON轻量级数据传输格式，不是一种技术，不是一种语言，而是一个格式？

3、在go语言中如何进行结构体和json字符串转换，以及如何把一个byte切片转换为一个结构体对象，同时演示了ajax请求json数据时全部的代码

例子请看json.go

# 一.JSON简介

* 轻量级数据传输**格式**

* 总体上分为两种:

  * 一种是JSONObject(json对象)

  ```
  {"key":value,"key":value}
  ```

  * 一种是JSONArrayP(json数组),包含多个JSONObject

  ```
  [{"key":"value"},{"key":"value"}]
  ```

* key是string类型,value可以是string类型(值被双引号包含),也可以是数值或布尔类型等,也可以是JSONObject类型或JSONArray类型

* 可以使用Go语言标准库中 encoding/json 包下的Marshal()或Unmarshal()把结构体对象转换成[]byte或把[]byte中信息写入到结构体对象中

  * 在转换过程中结构体属性tag中定义了json中的key,属性的值就是json中的value
  * 如果属性没有配置tag,属性就是json中的key

* 属性的tag可以进行下面配置

```go
// 字段被本包忽略
Field int `json:"-"`
// 字段在json里的键为"myName"
把该属性转化为json对象时key是什么
Field int `json:"myName"`
// 字段在json里的键为"myName"且如果字段为空值将在对象中省略掉
Field int `json:"myName,omitempty"`
// 字段在json里的键为"Field"（默认值），但如果字段为空值会跳过；注意前导的逗号
Field int `json:",omitempty"`
```

 

# 二.代码示例

* 结构体和[]byte进行转换代码比较简单
  * 只要满足键值对形式的类型都可以转换成标准的json格式

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name string
	Age  int
}

func main() {
	user := User{"张三", 12}
	//user:=map[string]interface{}{"Name":"张三","Age":18}
	//把结构体转换为[]byte
	b, _ := json.Marshal(user)
	fmt.Println(string(b))
	//把[]byte转为json
	u2 := new(User)
	json.Unmarshal(b, u2)
	fmt.Println(u2)
}

```



# 三.Ajax访问返回json数据

* 使用jQuery封装的$.post()进行ajax请求
* HTML页面发送ajax请求,请求数据

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="/static/js/jquery-1.7.2.js"></script>
    <script type="text/javascript">
        $(function () {
            $("button").click(function () {
                $.post("getUser", function (data) {
                    var result = "";
                    for (var i = 0; i < data.length; i++) {
                        result += "<tr>";
                        result += "<td>";
                        result += data[i].Name;
                        result += "</td>";
                        result += "<td>";
                        result += data[i].Age;
                        result += "</td>";
                        result += "</tr>";
                    }
                    $("#t_tbody").html(result)
                })
            })
        })
    </script>
</head>
<body>
<button>加载数据到表格</button>
<table border="1">
    <tr>
        <th>姓名</th>
        <th>年龄</th>
    </tr>
    <tbody id="t_tbody">

    </tbody>
</table>
</body>
</html>

```

* 服务端返回json数据即可.

```go
package main

import (
	"net/http"
	"html/template"
	"encoding/json"
	"fmt"
)

type User struct {
	Name string
	Age  int
}

func welcome(w http.ResponseWriter, r *http.Request) {
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)
}
func getUser(w http.ResponseWriter, r *http.Request) {
	users := make([]User, 0)
	users = append(users, User{"张三", 12})
	users = append(users, User{"李四", 13})
	users = append(users, User{"王五", 14})
	w.Header().Set("Content-type", "application/json;charset=utf-8")
	b, _ := json.Marshal(users)
	fmt.Fprintln(w, string(b))
}

func main() {
	server := http.Server{Addr: ":8090"}
	http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("static"))))
	http.HandleFunc("/", welcome)        //首页
	http.HandleFunc("/getUser", getUser) //获取信息Handler
	server.ListenAndServe()
}

```

