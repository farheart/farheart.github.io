---
layout: post
title: 在 GitHub 上建立 Blog 的历程
category: GitHub
tags: GitHub  Blog
keywords: GitHub  Blog
published: true
---

知道了git，很自然的就知道了GitHub。知乎上对GitHub介绍的[高票答案](https://www.zhihu.com/question/28976652)竟然是

> Github 是目前全球最大的男性同性交友平台. 

也真是让我无语了 ...

闲话不提，书归正传。Google `github 上 建立 blog` 返回的前列结果是 `OctoPress`。进一步查看，需要装`Ruby`, `DevKit`, `Jekyll` ... 

年纪渐长，懒得折腾。有没有简单方法？

一来二去，发现了这个网站 [Build A Blog With Jekyll And GitHub Pages ](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)。 看上去步骤似乎不复杂，可以一试

1. Fork [Jekyll Now](http://www.github.com/barryclark/jekyll-now)
2. Change the repo name from ***Jekyll Now*** to < YourUsername >.github.io
3. Now you already can preview the blog by browsing < YourUsername >.github.io
4. Customize -- change the default contents
	* `_config.yml`
    * `about.md`
    * `/_posts/2014-3-3-Hello-World.md`
	
甚是容易，5分钟成功。

很快的，我又发现了[suyan](https://github.com/suyan) 的blog，觉得这风格很对胃口。人心喜新厌旧，见异思迁是难免的，fork之。一窍通则百窍通，这一次轻车熟路，很快进入到了**定制**步骤，改完`_config.yml`，刷新一下看看 ... 404 Not Found ？！ ...

折腾了半小时之后，我无奈的认栽：偷懒是不成的。装个Jekyll测试环境吧 。。。

我的系统是win8，按照了如下两个帖子试图安装 Jekyll:

* [Windows下OctoPress环境搭建](http://www.yebangyu.org/blog/2015/10/17/howtoinstalloctopress/)
* [在 Windows7 下从头开始安装部署 Octopress](http://alfred-sun.github.io/blog/2014/11/23/install-and-deploy-octopress-to-github-on-windows7-from-scratch/)

结果是失败： 卡在了`gem install bundler` 一步。即使按照帖子所示：

> 把 http://rubygems.org/ 改为https://ruby.taobao.org/

也没有任何luck。

无奈，启动VBOX，进入Ubuntu 14.04，参考这篇 [Install Jekyll 2 on Ubuntu 14.04](http://michaelchelen.net/81fa/install-jekyll-2-ubuntu-14-04/) 从头再来。这一回还是相对顺利的（微软啊微软...），只是在ruby版本上稍有挫折，不过最后还是成功地启动了 `jekyll serve` ...

嗯，`_config.yml` 编译错误，第5行，改正之。。。呵呵，天不负我，http://localhost:4000 出现页面了

（后来发现，其实测试环境也并非必要：在`GitHub` 上 Repository 的 `Settings` 页面中， `GitHub Pages` 一节的开始部分是会给出编译出错的提示的（我就说嘛...）。不仅如此，GitHub 也会往 Email 发送出错通知的 ......）

后面还有一些曲折，不一一详述了，总结几点所学到的经验吧：

1. 所发布的站点必须位于< YourUsername >.github.io 的**Master branch**下 （我在一次push中，莫名其妙地建立了一个新的`branch`，而后就是长久地对着 404 Page 百思不得其解 ...）

2. 如果采用了 `GitHub` 的 `Enforce HTTPS` 建议，那么页面中所用到了一切资源 (CSS, JavaScript, ...) 都必须以 HTTPS 方式访问，否则浏览器只会静默的停止加载那部分内容。我是在弄 disqus 评论功能的时候发现了这一点（感谢 Chrome 的 `CTRL` + `SHIFT` + `I`）

3. 如前提及，在 < YourUsername >.github.io 的 `Settings` 页面中， `GitHub Pages` 部分会有Jekyll的编译成败提示
	
	
附：	
一个挺好使的 Online GitHub file editor: [Prose](http://prose.io/#farheart/farheart.github.io/tree/master)