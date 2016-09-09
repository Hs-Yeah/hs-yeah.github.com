---
layout: post
title: Jekyll 提示 Liquid Exception：highlight tag was never closed 的解决办法
categories : [Jekyll]
tag: [Jekyll, Pygments, Highlight]
---

在使用 Pygments 的时候，在{% raw %}
`{% highlight shortname %}`
{% endraw %}跟{% raw %}`{% endhighlight %}`{% endraw %}之间插入了代码之后，

在终端输入`jekyll serve`之后，提示

	Liquid Exception: highlight tag was never closed
	
这是 Jekyll 的一个 Bug ，在 Bug 解决之前，可以在 `_config.yml` 内加上语句：

	excerpt_separator: ""
	
然后就可以顺利使用Jekyll生成页面了。
