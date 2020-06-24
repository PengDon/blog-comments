---
title: python常用命令
date: 2020-06-22 14:36:50
categories: python
tags: python
---

## 前提
python3.8
win10 64
vscode

## pip常用命令

```sh
# 查看版本
pip -V
# 安装模块
pip install 模块名称
# 卸载模块
pip uninstall 模块名称
# 查看模块是否已安装
pip show 模块名
# 升级模块
pip install --upgrade 模块名称
# 升级pip
pip install --upgrade pip
# 列出已安装包
pip list
# 显示模块所在目录
pip show -f 模块名称
# 搜索模块
pip search 模块名称
# 查询可升级包
pip list -o
# 下载模块而不安装
pip install -d 模块名称
# 打包
pip wheel 模块名称
# 更换国内pypi镜像
## 国内镜像
阿里：https://mirrors.aliyun.com/pypi/simple
中国科学技术大学：http://pypi.mirrors.ustc.edu.cn/simple/
## 指定单次安装源
pip install -i 模块名 https://mirrors.aliyun.com/pypi/simple
## 指定全局安装源
配置文件路径：%HOME%\pip\pip.ini
文件内容：
[global]
timeout = 6000
  index-url = https://mirrors.aliyun.com/pypi/simple
```



