---
title: Hexo使用入门
layout: page
toc: true # 是否生成目录
indent: true # 是否首行缩进
archive: true # 是否显示在归档
cover: false # 是否显示封面
mathjax: false # 是否渲染公式
pin: false # 是否首页置顶
top_meta: false # 是否显示顶部信息
bottom_meta: false # 是否显示尾部信息
sidebar: [toc]
tag:
  - hexo
categories: Hexo
keywords: Hexo使用
date: { { date } }
description:
icons: [fas fa-fire red, fas fa-star green]
---

---

## hexo 安装

```shell
brew install hexo
# 或
npm install -g hexo-cli
```

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```shell
hexo init <folder>
cd <folder>
npm install
```

初始化后，您的项目文件夹将如下所示：

```os
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

## 指令

## init

```shell
hexo init [folder]
```

新建一个网站。 如果没有设置 `folder` ，Hexo 默认在目前的文件夹建立网站。

## new

执行以下命令新建一个页面

```shell
hexo new [layout] <title>
```

- layout 可选,默认在`_config.yml` 的 `default_layout`设置

hexo 具有三种默认布局:

| Layout  |       Path       |
| :------ | :--------------: |
| `post`  | `source/_posts`  |
| `page`  |     `source`     |
| `draft` | `source/_drafts` |

| 选项              | 描述                            |
| :---------------- | :------------------------------ |
| `-p`, `--path`    | 文章的路径。 自定义文章的路径。 |
| `-r`, `--replace` | 如果存在的话，替换当前的文章。  |
| `-s`, `--slug`    | 文章别名。 自定义文章的 URL。   |

默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。 对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 `index.md` 文件。 你可以使用 `--path` 参数来覆盖上述行为、自行决定文件的目录：

```shell
hexo new page --path about/me "About me"
```

以上命令会创建一个 `source/about/me.md` 文件，同时 Front Matter 中的 title 为 `"About me"`

注意！ title 是必须指定的！

## generate

```shell
hexo generate
```

| 选项                  | 描述                                         |
| :-------------------- | :------------------------------------------- |
| `-d`, `--deploy`      | Deploy after generation finishes             |
| `-w`, `--watch`       | 监视文件变动                                 |
| `-b`, `--bail`        | 生成过程中如果发生任何未处理的异常则抛出异常 |
| `-f`, `--force`       | 强制重新生成                                 |
| `-c`, `--concurrency` | 要同时生成的文件的最大数量。 默认无限制      |

## publish

```shell
hexo publish [layout] <filename>
```

发表草稿。

## server

```shell
hexo server
```

启动服务器。 默认情况下，访问网址为： `http://localhost:4000/`。

| 选项             | 描述                                   |
| :--------------- | :------------------------------------- |
| `-p`, `--port`   | 重设端口                               |
| `-s`, `--static` | 只使用静态文件                         |
| `-l`, `--log`    | Enable logger. Override logger format. |

## deploy

```shell
hexo deploy
```

部署你的网站。

| 选项               | 描述                       |
| :----------------- | :------------------------- |
| `-g`, `--generate` | Generate before deployment |

## render

```shell
hexo render <file1> [file2] ...
```

渲染文件。

| 选项             | 描述               |
| :--------------- | :----------------- |
| `-o`, `--output` | Output destination |

## 自定义配置文件的路径

```shell
hexo --config custom.yml
```

自定义配置文件的路径，指定这个参数后将不再使用默认的 `_config.yml`。 还接受一个以逗号分隔的 JSON 或 YAML 配置文件列表（无空格），该列表将把这些文件合并为一个 `_multiconfig.yml` 文件。

```shell
hexo --config custom.yml,custom2.json
```

## 显示草稿

```shell
hexo --draft
```

显示 `source/_drafts` 文件夹中的草稿文章。

## 文件名

默认的, hexo 使用标题作为文件名

可以在`_config.yml`的 new_post_name` 中设置

可以通过占位符自定义文件名,例如 `:year-:month-:day-:title.md`

| Placeholder | 描述                                |
| :---------- | :---------------------------------- |
| `:title`    | 标题（小写，空格将会被替换为短杠）  |
| `:year`     | 建立的年份，比如， `2015`           |
| `:month`    | 建立的月份（有前导零），比如， `04` |
| `:i_month`  | 建立的月份（无前导零），比如， `4`  |
| `:day`      | 建立的日期（有前导零），比如， `07` |
| `:i_day`    | 建立的日期（无前导零），比如， `7`  |

## 草稿

使用`draft`布局的文件将被保存在`source/_drafts`

可以使用`publish` 将 draft 文件移动到`source/_posts`

```shell
hexo publish [layout] <title>
```

默认情况下不显示草稿。您可以在运行 Hexo 时添加 `--draft` 选项或启用 `_config.yml` 中的 render_drafts 设置来渲染草稿。

## 脚手架

在新建文章时，Hexo 会根据 `scaffolds` 文件夹内相对应的文件来建立文件。 例如：

```shell
hexo new photo "My Gallery"
```

在执行这行指令时，Hexo 会尝试在 `scaffolds` 文件夹中寻找 `photo.md`，并根据其内容建立文章。 以下是您可以在模版中使用的变量：

| Placeholder | 描述         |
| :---------- | :----------- |
| `layout`    | 布局         |
| `title`     | 标题         |
| `date`      | 文件建立日期 |

## 支持的格式

Hexo 支持以任何格式书写文章，只要安装了相应的渲染插件。

例如，Hexo 默认安装了 `hexo-renderer-marked` 和 `hexo-renderer-ejs`，因此你不仅可以用 Markdown 写作，你还可以用 EJS 写作。 如果你安装了 `hexo-renderer-pug`，你甚至可以用 Pug 模板语言书写文章。

只需要将文章的扩展名从 `md` 改成 `ejs`，Hexo 就会使用 `hexo-renderer-ejs` 渲染这个文件，其他格式同理。

## hexo 常用指令

在本地运行博客用于测试

```shell
hexo c && hexo g && hexo s
```

## 将 hexo 部署到 github

创建 ${username}.github.io 项目,将博客文件移动到文件夹中

```shell
mkdir richardthunder.github.io
mv hexoBlog richardthunder.github.io
cd richardthunder.github.io
```

安装 git 依赖

```shell
npm install hexo-deployer-git --save
```

修改 \_config.yml 中的 Deployment

```shell
deploy:
  type: git
  repo: https://github.com/richardthunder/richardthunder.github.io.git
  branch: main
```

清除之前的静态文件, 建立静态文件,部署至 github

```shell
hexo cl && hexo g  && hexo d
```

## 安装 next 主题

官网 [Next](https://theme-next.js.org/)

```shell
git clone https://github.com/next-theme/hexo-theme-next themes/next
```

### 新建 \_config.next.yml

```shell
touch _config.next.yml
# or
cp themes/next/_config.yml _config.next.yml
```

### 更改\_config.yml

```shell
theme: next
```

### 设置主题

[配置主题](https://theme-next.js.org/docs/theme-settings/)

## 本地搜索功能

安装

```shell
npm install hexo-generator-searchdb
```

\_config.yml

```shell
search:
  path: search.xml
  field: post
  content: true
  format: html
```

\_config.next.yml

```shell
# Local search
# Dependencies: https://github.com/next-theme/hexo-generator-searchdb
local_search:
  enable: true
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false

  #修改
codeblock:
  copy_button:
    enable: true

```
