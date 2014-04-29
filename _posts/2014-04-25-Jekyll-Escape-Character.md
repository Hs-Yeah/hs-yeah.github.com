---
layout: post
title: Jekyll 花括号百分号转义输出
categories : [Jekyll]
tag: [Jekyll]
---

Markdown下{% raw %}`{% 内容 %}`{% endraw %}、{% raw %}`{{ 内容 }}`{% endraw %}都可以正常输出的，但是Jekyll下编译会失败，生成不了页面，`jekyll serve`也会启动失败

####解决方法：
需要转义的地方使用


	{ % raw % }需要原样输出的内容{ % endraw % }

注意：以上代码中花括号需要跟百分号紧贴在一起，否则不生效，这里不贴在一起是因为`{ % raw % }`对自身不起作用╮(￣▽￣")╭
	
这样就可以原样输出了。