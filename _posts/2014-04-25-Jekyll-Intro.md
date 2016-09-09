---
layout : post
title : Jekyll 入门
categories : [Jekyll]
tags : [Jekyll, 入门]
---

**本文源自 [GitHub用户 @alex86gbk 的项目 pages](https://github.com/alex86gbk/pages/blob/master/_posts/2011-11-19-jekyll-intro.md) ，仅稍作修改和更新**

## 1. 安装 ##

Jekyll使用动态脚本语言 [Ruby](http://zh.wikipedia.org/wiki/Ruby) 写成。请首先 [下载并安装Ruby（中文）](http://www.ruby-lang.org/zh_cn/downloads/) 。

在使用Jekyll之前，你可能想要对Ruby语言有一些初步了解（非必需）。推荐阅读官方网站上的 [20分钟体验Ruby（中文）](http://www.ruby-lang.org/zh_cn/documentation/quickstart/) 。

安装Jekyll的最好方式是通过RubyGems：

	$ gem install jekyll

Jekyll依赖以下的gems模块： `liquid` 、 `fast-stemmer` 、 `classifier` 、 `directory_watcher` 、 `syntax` 、 `maruku` 、 `kramdown` 、 `posix-spawn` 和 `albino` 。它们会被 `gem install` 命令自动安装。

如果你在gem的安装过程中遇到了问题，你可能需要安装用于在Ruby 1.8上编译扩展模块的头文件。在Debian系统上使用如下命令：

	$ sudo apt-get install ruby1.8-dev

在Red Hat / CentOS / Fedora平台上则是：

 	$ sudo yum install ruby-devel

在 **NearlyFreeSpeech** 上你需要：

	$ RB_USER_INSTALL=true gem install jekyll

如果你在Windows平台上看到如下错误提示信息： Failed to build gem native extension ，你可能需要安装 [RubyInstaller DevKit](http://wiki.github.com/oneclick/rubyinstaller/development-kit) 。

在OS X上，你可能需要升级你的RubyGems：

	$ gem update --system

**LaTeX转换到PNG**

[Maruku](http://maruku.rubyforge.org/) 内置可选的转换LaTeX格式到PNG图片的支持。它通过0.6版的blahtex进行渲染，这和 dvips 同样都必须存在于你的$PATH环境变量中。

（注意： [remi的Maruku分支](http://github.com/remi/maruku/tree/master) 并不使用修正的 dvips 路径，如果你需要修改它的话。）

**kramdown**

如果你想用 [kramdown](http://kramdown.gettalong.org/index.html) 取代 [Maruku](http://maruku.rubyforge.org/) 作为你的Markdown标记语言转换引擎（由于[Maruku](http://maruku.rubyforge.org/)[已经停止维护](http://benhollis.net/blog/2013/10/20/maruku-is-obsolete/)，[GitHub官方推荐使用](https://help.github.com/articles/migrating-your-pages-site-from-maruku)[kramdown](http://kramdown.gettalong.org/index.html)来取代[Maruku](http://maruku.rubyforge.org/)），只需确认安装：

 	$ gem install kramdown

并通过以下命令行参数执行Jekyll：

	$ jekyll --kramdown

或者也可以在你站点下的 `_config.yml` 文件中加入以下配置，以便以后每次执行时不必再指定命令行参数：

	markdown: kramdown

**RedCloth**

若要使用Textile标记语言，需要安装相应的转换引擎 [RedCloth](http://redcloth.org/) 。

	$ gem install RedCloth

**Pygments**

若要通过 {&#37; highlight &#37;} 标签在帖子中嵌入语法高亮的代码，那么需要安装 [Pygments](http://pygments.org/) 。

**在OS X Leopard和Snow Leopard平台上：**

它已经包含在了Python 2.6的预安装包当中：

	$ sudo easy_install Pygments

**在OS X平台上（使用MacPorts）：**

	$ sudo port install python25 py25-pygments

**在OS X平台上（使用Homebrew）：**

	$ brew install python
	# export PATH="/usr/local/Cellar/python:${PATH}"
	$ easy_install pip
	$ pip install --upgrade distribute
	$ pip install pygments

**注意：** Homebrew并不会为你自动创建可执行文件的符号链接。如果使用Homebrew默认的Cellar目录位置和Python 2.7，请确保你已添加 /usr/local/Cellar/python 到你的 PATH 变量中。

**在Arch Linux平台上：**

	$ sudo pacman -S python-pygments
	$ sudo easy_install-3.2 Pygments

或选择使用python2版本的pygments：（不推荐）

	$ sudo pacman -S python2-pygments
	$ sudo easy_install-2.7 Pygments

**注意：** python2版本的pygments可执行文件名为 pygmentize2 ，而Jekyll所调用的可执行文件是 pygmentize 。你应当创建一个指向pygmentize的符号链接 # ln -s /usr/bin/pygmentize2 /usr/bin/pygmentize ，或者直接选择使用Python 3版本的pygments。

**在Ubuntu和Debian平台上：**

	$ sudo apt-get install python-pygments

**在Fedora平台上：**

	$ sudo yum install python-pygments

**在Gentoo平台上：**

	$ sudo emerge -av dev-python/pygments

## 2. 使用 ##

一旦Jekyll安装成功后，搭建一个Jekyll站点通常包括下面几步：

1.  设定站点的基本结构，使用HTML和Liquid模板语言创建网页布局。
1.  创建一些帖子，或者从你以前的博客平台导入。
1.  在本地测试站点，查看效果。
1.  部署你的网站。

**基本结构**

Jekyll从核心上来说是一个文本转换引擎。该系统内部的工作原理是：你输入一些用自己喜爱的标记语言格式书写的文本，可以是Markdown、Textile或纯粹的HTML，它将这些文本混合后放入一个或一整套页面布局当中。在整个过程中，你可以自行决定你的站点URL的模式、以及哪些数据将被显示在页面中，等等。这一切都将通过严格的文本编辑完成，而生成的Web界面则是最终的产品。

一个典型的Jekyll站点通常具有如下结构：

	 .
	|-- _config.yml
	|-- _includes
	|-- _layouts
	|   |-- default.html
	|   `-- post.html
	|-- _posts
	|   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
	|   `-- 2009-04-26-barcamp-boston-4-roundup.textile
	|-- _site
	`-- index.html

以下是每部分功能的简述：

**_config.yml**

保存Jekyll配置的文件。虽然绝大部分选项可以通过命令行参数指定，但将它们写入配置文件可以使你在每次执行时不必记住它们。

**_includes/**

该目录存放可以与\_layouts和\_posts混合、匹配并重用的文件。Liquid标签\{\% include file.ext \%\}可以用于嵌入文件_includes/file.ext。

**_layouts/**

该目录存放用来插入帖子的网页布局模板。页面布局基于类似博客平台的“一个帖子接一个帖子”的原则，通过YAML前置数据定义。Liquid标签用于在页面上插入帖子的文本内容。

**_posts/**

该目录下存放的可以说成是你的“动态内容”。这些文件的格式很重要，它们的命名模式必须遵循 YEAR-MONTH-DATE-title.MARKUP 。每一个帖子的固定链接URL可以作弹性的调整，但帖子的发布日期和转换所使用的标记语言会根据且仅根据文件名中的相应部分来识别。

**_site/**

这里是Jekyll用以存放最终生成站点的根路径位置。也许把它加到你的 .gitignore 列表中会是个不错的主意。

**index.html和其他HTML/Markdown/Textile文件**

如果一个文件的头部存在YAML前置数据的部分，那么Jekyll将会自动处理转换该文件并传送到站点路径下。这对于站点的根目录或其他任意子目录下的所有 .html 、 .markdown 、 .textile 文件都适用。

**其他文件/目录**

除了以上提到的文件之外，每一个其他的、不以下划线_开头的目录和文件都会被照原样传送到站点路径下。例如，你可以在网站根目录下面添加一个 css 目录，一个 favicon.ico ，等等等等。假如你对于怎样实现静态站点布局仍然感到困惑的话，那么这里有不少 [基于Jekyll的站点](https://github.com/mojombo/jekyll/wiki/Sites) 可供参考。

在这些目录下的文件同样会被解析转换和传送，依据处理根目录下文件时的相同规则。

**运行Jekyll**

通常直接在命令行下使用可执行的Ruby脚本 jekyll ，它可以从gem安装。如果要启动一个临时的Web服务器并测试你的Jekyll站点，执行：

	$ jekyll serve

然后在浏览器中访问 [http://localhost:4000](http://localhost:4000) 或 [http://0.0.0.0:4000](http://0.0.0.0:4000) 。当然这里还有其他许多参数选项可以使用。

在Debian或Ubuntu下，你可能需要将 /var/lib/gems/1.8/bin/ 加到你的PATH环境变量中。

**部署**

由于Jekyll所做的仅仅是生成一个包含HTML等静态网站文件的目录（_site），它可以通过简单的拷贝（scp）、远程同步（rsync）、ftp上传或git等方式部署到任何Web服务器上。

## 3. 配置 ##

Jekyll允许你以任何可能的方式配置你的站点。以下是目前支持的配置选项的列表。它们均可通过站点根目录下的配置文件 _config.yml 指定。 jekyll 可执行文件同样有一些命令行参数对应于这些配置选项。当配置出现冲突时的优先级顺序是：

1.  命令行参数
2.  配置文件中的选项
3.  默认值
 
**配置选项和命令行参数**

<table>
	<tbody>
    <tr>
        <td><b>设定</b></td><td><b>配置文件</b></td><td><b>命令行参数</b></td><td><b>简述</b></td>
    </tr>
    <tr>
        <td>重新生成</td><td>auto: [boolean]</td><td>--no-auto --auto </td><td>允许或禁止Jekyll在文件被修改后重新生成整个站点</td>
    </tr>
    <tr>
        <td>本地服务器</td><td>server: [boolean]</td><td>--server</td><td>自动开启一个用于托管_site目录的本地Web服务器</td>
    </tr>
    <tr>
        <td>本地服务器端口</td><td>server_port: [integer]</td><td>--server [port]</td><td>更改Jekyll所使用的服务器端口</td>
    </tr>
    <tr>
        <td>Base URL</td><td>baseurl: [BASE_URL]</td><td>--base-url [url]</td><td>使用指定的Base URL在服务器上运行站点</td>
    </tr>
    <tr>
        <td>站点目的路径</td><td>destination: [dir]</td><td>jekyll [dest]</td><td>更改Jekyll存放生成文件的路径</td>
    </tr>
    <tr>
        <td>站点源路径</td><td>source: [dir]</td><td>jekyll [source] [dest]</td><td>更改Jekyll所处理文件的路径</td>
    </tr>
    <tr>
        <td>Markdown</td><td>markdown: [engine]</td><td>--rdiscount or --kramdown</td><td>使用RDiscount或[engine]以取代Maruku</td>
    </tr>
    <tr>
        <td>Pygments</td><td>pygments: [boolean]</td><td>--pygments</td><td>允许Pygments处理代码语法高亮</td>
    </tr>
    <tr>
        <td>LSI</td><td>lsi: [boolean]</td><td>--lsi</td><td>产生相关帖子的索引</td>
    </tr>
    <tr>
        <td>固定链接</td><td>permalink: [style]</td><td>--permalink=[style]</td><td>控制生成帖子的URL</td>
    </tr>
    <tr>
        <td>分页</td><td>paginate: [per_page]</td><td>--paginate [per_page]</td><td>将你的帖子分成多个子目录："page2"、"page3"、……"pageN"</td>
    </tr>
    <tr>
        <td>排除</td><td>exclude: [dir1, file1, dir2]</td><td></td><td>不需要进行转换的目录和文件列表</td>
    </tr>
    <tr>
        <td>帖子限制</td><td>limit_posts: [max_posts]</td><td>--limit_posts=[max_posts]</td><td>限制被转换与发布的帖子数量</td>
    </tr>
    </tbody>
</table>
<p></p>
**注意：** 请不要在配置文件中使用Tab制表符。这会导致解析错误，或使Jekyll意外地回退到配置的默认值。

**配置的默认值**

	safe:        false
	auto:        false
	server:      false
	server_port: 4000
	baseurl:    /
	
	source:      .
	destination: ./_site
	plugins:     ./_plugins
	
	future:      true
	lsi:         false
	pygments:    false
	markdown:    maruku
	permalink:   date
	
	maruku:
	  use_tex:    false
	  use_divs:   false
	  png_engine: blahtex
	  png_dir:    images/latex
	  png_url:    /images/latex
	
	rdiscount:
	  extensions: []
	
	kramdown:
	  auto_ids: true,
	  footnote_nr: 1
	  entity_output: as_char
	  toc_levels: 1..6
	  use_coderay: false
	  
	  coderay:
	    coderay_wrap: div
	    coderay_line_numbers: inline
	    coderay_line_numbers_start: 1
	    coderay_tab_width: 4
	    coderay_bold_every: 10
	    coderay_css: style
