1、HttpOnly

默认HttpOnly为false,表示客户端可以通过js获取

表示把cookie存到客户端，可以通过js脚本获取

```
c := http.Cookie{Name: "mykey", Value: "myvalue", HttpOnly: false}
http.SetCookie(w, &c)
```

2、Path属性设置Cookie的访问范围

3、

- Cookie默认存活时间是浏览器不关闭,当浏览器关闭后,Cookie失效
- 可以通过Expires设置具体什么时候过期,Cookie失效. 也可以通过MaxAge设置Cookie多长时间后实现
- IE6,7,8和很多浏览器不支持MaxAge,建议使用Expires
- Expires是time.Time类型,所以设置时需要明确设置过期时间

```go
//MaxAge=0 means 关闭浏览器就失效了
// MaxAge<0 means delete cookie now, 立刻删除
// MaxAge>0 means Max-Age 设定特定时间
```



# 一.HttpOnly

* HttpOnly:控制Cookie的内容是否可以被JavaScript访问到。通过设置HttpOnly为true时防止XSS攻击防御手段之一
* 默认HttpOnly为false,表示客户端可以通过js获取
* 在项目中导入jquery.cookie.js库,使用jquery获取客户端Cookie内容
* HTML代码如下

```go
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="/static/js/jquery-1.7.2.js"></script>
    <script src="/static/js/jquery.cookie.js"></script>
    <script type="text/javascript">
        $(function () {
            $("button").click(function () {
                var value = $.cookie("mykey")
                alert(value)
            })
        })
    </script>
</head>
<body>
<a href="setCookie">产生Cookie</a>
<button>获取cookie</button>
</body>
</html>
```

* 服务端代码如下

```go
package main

import (
	"net/http"
	"html/template"
)

func welcome(w http.ResponseWriter, r *http.Request) {
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)
}
func setCookie(w http.ResponseWriter, r *http.Request) {
	c := http.Cookie{Name: "mykey", Value: "myvalue", HttpOnly: false}
	http.SetCookie(w, &c)
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)

}

func main() {
	server := http.Server{Addr: ":8090"}
	http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("static"))))
	http.HandleFunc("/", welcome)
	http.HandleFunc("/setCookie", setCookie)
	server.ListenAndServe()
}

```

# 二. Path

* Path属性设置Cookie的访问范围
* 默认为”/”表示当前项目下所有都可以访问
* Path设置路径及子路径内容都可以访问
* 首先先访问index.html,点击超链接产生cookie,在浏览器地址栏输入localhost:8090/abc/mypath后发现可以访问cookie
* html代码没有变化,只需要修改服务器端代码如下

```go
package main

import (
	"net/http"
	"html/template"
	"fmt"
)

func welcome(w http.ResponseWriter, r *http.Request) {
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)
}
func setCookie(w http.ResponseWriter, r *http.Request) {
	//验证httponly
	//c := http.Cookie{Name: "mykey", Value: "myvalue", HttpOnly: false}
	//验证path
	c := http.Cookie{Name: "mykey", Value: "myvalue", Path: "/abc/"}
	http.SetCookie(w, &c)
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)
}

//验证path属性是否生效的handler
func mypath(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, r.Cookies())
}

func main() {
	server := http.Server{Addr: ":8090"}
	http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("static"))))
	http.HandleFunc("/", welcome)
	http.HandleFunc("/setCookie", setCookie)
	//路径必须以/abc/开头
	http.HandleFunc("/abc/mypath", mypath)
	server.ListenAndServe()
}

```

# 三.Expires

* Cookie默认存活时间是浏览器不关闭,当浏览器关闭后,Cookie失效
* 可以通过Expires设置具体什么时候过期,Cookie失效. 也可以通过MaxAge设置Cookie多长时间后实现
* IE6,7,8和很多浏览器不支持MaxAge,建议使用Expires
* Expires是time.Time类型,所以设置时需要明确设置过期时间
* 修改服务器端代码如下.只需要修改创建Cookie的代码,其他位置不变

```go
package main

import (
	"net/http"
	"html/template"
	"fmt"
	"time"
)

func welcome(w http.ResponseWriter, r *http.Request) {
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)
}
func setCookie(w http.ResponseWriter, r *http.Request) {
	//验证httponly
	//c := http.Cookie{Name: "mykey", Value: "myvalue", HttpOnly: false}
	//验证path
	//c := http.Cookie{Name: "mykey", Value: "myvalue", Path: "/abc/"}
	//验证Expires
	c := http.Cookie{Name: "mykey", Value: "myvalue", Expires: time.Date(2018, 1, 1, 1, 1, 1, 0, time.Local)}
	http.SetCookie(w, &c)
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)
}

//验证path属性是否生效的handler
func mypath(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, r.Cookies())
}

func main() {
	server := http.Server{Addr: ":8090"}
	http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("static"))))
	http.HandleFunc("/", welcome)
	http.HandleFunc("/setCookie", setCookie)
	//路径必须以/abc/开头
	http.HandleFunc("/abc/mypath", mypath)
	server.ListenAndServe()
}
```

