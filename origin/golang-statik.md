# statik

# 一、简介

对于Web项目，经常需要将少量的静态资源文件（HTML、JavaScript、CSS）编译到项目二进制文件，而go build是无法直接将静态资源文件编译成二进制文件的。所以此时需要第三方工具

GitHub：https://github.com/rakyll/statik

# 二、使用

## 1、安装

```bash
go get github.com/rakyll/statik
go install github.com/rakyll/statik
```

## 2、编译指定目录下的静态资源文件为二进制

```bash
statik -src=/path/to/your/project/public
```

会在项目根目录下生成`statik/static.go`

```go
// Code generated by statik. DO NOT EDIT.
package statik

import (
	"github.com/rakyll/statik/fs"
)

func init() {
	data := "PK\\x03\x04\x14......省略"
	fs.Register(data)
}
```

## 3、main中引用

```go
package main
import (
	_ "github.com/项目名/statik"
	"github.com/rakyll/statik/fs"
)

func main (){
  //statikFS为http.FileSystem类型
	statikFS, err := fs.New()
  if err != nil {
    log.Fatal(err)
  }
  http.Handle("/public/", http.StripPrefix("/public/", http.FileServer(statikFS)))
  // 或者在gin框架下router.StaticFS("/public", statikFS)
}
```

