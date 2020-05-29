# 一. 编写Go web项目

* 使用之前的模版显示index.html即可

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
func main() {
	s := http.Server{Addr: ":8090"}
	http.HandleFunc("/", welcome)
	s.ListenAndServe()
}
```



# 二.制作Docker镜像

* 修改系统环境变量GOPATH为项目的路径
* 在项目路径下新建文件dockfile(没有扩展名)并添加下面信息

```
#源镜像
FROM golang:latest
#作者
MAINTAINER smallming
#设置工作目录
WORKDIR $GOPATH/src/c
#将服务器的go工程代码加入到docker容器中
ADD . $GOPATH/src/c
#go构建可执行文件
RUN go build .
#暴露端口
EXPOSE 8091
#最终运行docker的命令
ENTRYPOINT  ["./c"]
```

* 安装docker后,在任务栏图标右键-->switch to windos container 切换到容器
* 启动windows命令行,进入到项目所在目录,输入命令进行构建,第一次构建需要下载golang语言库,时间较长,如果出现下载失败,多次尝试即可.下载后以后就不需要下载了
  * -t表示自定义个tag,给项目起名
  * c 名称
  * . 表示当前目录

```
docker build -t c .
```

* 查看docker中的images,发现有一个c的images和golang的images

```
D:\gowork\c>docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
c                   latest              c7eaa8da1792        38 minutes ago      11.2 GB
golang              latest              3d0b1554763a        5 days ago          11.2 GB
```

* 运行docker
  * 80:80 表示把宿主80绑定给docker容器的80
  * -d 表示后端运行

```
docker run -p 80:80 -d c
```

* 查看docker 容器的id,并根据id查看容器ip

```
D:\gowork\c>docker ps
CONTAINER ID    IMAGE    COMMAND   CREATED        STATUS         PORTS       NAMES
31cd94e28249    c        "./c"     9 minutes ago  Up 9 minutes   0.0.0.0:80->80/tcp, 8091/tcp   hungry_shirley

D:\gowork\c>docker inspect 31cd94e28249
在信息中看"IPAddress"属性就是docker容器的ip
```

* 在浏览器地址栏输入:http://ip:8090就可以访问