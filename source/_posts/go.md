---
title: go入门
date: 2020-06-29 11:20:05
categories: go
tags: go
---

## 前提
系统环境：win10 64

## 简介
Go（又称 Golang）是 Google 的 Robert Griesemer，Rob Pike 及 Ken Thompson 开发的一种静态强类型、编译型语言。Go 语言语法与 C 相近，但功能上有：内存安全，GC（垃圾回收），结构形态及 CSP-style 并发计算。

## 特点
* 语法简单
* 并发模型
* 内存分配
* 垃圾回收
* 静态链接
* 标准库
* 工具链

## 可以用来做什么
* 服务器编程
* 分布式系统，数据库代理器等
* 网络编程，这一块目前应用最广，包括Web应用、API应用、下载应用
* 内存数据库
* 云平台

## 安装
* 方法一：使用choco安装go
```sh
choco install golang
```
* 方法二：去官网下载对应的安装包，官网地址：https://golang.google.cn/dl/

## 常用命令
> go <命令> [参数]

命令 | 描述
--- | ---
bug | 创建一个bug报告,执行完命令后，会用浏览器访问github.com/golang/go 的issue。自动填写一些内容，引导你如何提交一个bug报告
build | 编译包以及其依赖,最常用的命令之一。默认情况下，会在命令所在目录生成一个当前操作系统对应的可执行文件。安装完整版的Go环境，可以交叉编译其他操作系统的二进制可执行文件
clean | 清空对象文件和缓存文件,前面提到的build命令和下面的test命令会生成一些文件和目录，clean会清理掉这些文件，包括build命令生成可执行文件
doc | 打印包中的文档和标记符,打印出包或指定文件的说明文档，加上-all 参数，可以看到包里的所有函数列表和文档。创建一个go文件，写入一下代码
env | 打印出你现在的Go环境信息,查看各个go的开发环境参数，忘记GOPATH和GOROOT路径就可以用这个打印出来了
fix | 用go的新版本的API更新
fmt | 自动格式化代码文件
generate | 可以执行指令，包括生成和更新go源码文件的指令
get | 下载和安装go包以及其依赖包的命令,eg:go get <包的路径>
install | 编译和安装包及其依赖包,可执行文件会被安装在$GOPATH/bin目录下。
list | 列出目录下的所有包和模块，每行一个。
run | 运行go项目,非常常用。它会编译包，然后直接运行起来，不会在当前目录生成二进制文件。
test | 运行调试,用于运行_text.go文件中的Test开头并且参数为 *testing.T的函数
mod | 管理模块（包和依赖项）
tool | 运行指定的go工具
version | 查看当前go版本
vet | 查看包中可能出现的错误,例如，给整型%d占位符提供一个字符串参数，就会检查出类型错误，但是这个代码编译是不会报错的。

## 简单示例
1. 创建开发目录，eg: d:/go
2. 新建test.go文件，文件内容如下：
```go
package main
import "fmt"

func main() {
   fmt.Println("Hello, World!")
}
```
3. 在命令行执行：go run test.go 查看效果

## 解决go get 无反应、下载慢等问题
见参考：Goproxy模块代理解决国内下载模块慢

## 修改GOPATH路径
1、自己选个盘符新建目录
```js
   eg:
     目录名：godev
     根目录建三个文件夹：bin/pkg/src
```
2、修改环境变量GOPATH目录为新目录地址

##  go mod详解
> 官方包1.11及以上版本将会自动支持 “go mod”，默认GO111MODULE=auto(auto是指如果在gopath下不启用mod)

> Golan 提供一个环境变量 GO111MODULE 来设置是否使用mod，它有3个可选值，分别是off, on, auto（默认值），具体含义如下：

 默认值 | 描述
 --- | ---
 off | GOPATH mode，查找vendor和GOPATH目录
 on  | module-aware mode，使用 go module，忽略GOPATH目录
 auto | 如果当前目录不在$GOPATH 并且 当前目录（或者父目录）下有go.mod文件，则使用 GO111MODULE， 否则仍旧使用 GOPATH mode

* 设置 GO111MODULE 命令
```sh
go env -w GO111MODULE=on
```
> 在使用模块的时候， GOPATH 是无意义的，不过它还是会把下载的依赖储存在 GOPATH/src/mod 中，也会把 go install 的结果放在 GOPATH/bin（如果 GOBIN 不存在的话）

命令 | 描述
--- | ---
go mod download | 下载模块到本地缓存，缓存路径是 $GOPATH/pkg/mod/cache
go mod edit | 是提供了命令版编辑 go.mod 的功能，例如 go mod edit -fmt go.mod 会格式化 go.mod
go mod graph | 把模块之间的依赖图显示出来
go mod init | 初始化模块（例如把原本dep管理的依赖关系转换过来）
go mod tidy | 增加缺失的包，移除没用的包
go mod vendor | 把依赖拷贝到 vendor/ 目录下
go mod verify | 确认依赖关系
go mod why | 解释为什么需要包和模块


**注意：**
1、go mod 命令在 $GOPATH 里默认是执行不了的，因为 GO111MODULE 的默认值是 auto。默认在$GOPATH 里是不会执行， 如果一定要强制执行，就设置环境变量为 on
2、go mod init 在没有接module名字的时候是执行不了的，会报错 go: cannot determine module path for source directory

-------

## 自定义目录创建go项目
1、比如GO安装目录在C盘，GOPATH目录在D盘，新建的项目工程目录在E盘，eg：godev
2、进入godev目录执行以下命令：
```sh
cd godev
go mod init 模块名称
```
  

## 参考
* [go官网](https://golang.org/)
* [go入门教程](https://www.w3cschool.cn/go/)
* [Goproxy模块代理解决国内下载模块慢](https://goproxy.cn/)

