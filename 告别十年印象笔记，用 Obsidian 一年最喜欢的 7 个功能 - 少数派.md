**Matrix 首页推荐**  

[Matrix](https://sspai.com/matrix)  是少数派的写作社区，我们主张分享真实的产品体验，有实用价值的经验与思考。我们会不定期挑选 Matrix 最优质的文章，展示来自用户的最真实的体验和观点。   
文章代表作者个人观点，少数派仅对标题和排版略作修改。

___

我很少如此喜欢一款软件。

在此之前，也从未想过软件的力量可以如此强大。无论是知识管理还是工作生活，都因为它的加入而变得不一样。它就是用了一年的本地的免费的笔记管理软件 Obsidian。

运营摄影视频自媒体《橙猫涉影》，需要写稿、修图、剪视频，刚开始在做这些事的时候，还是嵌入式开发工程师，同时也能用 C#、Android、Python、js 写出辅助工具优化工作流的水平。而这一切的记录管理，包括后来去讲脱口秀的稿件，都依赖着老牌著名笔记软件：印象笔记。今年已是 10 年老用户了。

![我的印象笔记](https://cdn.sspai.com/2023/01/16/article/eafc08d708cbc6a82f3316bcef8e6eec?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

所有的事情加在一起，数量越来越多，复杂度也越来越高，以至于无法回顾总结等等，甚至别的事情都没有精力和时间去处理。早在 2 年前印象笔记满足不了需求，但又没意识到「人是会被软件卡脖子的」，毕竟常用的 Keil、IAR、Ps、Lr 都是几十年如一日。2021 年底，阴差阳错接触到笔记软件，了解到有更新换代的说法，马上把推荐的 Obsidian、Notion、语雀、logseq、wolai 等，都用了一遍。最早偏爱 Notion，因网络原因放弃而转向本地的笔记软件，选择了热度最高的 Obsidian。随着深入使用，发现这次是遇到了真爱。第一爱好编程，第二爱好摄影，Obsidian 居然能让它们结合起来，一款能编程自动化、带插件系统、全平台免费的笔记软件！

比起别的软件打开就能上手，打开 Obsidian，让人无从下手，菜单都找不到。Slogan 是「A second brain, for you, forever」，看上去却是一张白纸。

接下来的分享，不是基础向，但会给出相关链接学习，毕竟我也是从这些老师身上学过来的。

## 从日记功能开始

**相关功能：核心插件-日历、Calendar、Templater**

Obsidian 的强大离不开它的插件系统，我的每个用途都离不开。一开始的学习是从自带的日记功能开始上手，过渡起来相对比较简单。随着使用的深入慢慢往里面加自己想要的东西，需求在使用中诞生。当自带功能满足不了，于是便跟着教学安装了插件。

-   Templater：模板文件，比自带的功能更多，支持 JavaScript
-   Calendar：日历视图，直观查看日记

![最早日记与现在日记](https://cdn.sspai.com/2023/01/16/article/637c540f6418ec56dc02b6abe0e83b45?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1)

最早是记录了当天完成了什么事情，附上一张自己的摄影作品，没太多想法。从富文本到 md 源格式，这个探索过程就像 《[2022 年，在记录中感受混乱与秩序之美](https://sspai.com/post/77642)》提到的。

随着工具的学习深入，对 Obsidian 和 MD 越来越熟悉，学会了增加 Heading 标题，既能分类，又能集中记录在日记中。用到的功能涉及核心插件-日历、第 3 方插件 Calendar、Templater。

### 每日语录

一开始是来一些名言警句，总觉得少了点意思，然后演变成从读过的书里随机一本，再找出一句有意义的话作为语录，温故知新，形成闭环。解决了我太久不回顾而忘记的知识，以一种舒服又轻松的方式，没有增加任何的压力。同时加入了随机图片的展示，让自己的摄影作品重新活了过来，它们很多时候都是躺在硬盘里，现在又多了一个价值。

### 天气

看天气是摄影做规划的一个环节，也是每个日子都有的属性。将来再看的时候，还能知道当时的天气。也顺带为此学了 JavaScript，学习修改调用 API 显示天气，后来学了正则表达式折返回来优化过。编程技能有了用武之地。

一开始用简单实用的方法：来自《[我的](https://sspai.com/link?target=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F441013488) Obsidian [入门之旅](https://sspai.com/link?target=https%3A%2F%2Fzhuanlan.zhihu.com%2Fp%2F441013488)》，在 Templater 的相关介绍章节中。

当我想要更多天气信息时，学了需要编程的方法：《Obsidian [使用技巧：为你的笔记添加一个天气视图吧](https://www.bilibili.com/video/BV1Sb4y1p7Un)》。

### Todo

主要记录当天做了什么或者要去做的事情，分为工作（隐私原因已移除）、摄影、生活、记账，都是在过去 1 年的使用中慢慢演变而来。这一块感触很深，看别人分享的分类都好像是一件很容易很简单的事情。但从一张白纸开始，思考起来混乱不知所措。当梳理完成的时候，又有一种「真的掌握了」的成就感。

### 日记方法

在学习了这位大佬的基础上进行的调整：[玩转](https://sspai.com/post/69982) Obsidian [07 ：自动化「间歇式日记」](https://sspai.com/post/69982)

我没有研究区分自带和插件的之间会有什么影响，就把设置都放出来。我的日记命名格式为「2023-01-11：星期三」，在日记插件对应设置：  
 

![日记设置](https://cdn.sspai.com/2023/01/16/article/85504a7928b46dc1d1091eb256374a8c?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

利用 Templater 的编程模板，写了随机读取图片（图床格式）功能、随机获取已阅书籍、天气信息，「日记模板.md」[完整内容](https://sspai.com/link?target=https%3A%2F%2Fflowus.cn%2Fcmsy%2Fshare%2F5e4b2ce0-6423-48af-9367-64990cb29a43)。

## 记账功能

**相关功能：Dataview，Admonition**

刚毕业那几年，好奇自己一年的开销是多少，开始去记账。每一次手动录入记账软件，3 年之后发现每年差不多，就停了下来，毕竟也不是一件很轻松的事情。后来有了自己的自媒体频道，要区分钱哪些用在了《橙猫涉影》上，哪些是个人开销，记账变成了一件「必须做」的事情。换笔记软件的过渡期是在 Notion 上记录，把每个月把微信记账本和支付宝记帐本一条一条手动输入，有种「零存整取」的感觉。每个月都要花费半天时间整理，心里实在是不想花时间去做这件事，产生了习惯性拖延。

> 「想做」和「必须做」，这两者对于我们处理事情造成很大的差异。
> 
> ——《怎样说话才打动人》

我必须要做调整，把「必须做转化成想做」。探索了很长时间，到底用新的记账软件还是嵌入表格，还是回归 excel 不追求 all in one。最终选择了过程最复杂结果最简单的方式，插件 Dataview 的 js 功能。既能实现需求，同时满足编程实现自动化，这是我一直都在做事，也借此机会深入学习了 Ob 自身的 api 和 js。

把记账功能放在 Ob 日记里，在我想起的时候/发呆的时候/不知道要干嘛的时候，写上开销，「用琐碎的时间做琐碎的事」。比起零存整取，这是零存自动取。当然记账软件都能做到。但因为 Ob 一直打开的缘故，这里的录入没有零存的烦躁，没有「刻意」的操作，过程非常丝滑。每次记账都减少了打开和关闭 App 的一系列操作，我的感觉就是，「每少一个步骤，执行力上一个等级」。这种方式让我一直坚持，丰富了all in one。这次用到了 2 款插件：Dataview 和 Admonition。

-   Dataview：根据 yaml 格式化生成索引，表格展示之类
-   Admonition：美化显示

### 记账方法

我是支付宝和微信支付混用，微信记账本和支付宝记账本都开通了（免费的，貌似不开通只保留几个月），做记忆的保底。在日记中开辟一个 heading 作为记账功能。每一笔支出以列表的方式记录，在最前面以「cmsy」来划分 2 个大类别，我只需要不同类别的支出总数额，不对每一项细分。对应代码编写起来就相对简单。如果有大神帮忙优化一下更好，毕竟是入门水平。

日记中的记账样式，采用无序列表，「:」或「：」后面必须接对应的消费金额，如下图：  
 

![400](https://cdn.sspai.com/2023/01/16/article/80a04d8872913c14ed66af6a110d19f5?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

Dataview 再搭配插件 Admonition 的美化效果，让界面看起来更舒服。  
「记账.md」中对 2023 年 1 月的记账 Admonition+Dataview 的 js [完整内容查看](https://sspai.com/link?target=https%3A%2F%2Fflowus.cn%2Fcmsy%2Fshare%2F5e4b2ce0-6423-48af-9367-64990cb29a43)，最终效果如下：  
 

![image.png](https://cdn.sspai.com/2023/01/16/article/95e798aecaa502ef838e78f31456ea4c?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

每月初，只需要核对微信记账、支付宝记账和 Obsidian 中的金额是否能匹配，时间大幅度削减。功能如果过于自动化，写了就会等于没写，不定期看，会助长惰性，时间一久都忘了写的啥。不做成全自动化，是因为这需要一些仪式感，真的不是懒。

## Card View

**相关功能：Templater, Dataview, 自带功能 Callout, YAML 格式**

Notion 我最喜欢的功能是 Gallery view，为我的照片又提供了一个新用途，可视化的记录查看起来非常直观方便，也很适合我们做视频的展示素材。

![Notion Gallery View](https://cdn.sspai.com/2023/01/16/article/cf6785b0871c9c46755bb8d79f1bc760?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

在 Obsidian 中同样也有相似功能，依赖主题搭配 CSS 去实现。方法参考：《[由豆瓣建立Minimal样式主题的阅读书单卡片](https://www.bilibili.com/video/BV15b4y147jz)》。

![使用前](https://cdn.sspai.com/2023/01/16/article/c0ce376b4366d7b1f8f504377f9fee6b?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

使用 Dataview 搭配 YAML，可以呈现出表格效果。我用的是 minimal 主题，在 md 文档的 YAML 加入「cssClasses: cards」（需要主题支持）。Dataview 的表格样式就会以卡片的形式显示。

![使用后](https://cdn.sspai.com/2023/01/16/article/4b712bf82160c4652a4c6f6a31f6ea61?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

其对应的完整 md 文档内容如下：

```
---
cssClasses: cards
---
> [!tldr]- 摄影日志
> ```dataview
> table
> ("![](" + cover + ")") as 封面, 
> "主题："+ theme as theme, 
> "模特："+ model as model,
> "地点："+ place as place
> from "001-CMSY/004-摄影日志"
> sort date desc
> ```
```

这个功能是我使用频率最高的，看过的书、组织的拍摄、产出的视频、衣柜衣服、模特档案、摄影器材库等等，有收集水浒卡的感觉。我一直坚持记录了 2022 年的每一场拍摄，最大的感受就是：「我的记忆不需要记忆了」。

![2022阅读书单](https://cdn.sspai.com/2023/01/16/article/7170f8745d38e41bd7364daa3e747300?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

这是 2022 看过的书，我设定的目标是每月一本。对于看书这个问题，我的笔记是偏少的，但又有很多重要的知识点需要记下来，就会配合 Chrome 插件**微信读书笔记助手**，辅助获取热门标注作为更详细的笔记。回顾的时候，就能轻松的过一遍整本书的知识点，结合每日语录实现温故知新。

![2022制作的频道视频](https://cdn.sspai.com/2023/01/16/article/38d39f1f18bbfad0ee402a8ca9d05db0?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

对于制作的视频，第一次有种我的东西还是属于我的舒适感。早在 Obsidian 之前，我的视频是属于视频网站，并非我的。借助 Ob 的可视化，利用爬虫爬了自己过去 200 多个视频，整理成了笔记。以后再需要什么视频相关信息的时候，在笔记中搜索即可。

衣柜衣服太多，挑起来很费劲。我把它们全放到 Obsidian 里，非常直观。

![image.png](https://cdn.sspai.com/2023/01/16/article/c1ad126c62b032c3121003d878ea105b?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

目前还有很多类似的 Card View 在收集的过程中，只要量上来了，这个功能强推，减轻大脑负担！

## 地图功能，进一步可视化

**相关功能：Obsidian-Leaflet**

我不是热爱旅游的人，但在一个城市，我会好奇自己去过的地方都有哪些，拍摄地点上重复选择的频率是多少？

拍摄有目的地，朋友/模特有住址，组织活动也要看朋友之间的距离好安排司机，这些基本上都依赖脑海中记忆和对本地的熟悉，就很希望有一个工具，能像手机照片的地图功能那样，有一张地图直观展示地点。要是Obsidian也有这个功能那该多好。我的想法，一定也是别人的想法，一定有人有这个想法的同时有能力实现它，并且做成了插件。于是就在Obsidian的插件市场一个一个的找，还真的找到了。

-   Obsidian Leaflet：根据md文档中的yaml的location属性，在地图中标注出来。

根据文件路径标注出来：

````
```leaflet
id: leaflet-map
lat: 23.125463267494546
long: 113.29981259960813
minZoom: 5
maxZoom: 17
defaultZoom: 12
unit: meters
scale: 1
markerFolder: 001-CMSY/005-摄影资料/模特档案
```
````

在设置里，可以根据 Tag 标签指定相关图标：  
 

![image.png](https://cdn.sspai.com/2023/01/16/article/b2dba96d10f59bfa028a902f0c336970?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

对应的笔记格式：

```
---
model: 模特A
wechat: 
phone: 
place: 鹭江
location: [23.096685,113.311895]
tags: 摄影/模特
---
```

Location 属性搭配[高德的坐标拾取系统](https://sspai.com/link?target=https%3A%2F%2Flbs.amap.com%2Ftools%2Fpicker)（需注册账号）使用，标出了过去 5 年 200 多个视频拍摄地点，只能说，真的很累。

![image.png](https://cdn.sspai.com/2023/01/16/article/57a6245e74e5915371125806010fc399?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

## 全平台同步，文件管理

**用到功能：Remotely Save，坚果云，阿里云**

很坚定用 Obsidian，其中有一个很重要的功能，插件 Remotely Save 实现全平台同步。无论搭配阿里云腾讯云还是坚果云，它的相关教程很多。

公司电脑 Win，自用黑苹果+Win 双系统，有 Mac Pro，手机也有 iOS 和 Android，现在跨平台的软件都没有办法满足了，追求全平台。最早的时候使用坚果云同步，但在安卓上是需要用 FolderSync 来同步，每次同步都很耗时间，非常影响在 Android 上的使用频率。

后来接触到 Remotely Save 插件，配合阿里云搭建同步系统，真正做到了全平台。虽然它的同步也需要等，但起码是在 Ob 里面等，可以浏览别的文档，并不需要额外打开 App。

同步功能我的用法是 Remotely Save 全平台同步，电脑额外配置坚果云备份，操作失误起码有保底。实际使用过程中确实出现过很多次误删，过了很多天才发现的情况。非常建议大家注册阿里云，除了同步文档使用，还可以作为图片的图床。我的图片都是以图床的方式存在，我希望笔记库给我的感觉都是轻量的轻盈的。

同步与图床使用大半年，大概费用也就一碗猪脚饭。

![阿里云费用](https://cdn.sspai.com/2023/01/16/article/81bb7a2fedabfded6e27b8b5fe6ca091?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

8 月的费用变高，是因为买了个域名做网站。

## 衍生品：个人网站

**相关功能：Hexo**

看着自己打造的各种功能，它们组合起来就好像一个网站，于是就有了一个想法：是不是有网站架构能支持 MD 格式？

这想法一出，又是折腾。

Hexo 这个网站架构非常完美匹配我的需求，把本地笔记变成一个网站。学习 Hexo 也基本是半天的事情，因为个人的需求，对它主题进行了大量修改，前前后后就折腾了一个半月左右。

自称[最卷摄影师网站](https://sspai.com/link?target=https%3A%2F%2Fwww.cmsy.fun%2F)，因为一般摄影没有自己网站，也没啥用。对我来说也只是个笔记的衍生品，把 Obsidian 的功能搬上了服务器而已。

![](https://cdn.sspai.com/2023/01/16/d20b8bdab7a8f59343a241c23a1790b8.jpg?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

网站首页

## 写作障碍，开始做减法

目前来看，Obsidian 似乎非常完美适合自己，也学的足够多了。一切都在变好，但却面临着最严重的的障碍：「我无法在 Obsidian 写稿。」

功能强大，各种花里胡哨的功能，界面各种拼接全屏，什么酷炫样式都有，像极了数据大屏。

-   Templater：「我这里可以做模板，编程多好玩。」
-   Dataview：「不给你的代码来点优化，展示更多有趣内容吗？」
-   Ob插件库：「快去看看有什么更新的好玩的东西。」
-   主题：「不来修改一下，让我看上去更舒服更有设计感吗？」

一堆诱惑向你招手：客官过来玩呀。根本无法在这样的笔记中静下心来写作。直到写这篇文章的时，我才找到原因：「我需要的只是一张稿纸。」

开始对各种插件做减法，从 50 多减到 20 多，取消笔记全屏，取消 Stack Tab 的花哨模式，取消 workplace 的切换，隐藏树状文件管理，笔记字体标题回归黑色，学习了油管的一个博主，在文件中都加入上下层的超链接，开启了类网页超链接的模式。使用 Homepage 做主页，点击完成分类跳转。

![image.png](https://cdn.sspai.com/2023/01/16/article/b3638201188409ef2e812c97935f6788?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

每个文件都加入了层次跳转，就像网页的前进后退，非常方便。不需要再占有整个屏幕，随意缩放。

![image.png](https://cdn.sspai.com/2023/01/16/article/35529f2ea1b11b750414fec8d31f0a75?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

管理的时候，它是一个窗口。写作的时候，它就是一张白纸，这篇长文就是诞生于 Obsidian。我想起了那句「重器轻用」。它没有变轻，但是它却给了我很轻巧很轻盈的感觉，而这个感觉至关重要。「重」是功能，「轻」是感觉。

## 开启新世界：白板 Canvas 与 Excalidraw

最早时候我是没能理解，为什么 Excalidraw 有着很高的下载量。

![](https://cdn.sspai.com/2023/01/16/a8ae960a192e3b9baf22367ef923d3a6.png?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

后来我的频道内容需要调整方向，工作流也要改变。用卡片的方式设计短剧情，有的可能只有一句话，最长的就一段话，有的是影视视频的剪辑。平常积累好了，当我去回顾的时候，要一个个去点击查看，很局限，看不到全局，没了「大局观」，做不到合理挑选。这时候我就想到了 Excalidraw。

插件 Excalidraw 完美解决了这个问题。每一个卡片都是本地 MD 文件，全平台免费，夫复何求！

![image.png](https://cdn.sspai.com/2023/01/16/article/d6623129cdc13a0ae9f1e2251baabd77?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

后来官方推出了白板功能 Canvas，完结撒花！Excalidraw 与白板，有功能重叠，也有各自优势。目前我一些脚本的设计、摄影课程的设计，都用上了白板。毕竟是自家人，兼容插件，可直接观看视频。

![image.png](https://cdn.sspai.com/2023/01/16/article/0cf7f07a5571b3cd391556dfb5435444?imageView2/2/w/1120/q/40/interlace/1/ignore-error/1)

白板的瑕疵是手机端无法显示，但不影响 Obsidian 在我心目中的完美程度！试过很多白板工具，包括 Heptabase、氢图、Colube、无边记等等，如果你的需求像我一样简单，而 Obsidian 对你来说又很复杂，那至少试试它的白板，支持 MD、本地、免费。至今也无法忘记，在到处搜索查找能结合 Obsidian 的白板时，突然看到内测白板工具那种激动！尤其是它上线的当天，我就阳了！

## 总结

2022 为一款笔记去折腾去写代码，比起纯粹的笔记，过程很繁琐，使用却很简单，「至繁归于至简」。把记忆提取出来成为笔记，成为你的 second brain，让 first brain 更多用于创作。我是鼓励大家去折腾的，Obsidian 不止 for you 还 for free，折腾也是学习的一个过程，「任何事情过了度，才能找到合适自己的度」。

对我来说，2022 已经折腾完毕，2023 要更多投入视频创作了。喜欢摄影的，可以关注一波我的频道：[橙猫涉影](https://space.bilibili.com/11455098)。

\> 下载 [少数派 2.0 客户端](https://sspai.com/page/client)、关注 [少数派公众号](https://sspai.com/s/J71e)，解锁全新阅读体验 📰

\> 实用、好用的 [正版软件](https://sspai.com/mall)，少数派为你呈现 🚀