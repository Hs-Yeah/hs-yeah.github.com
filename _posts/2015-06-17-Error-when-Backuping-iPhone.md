---
layout: post
title: iTunes 备份 iPhone 提示“无法备份 iPhone，发生了一个错误”的解决过程
categories : [日常]
tag: [iTunes,iOS]
---
进行备份的时候打开 OS X 的 Console.app，看到

	AMDeviceSecureArchiveApplication (thread XXXX): Could not archive application on device: kAMDAPIInternalError

应该是因为 iOS 无法 archive 某个 App.

向万能的 Google 求救，在 [Apple Support Communities](https://discussions.apple.com) 上面找到一个帖子： [sync failing unknown error](https://discussions.apple.com/message/28374480?tstart=0#28374480)，里面的用户回复提到原因可能是某个 App 没卸载干净。

进入 iOS 的设置→通用→用量→管理存储空间，里面果然有一个没有名字的 App，版本号 30.0，而且删除了还是会出现。

通过查询 iTunes 的 App 库，找到版本号最接近的 App 是 Facebook.app，这个 App 在前不久被我删掉了。（可见平时随手同步已购买的 App 有多重要）

于是在手机上面重新下载安装 Facebook.app，卡在下载完成，但是没有开始安装（表示进度的圆圈已经闭合）。然后点击圆圈，取消安装，可是 Springboard（主屏幕）上面还是有一个显示为“正在载入…”的 App 图标，长按该图标，点击左上方叉号，删除，没反应。最后只好进入设置的管理存储空间那里删掉。

然后换用 iTunes 下载好 App，通过 USB 线同步安装，可貌似还是卡在“正在载入…”，不过这次直接长按删除就消失了，管理存储空间里也没看到有，不过看到 iOS 的状态栏的同步标志一直在转，貌似是这个安装的操作卡住了？重启手机先。

重启之后进入设置管理存储空间，发现这个 Facebook.app 的残留真的没有了。

再来试试使用 iTunes 备份，这次不提示有一些没有购买的应用程序需要同步了，而且 Console.app 也没有看到有相关的错误日志产生了。

然后就备份成功了。
