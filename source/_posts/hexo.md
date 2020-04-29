---
title: hexo 基础
date: 2020-04-29 09:54:54
categories: 
- blog
tags: 
- hexo
---

## 前提
* Node10^
* git

## 什么是 Hexo？
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装 Hexo
```sh
npm install -g hexo-cli
```
## 建站
1、 执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件
```sh
hexo init <folder>
cd <folder>
npm install
```
2、 工程默认结构
```js
// Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去
├── _config.yml   // 网站的 配置 信息，您可以在此配置大部分的参数
├── package.json  // 应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，也可以自由移除
├── scaffolds     // 模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件
├── source        // 资源文件夹是存放用户资源的地方。
|    ├── _drafts  // 
|    └── _posts   // 除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略
└── themes        // 主题 文件夹。Hexo 会根据主题来生成静态页面
```

## _config.yml 配置
1、 **网站**

参数 | 描述
---  | ---
title |	网站标题
subtitle |	网站副标题
description |	网站描述，主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词
keywords |	网站的关键词。使用半角逗号 , 分隔多个关键词。
author |	您的名字，用于主题显示文章的作者
language |	网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 zh-Hans和 zh-CN
timezone |	网站时区。Hexo 默认使用您电脑的时区。请参考 时区列表 进行设置，如 America/New_York, Japan, 和 UTC 。一般的，对于中国大陆地区可以使用 Asia/Shanghai

2、 **网址**

参数 | 描述 | 默认值
---  | ---  | --- 
url |	网址	
root |	网站根目录	
permalink |	文章的 永久链接 格式 |	:year/:month/:day/:title/
permalink_defaults |	永久链接中各部分的默认值	
pretty_urls |	改写 permalink 的值来美化 URL	
pretty_urls.trailing_index |	是否在永久链接中保留尾部的 index.html，设置为 false 时去除 |	true
pretty_urls.trailing_html |	是否在永久链接中保留尾部的 .html, 设置为 false 时去除 (对尾部的 index.html无效) |	true

> 网站存放在子目录,如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/
```sh
# 比如，一个页面的永久链接是 http://example.com/foo/bar/index.html
pretty_urls:
  trailing_index: false
# 此时页面的永久链接会变为 http://example.com/foo/bar/
```

3、**目录**

参数 | 描述 | 默认值
---  | ---  | --- 
source_dir |	资源文件夹，这个文件夹用来存放内容。 |	source
public_dir |	公共文件夹，这个文件夹用于存放生成的站点文件。 |	public
tag_dir |	标签文件夹 |	tags
archive_dir |	归档文件夹 |	archives
category_dir |	分类文件夹 |	categories
code_dir	Include code | 文件夹，source_dir 下的子目录 |	downloads/code
i18n_dir |	国际化（i18n）文件夹 |	:lang
skip_render |	跳过指定文件的渲染。匹配到的文件将会被不做改动地复制到 public 目录中。您可使用 glob 表达式来匹配路径。	

```sh
skip_render: "mypage/**/*"
# 将会直接将 `source/mypage/index.html` 和 `source/mypage/code.js` 不做改动地输出到 'public' 目录
# 你也可以用这种方法来跳过对指定文章文件的渲染
skip_render: "_posts/test-post.md"
# 这将会忽略对 'test-post.md' 的渲染
```
> **提示**
> 如果您刚刚开始接触 Hexo，通常没有必要修改这一部分的值。

4、**文章**

参数 | 描述 | 默认值
---  | ---  | --- 
new_post_name |	新文章的文件名称|	:title.md
default_layout|	预设布局|	post
auto_spacing|	在中文和英文之间加入空格|	false
titlecase|	把标题转换为 |title case	false
external_link|	在新标签中打开链接	|true
external_link.enable|	在新标签中打开链接|	true
external_link.field	|对整个网站（site）生效或仅对文章（post）生效|	site
external_link.exclude	|需要排除的域名。主域名和子域名如 www 需分别配置|	[]
filename_case|	把文件名称转换为 (1) 小写或 (2) 大写|	0
render_drafts|	显示草稿	|false
post_asset_folder|	启动 Asset 文件夹	|false
relative_link|	把链接改为与根目录的相对位址	|false
future|	显示未来的文章|	true
highlight|代码块的设置	
highlight.enable|	开启代码块高亮|	true
highlight.auto_detect|	如果未指定语言，则启用自动检测|	false
highlight.line_number|	显示行数
Enabling| this option will also enable wrap option|	true
highlight.tab_replace|	用 n 个空格替换 tabs；如果值为空，则不会替换tabs |''
highlight.wrap | Wrap the code block in table | true
highlight.hljs |	Use the hljs-* prefix for CSS classes |	false

> **相对地址**
> 默认情况下，Hexo 生成的超链接都是绝对地址。例如，如果您的网站域名为 example.com,您有一篇文章名为 hello，那么绝对链接可能像这样：http://example.com/hello.html，它是绝对于域名的。相对链接像这样：/hello.html，也就是说，无论用什么域名访问该站点，都没有关系，这在进行反向代理时可能用到。通常情况下，建议使用绝对地址。

5、**分类&标签**

参数 | 描述 | 默认值
---  | ---  | --- 
default_category	|默认分类	|uncategorized
category_map|	分类别名	
tag_map	|标签别名	

6、 **日期 / 时间格式**

参数 | 描述 | 默认值
---  | ---  | --- 
date_format|	日期格式|	YYYY-MM-DD
time_format	|时间格式|	HH:mm:ss
use_date_for_updated|	启用以后，如果 Front Matter 中没有指定 updated， post.updated 将会使用 date 的值而不是文件的创建时间。在 Git 工作流中这个选项会很有用|	true

7、**分页**

参数 | 描述 | 默认值
---  | ---  | --- 
per_page |	每页显示的文章量 (0 = 关闭分页功能) |	10
pagination_dir |	分页目录 |	page

8、**扩展**

参数 | 描述
---  | ---
theme|	当前主题名称。值为false时禁用主题
theme_config|	主题的配置文件。在这里放置的配置会覆盖主题目录下的 _config.yml 中的配置
deploy|	部署部分的设置
meta_generator|	Meta generator 标签。 值为 false 时 Hexo 不会在头部插入该标签

9、**包括或不包括目录和文件**
> 在 Hexo 配置文件中，通过设置 include/exclude 可以让 Hexo 进行处理或忽略某些目录和文件夹。你可以使用 glob 表达式 对目录和文件进行匹配。
> include and exclude options only apply to the source/ folder, whereas ignore option applies to all folders.

参数 | 描述
---  | ---
include |	Hexo 默认会忽略隐藏文件和文件夹（包括名称以下划线和 . 开头的文件和文件夹，Hexo 的 _posts 和 _data 等目录除外）。通过设置此字段将使 Hexo 处理他们并将它们复制到 source 目录下。
exclude |	Hexo 会忽略这些文件和目录
ignore|	Ignore files/folders

```sh
# Include/Exclude Files/Folders
include:
  - ".nojekyll"
  # 包括 'source/css/_typing.css'
  - "css/_typing.css"
  # 包括 'source/_css/' 中的任何文件，但不包括子目录及其其中的文件。
  - "_css/*"
  # 包含 'source/_css/' 中的任何文件和子目录下的任何文件
  - "_css/**/*"

exclude:
  # 不包括 'source/js/test.js'
  - "js/test.js"
  # 不包括 'source/js/' 中的文件、但包括子目录下的所有目录和文件
  - "js/*"
  # 不包括 'source/js/' 中的文件和子目录下的任何文件
  - "js/**/*"
  # 不包括 'source/js/' 目录下的所有文件名以 'test' 开头的文件，但包括其它文件和子目录下的单文件
  - "js/test*"
  # 不包括 'source/js/' 及其子目录中任何以 'test' 开头的文件
  - "js/**/test*"
  # 不要用 exclude 来忽略 'source/_posts/' 中的文件。你应该使用 'skip_render'，或者在要忽略的文件的文件名之前加一个下划线 '_'
  # 在这里配置一个 - "_posts/hello-world.md" 是没有用的。

ignore:
  # Ignore any folder named 'foo'.
  - "**/foo"
  # Ignore 'foo' folder in 'themes/' only.
  - "**/themes/*/foo"
  # Same as above, but applies to every subfolders of 'themes/'.
  - "**/themes/**/foo"
```
> 列表中的每一项都必须用单引号或双引号包裹起来。
> include 和 exclude 并不适用于 themes/ 目录下的文件。如果需要忽略 themes/ 目录下的部分文件或文件夹，可以使用 ignore 或在文件名之前添加下划线 _。

**使用代替配置文件**

> 可以在 hexo-cli 中使用 --config 参数来指定自定义配置文件的路径。你可以使用一个 YAML 或 JSON 文件的路径，也可以使用逗号分隔（无空格）的多个 YAML 或 JSON 文件的路径。例如：
```sh
# use 'custom.yml' in place of '_config.yml'
$ hexo server --config custom.yml

# use 'custom.yml' & 'custom2.json', prioritizing 'custom3.yml', then 'custom2.json'
$ hexo generate --config custom.yml,custom2.json,custom3.yml
```
当你指定了多个配置文件以后，Hexo 会按顺序将这部分配置文件合并成一个 _multiconfig.yml。如果遇到重复的配置，排在后面的文件的配置会覆盖排在前面的文件的配置。这个原则适用于任意数量、任意深度的 YAML 和 JSON 文件。
```sh
# 例如，使用 --options 指定了两个自定义配置文件：
hexo generate --config custom.yml,custom2.json
```
如果 custom.yml 中指定了 foo: bar，在 custom2.json 中指定了 "foo": "dinosaur"，那么在 _multiconfig.yml 中你会得到 foo: dinosaur。

**覆盖主题配置**
通常情况下，Hexo 主题是一个独立的项目，并拥有一个独立的 _config.yml 配置文件。
你可以在站点的 _config.yml 配置文件中配置你的主题，这样你就不需要 fork 一份主题并维护主题独立的配置文件。

以下是一个覆盖主题配置的例子：
```sh
# _config.yml
theme_config:
  bio: "My awesome bio"
```
```sh
# themes/my-theme/_config.yml
bio: "Some generic bio"
logo: "a-cool-image.png"
```
最终主题配置的输出是：
```json
{
  bio: "My awesome bio",
  logo: "a-cool-image.png"
}
```

## 指令

命令 | 描述
--- | ---
hexo init [folder] | 新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站
hexo new [layout] title | 新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来
hexo generate | 生成静态文件(简写写：hexo g)
hexo publish [layout] filename | 发表草稿
hexo server | 启动服务器。默认情况下，访问网址为： http://localhost:4000/
hexo deploy | 部署网站(简写：hexo d)
hexo render file1 [file2] ... | 渲染文件
hexo migrate type | 从其他博客系统 迁移内容。
hexo clean | 清除缓存文件 (db.json) 和已生成的静态文件 (public)
hexo list type | 列出网站资料
hexo version | 显示 Hexo 版本
hexo --safe | 在安全模式下，不会载入插件和脚本。当您在安装新插件遭遇问题时，可以尝试以安全模式重新执行。
hexo --debug | 在终端中显示调试信息并记录到 debug.log
hexo --silent | 简洁模式,隐藏终端信息
hexo --draft | 显示 source/_drafts 文件夹中的草稿文章
hexo --cwd /path/to/cwd | 自定义当前工作目录（Current working directory）的路径


**自定义配置文件的路径**
```sh
# 使用 custom.yml 代替默认的 _config.yml
$ hexo server --config custom.yml

# 使用 custom.yml 和 custom2.json，其中 custom2.json 优先级更高
$ hexo generate --config custom.yml,custom2.json,custom3.yml
```
自定义配置文件的路径，指定这个参数后将不再使用默认的 _config.yml。
你可以使用一个 YAML 或 JSON 文件的路径，也可以使用逗号分隔（无空格）的多个 YAML 或 JSON 文件的路径。例如：
```sh
# 使用 custom.yml 代替默认的 _config.yml
$ hexo server --config custom.yml

# 使用 custom.yml, custom2.json 和 custom3.yml，其中 custom3.yml 优先级最高，其次是 custom2.json
$ hexo generate --config custom.yml,custom2.json,custom3.yml
```
当你指定了多个配置文件以后，Hexo 会按顺序将这部分配置文件合并成一个 _multiconfig.yml。如果遇到重复的配置，排在后面的文件的配置会覆盖排在前面的文件的配置。这个原则适用于任意数量、任意深度的 YAML 和 JSON 文件。






## 参考
* [hexo](https://hexo.io/zh-cn/)
