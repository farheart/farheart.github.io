---
layout: post
title: git的学习总结
category: Tech
tags: git
keywords: git
published: true
---

## 缘起

git大名，如雷贯耳，闻之已久。但直到最近才有机会来仔细了解它原理及用法，感觉很开眼界：作为融合了图论思想的版本管理系统，git的设计确是精妙。长久以来，很少有工具能让我觉得如此迷惑，却仍有动力来一篇接一篇的找教程、博客、视频来看的。在git这里，算是大大的例外了。

## 学习材料

盘点一下学习中所看过的材料：

### 文字型

* [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html):   
  第一篇在google上抓住我眼球的帖子。号称

  > 助你入门 git 的简明指南，木有高深内容

	so nice! 简直是我这样的懒人最爱嘛。可惜的是，我看过之后还是云里雾里，感觉似是而非。直到在感觉真正入门后我才明白：这份指南，是给懂得git的人看的总结性材料！（想靠它入门...呵呵。也许你可以，但是我不行 ... ）

* [Git学习笔记](http://shanewfx.github.io/blog/2012/04/21/learn-git-command/)：  
  这是我的入门启蒙。很好地介绍了git的概念，以及它的基本工作流程，并且还给出很不错的外部参考读物。

* [Interactive GIT tutorial portal](http://gitimmersion.com/lab_01.html)：  
  交互式学习git的站点，没有全部看完，但是那种循序渐进的学习感觉很好，推荐。

* [Git Community Book 中文版](http://gitbook.liuhui998.com/index.html):   
  还没看完，但是前半部分有关git的原理部分写的非常清晰，赞。
  
* [廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)：  
  又一个很不错的入门材料，看这个，是能够真正起到入门启蒙作用的。稍长，所以没有全部看完。当时吸引了我注意的是“[搭建Git服务器](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)”一节：说不定哪天心血来潮，打算自己搭建git服务器的时候可能会有用。

* [A succeddful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)：  
  git毕竟是工具，最终是要拿来使用的，这一篇文章给出了很好的工作流程模型。都说git灵活，那么这篇文章告诉了我们应该怎样善用这种灵活，而不是让一堆堆的git指令 `init，add，clone，checkout，merge，reset，pull` 和 `push` -- 把我们绕晕。毕竟，灵活（或强大）<> 高质量，没有一个好的工作流程，灵活估计更容易用来制造混乱 ...

* [Think Like (a) Git](http://think-like-a-git.net/epic.html#testing-out-merges):   
  这一篇点出了git中的图论思想。作者的幽默风格我很喜欢。特别是这一条
![EXAMPLE 3: HOMEOMORPHIC ENDOFUNCTORS](http://think-like-a-git.net/assets/images2/homeomorphic_endofunctors.jpg)

	此外，我也喜欢它对git命令的分类：
		- local：`commit, merge, rebase, reset`, ..., 
		- remote: `fetch, push, pull`, ...
	
	美中不足的一点是，我觉得它没有说清楚 cherry pick 与 rebase 的区别。

* [图解Git](http://marklodato.github.io/visual-git-guide/index-zh-cn.html): 不长，也不是入门材料。但是正如其页面上所说的那样

  > 如果你稍微理解git的工作原理，这篇文章能够让你理解的更透彻。

  对于git常用命令的概念解释非常清晰，很好。
  
### 视频类

* [[Linux.conf.au 2013] - Git For Ages 4 And Up](https://www.youtube.com/watch?v=1ffBJ4sVUb4&t=4217s)：  
  估计4岁小孩还是看起来有点吃力的 :smile，不过视频里面，用玩具进行的git流程类比非常准确，另外，它暗示出了很重要的一点：所谓`branch`，也只是一个`pointer`，指向了一个特定的commit对象，只不过，这个commit对象是处于一个特定的**“分支”**上的。理解这一点，其实很重要，因为在我的潜意识里面，总是会把 `branch` 当做一个**“链条”** 来看待，从而对一些git指令就觉得不好理解了。（目前没想到例子，留待日后想到时补上，`# TODO`）

* [Tech Talk: Linus Torvalds on git](https://www.youtube.com/watch?v=4XpnKHJAok8)：  
  还没看。这是git之父的talk，应该很有价值。
