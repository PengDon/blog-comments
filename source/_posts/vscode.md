---
title: vscode 记录
date: 2020-08-26 10:07:56
categories:
  - 开发工具
tags:
  - vscode
---

## 下载

官网下载地址：https://code.visualstudio.com/

## 常用插件

1.  Better Comments：你可以使用不同的前缀来让注释显示为不同的颜色。非常适合快速扫描并发现重要的代码片段。若使用的话, 建议团队统一规范
1.  Bracket Pair Colorizer：你是否经常在项目中出现查找是否缺失括号. 这是一件非常浪费时间的事情. 使用上面这款插件, 它可以帮你用不同的颜色标识括号
1.  change-case：提供了一种简单的方法来将单词或变量名更改为各种情况，包括 camelCase、snake_case、TitleCase…… 这种再多人合作, 遇到不一致的代码时, 可以极大地提高效率
1.  Import Cost： 有助于你认识到你引入的文件大小。它以内联方式显示每个导入的大小，如果导入大于正常大小，则显示红色和黄色警告颜色
1.  indent-rainbow：通过颜色区分, 让你一眼就看出缩进
1.  koroFileHeader: 用于生成文件头部注释和函数注释的插件

## 常见快捷键

| 快捷键组合           | 描述                       |
| -------------------- | -------------------------- |
| Ctrl+,               | 进入设置 Settings          |
| Ctrl+Shift+X         | 进入扩展（Extensinons）    |
| CTRL+K CTRL+S        | 显示快捷键                 |
| CTRL+R               | 切换工作区                 |
| ALT + Z              | 切换自动换行               |
| CTRL + G             | 转到行                     |
| CTRL + P             | 转到文件                   |
| CTRL + TAB           | 切换选项卡                 |
| SHIFT + ALT + I      | 在选定的每行末尾插入光标   |
| CTRL+L               | 选择当前行                 |
| CTRL + SHIFT + L     | 选择所有出现的当前选择     |
| CTRL + F2            | 选择所有出现的当前单词     |
| CTRL + SHIFT + SPACE | 触发参数提示               |
| SHIFT + ALT + F      | 格式化文档                 |
| CTRL + K CTRL + F    | 格式选择的代码             |
| F12                  | 转到定义                   |
| ALT+F12              | 查看定义                   |
| CTRL + K CTRL + X    | 删除尾部空格               |
| CTRL + K R           | 在资源管理器中显示活动文件 |
| CTRL + SHIFT + H     | 替换为文件                 |
| CTRL + K V           | 在右侧打开 Markdown 预览   |
| Ctrl + K Z           | 进入 Zen 模式              |

## 常见问题

**屏蔽 node_modules**

1. 文件 File->首选项 Preferences->设置 Settings，搜索“files.exclude”然后点击“Add Pattern”，输入“\*\*/node_modules”保存即可

**中文乱码**

1. 文件 File->首选项 Preferences->设置 Settings，搜索 “files.autoGuessEncoding”，打勾

**更改编辑器语言为中文**

1. 点击左侧扩展（Extensinons），搜索 Chinese，安装“Chinese (Simplified) Language Pack for Visual Studio Code”

**格式化时，单引号被改成了双引号**

1. 新建.prettierrc.json 文件，内容如下：

```json
{
  "singleQuote": true, //单引号
  "trailingComma": "es5", // 对象属性最后有 ","
  "semi": true //是否需要分号
}
```
