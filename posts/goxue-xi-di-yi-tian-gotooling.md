<!--
.. title: Go学习第一天 - GoTooling
.. slug: goxue-xi-di-yi-tian-gotooling
.. date: 2021-08-20 22:38:06 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

# Go学习第一天, GoTooling

## GOPATH vs GOROOT

> The GOPATH environment variable lists places to look for Go code. On Unix, the value is a colon-separated string, On Windows, the value is a semicolon-separated string, On Plan9, the value is a list
> GOPATH must be set to get, build and install packages outside the standard Go Tree(即GO SDK)

> GOROOT is where the go SDK being installed

GOPATH就是Go安装第三方库的位置, 通过改变这个，我们可以自由切换依赖环境, 实现类似于python中virtualenv的功能

GOROOT就是Go SDK的安装位置


## 创建第一个GO可执行程序

在go1.2以前, 如果要创建一个命名类似于"github.com/foo"的包的话, 需要在"$GOPATH/src/github.com"下创建, 流程如下:
```bash
mkdir -p $GOPATH/src/github.com/foo
```

在编译go项目后, 可以通过go install进行安装, 会将项目安装在"$GOPATH/bin/"下:
```
go install github.com/foo/hello # install the binary
$GOPATH/bin/hello # now you can run it
```

如果将GOPATH加入到了PATH中, 可以直接运行
```
hello
```

## 创建第一个GO module

大致流程和创建GO程序差不多, 但和GO程序的区别在于GO包编译并不会生成可执行文件, 因此我们并不需要使用"go install"命令进行安装, 
在完成后, 使用"go build"测试一下包能够通过编译即可

```bash
mkdir $GOPATH/src/github.com/foo/stringutil
go build github.com/goo/stringutil
```


## 更先进的项目环境管理方式

在go1.2后, 通过go.mod, 我们不再需要在$GOPATH/src/下进行项目开发了, 相应的, 我们可以通过定义go.mod来创建module

首先执行go init生成用于管理的go.mod和go.sum文件

```bash
go mod init github.com/foo 
```

一个典型的go.mod如下, 
```
module github.com/dagrons/godw

go 1.16

require github.com/gin-gonic/gin v1.7.2
```

指定了module名, go SDK版本, 以及依赖的第三方库及版本, 

这种项目依赖管理方式和package.json和Gemfile类似, 更加好用

在go.mod编写完成后, 需要通过"go tidy mod"进行依赖解析, 会下载并解析好所有的依赖关系

```bash
go mod tidy
```









