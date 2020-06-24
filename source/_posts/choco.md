---
title: win 10 安装 Chocolatey
date: 2020-06-24 14:04:25
categories: Chocolatey
tags: Chocolatey
---

## choco简介
Chocolatey 是一种软件管理解决方案，与您在 Windows 上经历过的任何其他事情都不一样。它着重于简单性，安全性和可伸缩性。您可以在 PowerShell 中为任何软件（不仅仅是安装程序）编写一次软件部署，然后可以使用任何可以管理系统（配置管理，端点管理等）并跟踪和管理该软件更新的解决方案，将其部署到 Windows 所在的任何位置。随着时间的推移。使用 Chocolatey 在本地，“云”中或 Docker 容器中管理软件。

## win10下安装choco
1. 在Win10开始菜单的搜索框中输入“命令提示符”即可自动搜索到“命令提示符”工具；
2. 右键点击“命令提示符”，选择“以管理员身份运行”即可打开“管理员:命令提示符”窗口；
3. 输入：
```sh
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```
4. 输入 choco，展示出版本号即为成功


## choco常用命令
```sh
choco search <keyword>  # 搜索软件
choco list <keyword>    # 跟 search 命令功能类似
choco install <package1 package2 ...>   # 安装软件
choco install <package>  -version ***   # 安装指定版本
choco uninstall name   # 卸载软件
choco version <package> # 查看安装包的版本情况
choco upgrade <package>    # 更新某个软件 
choco list -localonly   # 查看一下所有安装在本地的包的列表
choco list -lo  # 功能同上
```

## 包的类型
Chocolatey 的包有不同的类型，有些包的名字里面会包含特殊的后缀，比如 .install ，.commandline，.portable ，有些包的名字不带这些后缀。
* 无后缀（例：nodejs，git）
* install （例：nodejs.install，git.install）
  .install 后缀的包，这个包会出现在系统控制面板里的 卸载或更改程序 里面，你可以把 .install 的包想成是通过安装程序（msi）安装的包。
* commandline（例：nodejs.commandline，未来会被抛弃）
  .commandline（未来会被抛弃） 与 .portable 后缀的包是压缩包（zip），安装这种后缀的包，你不能在 卸载或更改程序 里找到它们。
* .portable （例：putty.portable）
你也可以选择不带后缀的包，这样如果系统中已经安装了这个包，就会跳过去，如果没安装，Chocolatey 就会为你安装一个，默认安装的这个包的类型应该就是 .install 后缀的包。

> 软件包的推荐顺序： 无后缀 > .install > .portable > .commandline


----

## 相关问题
* 问题1： Windows下cmder找不到choco
* 问题描述：在命令行下可以使用choco，可是在cmder下就找不到了，提示：“不是内部或外部命令，也不是可运行的程序”
* 解决方法：修改cmder目录下的config\user-profile.cmd文件，加上这句话：
```cmd
:: use this file to run your own startup commands
:: use  in front of the command to prevent printing the command

:: call "%GIT_INSTALL_ROOT%/cmd/start-ssh-agent.cmd"
:: set "PATH=%CMDER_ROOT%\vendor\whatever;%PATH%"

set "PATH=C:\ProgramData\chocolatey\bin;%PATH%"
```
* 然后就可以在cmder下使用choco了！


## 参考
* [Chocolatey官网](https://chocolatey.org/)
* [Chocolatey软件包网站](https://chocolatey.org/packages)
* [Chocolatey官方安装教程](https://chocolatey.org/install)



