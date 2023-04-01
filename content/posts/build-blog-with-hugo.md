---
title: '如何用HUGO搭建博客'
date: '2023-03-17T15:41:31+0800'
tags: ['blog']
categories: []
---

## 简介

Hugo是由Go语言实现的静态网站生成器。简单、易用、高效、易扩展、快速部署。这是官方的介绍。实际安装的时候还是踩了很多坑，现在记录下来，以备后用。

[HUGO官方网站](https://gohugo.io/)

[Hugo中文文档 (gohugo.org)](https://www.gohugo.org/)

## 安装

GitHub主页：[GitHub - gohugoio/hugo](https://github.com/gohugoio/hugo)，到releases页面下载最新版本。

### Windows

1. 下载页面：[hugo_extended_0.111.3_windows-amd64.zip](https://github.com/gohugoio/hugo/releases/download/v0.111.3/hugo_extended_0.111.3_windows-amd64.zip)

   一定要下载`extended`版本的，否则有些主题没法编译

2. 将`hugo.exe`文件复制到`C:\Windows`里

    ![](https://s1.ax1x.com/2023/03/29/ppc8QBt.png)

3. 打开`cmd`或者`power shell`，输入命令`hugo version`，显示版本信息

    ![](https://s1.ax1x.com/2023/03/29/ppc83Af.png)

### MacOs

官方推荐使用 `Homebrew` 安装：

```bash
brew install hugo
```

### Linux

和windows一样，也要下载`extended`版本

```bash
# 下载
wget -c https://github.com/gohugoio/hugo/releases/download/v0.111.3/hugo_extended_0.111.3_linux-arm64.deb
# 安装本地文件
sudo dpkg -i hugo_extended_0.111.3_linux-amd64.deb
# 查看版本
hugo version
```

![](https://s1.ax1x.com/2023/03/29/ppc88N8.png)

![](https://s1.ax1x.com/2023/03/29/ppc8G4S.png)

## 使用hugo创建博客

以下操作都是在windows系统操作，别的系统都差不多，可以到官网查看。

打开`cmd`或者`power shell`

### 创建站点

使用`hugo new site`创建新站点

```
PS C:\Users\Admin> d:
PS D:\> hugo new site blog
```

![](https://s1.ax1x.com/2023/03/29/ppc8lHP.png)

创建的文件内容如下

```bash
# blog文件的结构
│─archetypes
│      default.md	# 新建md文件模板
├─assets
├─content			# 博客文章所在目录
├─data
├─layouts			# 网站布局
├─public			# 编译后的文件
├─static			# 静态文件
├─themes			# 博客主题
└─config.toml		# 配置文件
```

### 设置主题

<font size=5 color=red>HUGO必须要设置一个主题才能运行</font>

[HUGO主题](https://themes.gohugo.io/)

选一个好看的主题用`git`下载，我们选择`Tranquilpeak`这个主题

```bash
git clone https://github.com/kakawait/hugo-tranquilpeak-theme.git themes/hugo-tranquilpeak-theme
```

![](https://s1.ax1x.com/2023/03/29/ppc8dun.png)

复制`hugo-tranquilpeak\exampleSite`文件夹里的所有文件，粘贴到`blog`文件夹里。

![](https://s1.ax1x.com/2023/03/29/ppc8Y9g.png)


![](https://s1.ax1x.com/2023/03/29/ppc8t3Q.png)

![](https://s1.ax1x.com/2023/03/29/ppc8Ncj.png)

### 启动网站

输入命令

```bash
hugo server
```

![](https://s1.ax1x.com/2023/03/29/ppc8Ujs.png)

在浏览器输入`http://localhost:1313/`打开，即可看到博客主页面。

![](https://s1.ax1x.com/2023/03/29/ppc80H0.png)

### 创建文章

你只需要按照Markdown 的格式编写自己的文章，然后将写好的文章放在`blog/content/posts`，hugo 就会读取到这片文章，并将这片文章展示在你的博客中。

与普通Markdown 文章不一样的地方是，你需要在文章的开头写入如下结构的内容，这些内容包含在三杠线之间，在三杠线下边就是Markdown 的正文了：

```yaml
---
title: "文章标题"           # 文章标题
author: "作者"              # 文章作者
description : "描述信息"    # 文章描述信息
date: 2015-09-28            # 文章编写日期
lastmod: 2015-04-06         # 文章修改日期
tags:	                    # 文章所属标签
- "文章标签1"
- "文章标签2"
categories:	                # 文章所属分类
- "文章分类1"
- "文章分类2"
keywords: 	                # 文章关键词
- "Hugo"
- "static"
- "generator"

# 下面是模版自带的参数
autoThumbnailImage: false	# 自动缩略图
thumbnailImagePosition: "top"	# 在首页显示图片位置 top、right、left
# 在首页显示的图片 如果图片在static/img/showcase.png
# thumbnailImage: "/img/showcase.png"，默认先从blog/static文件夹里寻找
thumbnailImage: //d1u9biwaxjngwg.cloudfront.net/welcome-to-tranquilpeak/city-750.jpg
# 文章页面显示的图片
coverImage: //d1u9biwaxjngwg.cloudfront.net/welcome-to-tranquilpeak/city.jpg
# 文章页面标题显示位置：top、right、left
metaAlignment: center
---

Markdown正文
```

比如我们编写了这样一篇文章，文件名为hello-hugo.md



```markdown
---
title: "Hello HUGO"
author: "Jack"
description : "我的第一篇bolg~"
date: 2023-03-19
lastmod: 2023-03-20
tags:
- "博客标签1"
- "博客标签2"
categories:
- "博客分类"
keywords:
- "博客关键字"

thumbnailImagePosition: "left"
thumbnailImage: "/img/showcase.png"
---

欢迎来到我的blog~

这里在主页显示
<\!--more-->
注：实际应用时要删除!前面的\
# H1
> 引用

**加粗**

```


我们将文件保存在`blog/content/posts` ，打开地址`http://localhost:1313/`，可以看到现在的博客中，新增了一篇文章：

![](https://s1.ax1x.com/2023/03/29/ppc8DEV.png)

![](https://s1.ax1x.com/2023/03/29/ppc8wBq.png)

### 配置文件

博客的配置文件为`config.toml`可以根据自己的需要修改，我们来看下主题的配置文件，这些配置属性通过英文并不难理解。



```toml
# 网站地址
baseURL = "https://example.org/"
# 网站语言
languageCode = "en-us"
# 如果模版里有i18n文件夹，则会显示对应语言
defaultContentLanguage = "zh-cn"
# 网站title
title = "My Blog"
# 主题的名字，这个要跟myblog/themes 目录中的子目录的目录名一致
theme = "hugo-tranquilpeak-theme"
# 谷歌分析工具，如果公网访问可以打开
# googleAnalytics = "UA-123-45"
# 分页，每页显示文章数
paginate = 10

[permalinks]
  # 文章链接会按http://localhost:1313/2023/03/hello-hugo/显示
  posts = "/:year/:month/"

[taxonomies]
  tag = "tags"
  category = "categories"
  archive = "archives"

[markup]
  # Set startLevel = 1 to support # title (<h1>title</h1>) otherwise table of content is blank/empty
  [markup.tableOfContents]
    endLevel = 3
    ordered = false
    startLevel = 1

# 主页的左侧页面设置和关于页面显示的内容
[author]
  name = "Firstname Lastname"
  bio = "Super bio with markdown support **COOL**"
  job = "Your job title"
  location = "China"
  # 图片，也可以调用static文件夹里的图片picture = "/img/head.jpg"
  picture = "https://cdn1.iconfinder.com/data/icons/ninja-things-1/1772/ninja-simple-512.png"

#侧边栏菜单
[[menu.main]]
  weight = 1
  identifier = "home"
  name = "Home"
  pre = "<i class=\"sidebar-button-icon fas fa-lg fa-home\" aria-hidden=\"true\"></i>"
  url = "/"
[[menu.main]]
  weight = 2
  identifier = "categories"
  name = "Categories"
  pre = "<i class=\"sidebar-button-icon fas fa-lg fa-bookmark\" aria-hidden=\"true\"></i>"
  url = "/categories"
[[menu.main]]
  weight = 3
  identifier = "tags"
  name = "Tags"
  pre = "<i class=\"sidebar-button-icon fas fa-lg fa-tags\" aria-hidden=\"true\"></i>"
  url = "/tags"
[[menu.main]]
  weight = 4
  identifier = "archives"
  name = "Archives"
  pre = "<i class=\"sidebar-button-icon fas fa-lg fa-archive\" aria-hidden=\"true\"></i>"
  url = "/archives"
[[menu.main]]
  weight = 5
  identifier = "about"
  name = "About"
  pre = "<i class=\"sidebar-button-icon fas fa-lg fa-question\" aria-hidden=\"true\"></i>"
  url = "/#about"

[[menu.links]]
  weight = 1
  identifier = "github"
  name = "GitHub"
  pre = "<i class=\"sidebar-button-icon fab fa-lg fa-github\" aria-hidden=\"true\"></i>"
  url = "https://github.com/"
  
[[menu.misc]]
  weight = 1
  identifier = "rss"
  name = "RSS"
  pre = "<i class=\"sidebar-button-icon fas fa-lg fa-rss\" aria-hidden=\"true\"></i>"
  url = "/index.xml"

# 个人定制
[params]
  # head标签里<meta name="description" content="myblog">
  # 显示在文章页的顶部
  description = "myblog"
  # 代码高亮
  syntaxHighlighter = "highlight.js"
  # 日期格式化，默认2 January 2006
  # 日期必须填2006-01-02，否则显示错误
  dateFormat = "2006-01-02"
  # 在所有页面隐藏侧边栏 true:隐藏，false:不隐藏
  clearReading = true
  # 侧边栏大小，无需修改
  sidebarBehavior = 1
  # 侧边栏背景图片
  coverImage = "images/cover.jpg"
  # 在有图片的文章底部显示图片库，无需修改
  imageGallery = true
  # 在首页显示文章的缩略图片 
  thumbnailImage = true
  # favicon
  #  favicon = "/favicon.png"

  # 加载自定义js
  #  [[params.customJS]]
  #     src = "js/myscript.js"
  
  # 加载自定义css
  #  [[params.customJS]]
  #     src = "css/custom.css"

  #底部分享按钮
#  [[params.sharingOptions]]
#    name = "Twitter"
#    icon = "fab fa-twitter"
#    url = "https://twitter.com/intent/tweet?text=%s"
  # 页面右上角小图标设置
  [params.header.rightLink]
    class = ""
    icon = ""
    url = "/#about"
# 自定义页脚
  #  [params.footer]
  #    copyright = "<a href=\"https://github.com/kakawait\">kakawait</a>"

```

---

