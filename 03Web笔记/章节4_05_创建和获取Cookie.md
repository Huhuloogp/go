1、

- Cookie就是客户端存储技术.以键值对的形式存在
- 在B/S架构中,服务器端产生Cookie响应给客户端,浏览器接收后把Cookie存在在特定的文件夹中,以后每次请求浏览器会把Cookie内容放入到请求中

2、

服务器产生cookie

```go
func setCookie(w http.ResponseWriter, r *http.Request) {
	//创建一个cookie并传给响应对象
   c := http.Cookie{Name: "mykey", Value: "myvalue"}
   http.SetCookie(w, &c)
   //返回给浏览器解析
   t, _ := template.ParseFiles("view/cookie/cookie.html")
   t.Execute(w, nil)
}
```



客户端得到cookie

```
func getCookie(w http.ResponseWriter, r *http.Request) {
   //根据key取出Cookie
   //c1,_:=r.Cookie("mykey")
   //取出全部Cookie内容
   cs := r.Cookies()
   t, _ := template.ParseFiles("view/cookie/cookie.html")
   t.Execute(w, cs)
}
```







# 一.Cookie 简介

* Cookie就是客户端存储技术.以键值对的形式存在
* 在B/S架构中,服务器端产生Cookie响应给客户端,浏览器接收后把Cookie存在在特定的文件夹中,以后每次请求浏览器会把Cookie内容放入到请求中

# 二.Go语言对Cookie的支持

* 在net/http包下提供了Cookie结构体

  * Name设置Cookie的名称
  * Value 表示Cookie的值
  * Path 有效范围
  * Domain 可访问Cookie 的域
  * Expires 过期时间
  * MaxAge 最大存活时间,单位秒
  * HttpOnly 是否可以通过脚本访问

  ```go
  type Cookie struct {
  	Name  string
  	Value string

  	Path       string    // optional
  	Domain     string    // optional
  	Expires    time.Time // optional
  	RawExpires string    // for reading cookies only

  	// MaxAge=0 means no 'Max-Age' attribute specified.
  	// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
  	// MaxAge>0 means Max-Age attribute present and given in seconds
  	MaxAge   int
  	Secure   bool
  	HttpOnly bool
  	Raw      string
  	Unparsed []string // Raw text of unparsed attribute-value pairs
  }
  ```

  # 三.代码演示

* 默认显示index.html页面,显示该页面时没有Cookie,点击超链接请求服务器后,服务端把Cookie响应给客户端,通过开发者工具(F12)观察整个过程.

  ```go
  <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
          "http://www.w3.org/TR/html4/loose.dtd">
  <html>
  <head>
      <title></title>
  </head>
  <body>
  <a href="setCookie">产生Cookie</a>
  <a href="getCookie">获取Cookie</a>
  <br/>
  {{.}}
  </body>
  </html>
  ```

* 服务器提供创建Cookie和获取Cookie的代码
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
	c := http.Cookie{Name: "mykey", Value: "myvalue"}
	http.SetCookie(w, &c)
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, nil)

}
func getCookie(w http.ResponseWriter, r *http.Request) {
	//根据key取出Cookie
	//c1,_:=r.Cookie("mykey")
	//取出全部Cookie内容
	cs := r.Cookies()
	t, _ := template.ParseFiles("view/index.html")
	t.Execute(w, cs)
}
func main() {
	server := http.Server{Addr: ":8090"}
	http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("static"))))
	http.HandleFunc("/", welcome)
	http.HandleFunc("/setCookie", setCookie)
	http.HandleFunc("/getCookie", getCookie)
	server.ListenAndServe()
}

```