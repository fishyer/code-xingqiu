2023-01-19 13:58:20 来源：[量子位](http://www.qbitai.com/)

这是教学计划的一部分。

> 詹士 发自 凹非寺
> 
> 量子位 | 公众号 QbitAI

**“从0到1手搓GPT”教程来了！**

视频1个多小时，从原理到代码都一一呈现，训练微调也涵盖在内，手把手带着你搞定。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/d9b744d3e1f34440bc2b6eb5cbd9075f~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=CzGqI0SaMNIQdPxSjH3x%2FbuRTek%3D)

该内容刚发出来，在Twitter已吸引400万关注量，HackerNews上Points也破了900。

**连马斯克也下场支持。**

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/28fb93084869419e921e535cb3529bcc~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=LyE%2FXYavBLEEfbvrPA6Q5zeqHEc%3D)

评论区更是一片锣鼓喧天鞭炮齐鸣，网友们纷纷马住。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/c9a8e1a45f9f4c2dabaa7513ba208e7d~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=%2FovrGQ8cUkK7yNBKWL7kSDiVQKY%3D)

有人评价，Andrej确实是一位出色的“事物解释者”，也热心于回答大家的问题。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/a7b62c2ed1b648159954c034a28600bf~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=hXAkVt0%2F8ovtOucD7AvHHI%2FdRb8%3D)

还有网友更夸张，称该教程简直是来“救命”。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/a0d56bfb6e2241d795ba80ac3576a2bd~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=jeiaQ9MAWpnH%2BQtcMZTLmrhTDbc%3D)

那么，这位**活菩萨**是谁？

正是前特斯拉AI总监，李飞飞高徒——**Andrej Karpathy**。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/5015e19e824746a486f5b0983544efaa~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=Zuu2md48tahCn1G50dlnyrAehyE%3D)

**教程具体说了什么？**

这就来展开讲讲。

## 从零构建GPT，总共几步？

视频教程先从理论讲起。

第一部分主要关于建立基准语言模型（二元）以及Transformer核心注意力机制，以及该机制内节点之间的信息传递，自注意力机制理论也有涉及。

该part内容长度超过1小时，不仅有概念解释，还教你如何使用矩阵乘法、添加softmax归一化，可谓“夯实基础”式讲解。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/fcfafbd586834bd1bde2711d210147a7~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=KT0nu2t3JFpgsgcrl27RiwxDTsQ%3D)

接着讲述构建Transformer。

这当中涉及了多头注意力（包括如何插入自注意力构建块）、多层感知机（MLP）、残差连接、归一化方法LayerNorm以及如何在Transformer中添加Dropout Notes…….

然后，作者会带大家训练一个模型，当中会用到一个名为nanoGPT的库，可调用GPT-2参数，快速完成GPT模型的训练。

教程中，作者还将所得模型与Open AI的GPT-3比较。两者规模差距达1万-100万倍，但神经网络是相同的。另一个将拿来比较的是人尽皆知的ChatGPT，当然，我们目前所得只是预训练模型。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/5483479a61374d65a6448652d31faaed~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=VgYhX%2FL8xsAvVsEZ2bOmg0PwefY%3D)

在上述内容引导下，我们已得一个10M参数规模的模型，在一个GPU上训练15分钟，喂给1MB大小的莎士比亚文本数据集，它就能像莎士比亚一样输出。

比如下面两张图，你能分辨哪个是真人莎士比亚写的吗？

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/4900a9f43c8a40448035829b8b8f46d8~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=XvfjuiGumhNnh8yADwPql5oeJHs%3D)

评论区有人好奇选什么GPU资源。作者也分享了下——自己用的是Lambda的云上GPU，这是他目前接触按需计费GPU中，最简单的渠道。

光说不练不行，作者还给出一些课后练习，总共四道题，包括：

-   N维张量掌握挑战；
-   在自己选择的数据集上训练GPT；
-   找一个非常大的数据集，基于它训练Transformer，然后初始化再基于莎士比亚数据集微调，看能否通过预训练获得更低的验证损失？
-   参考Transformer相关论文，看看之前研究中哪些操作能进一步提升性能；

## 神器nanoGPT也刚发布

前文提及，作者之所以能快速完成训练GPT，有赖于一个名nanoGPT的库。

这也是本教程作者前几天刚发布的利器，由2年前的minGPT升级而来，只是换了个更“标题党”的名字，自称纳米级（nano）。目前，其在GitHub所获star已超8k，网友连连点赞。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/43b00c1ed27a4f158ef8baf21a16d3e2~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=d47zD1547xy5Ky92DHqQOUoboz8%3D)

据作者介绍，该库里面包含一个约300行的GPT模型定义（文件名：model.py），可以从OpenAI加载GPT-2权重。

还有一个训练模型PyTorch样板（文件名：train.py），同样也是300多行。

对想上手的AI玩家来说，无论是从头开始训练新模型，还是基于预训练进行微调（目前可用的最大模型为1.3B参数的GPT-2），各路需求均能满足。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/2122daf6fdf243559fd378101da83ffe~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=pYfkYhfpegI2j1LT2Fl7LoCyjis%3D)

△ 一个训练实例展示

据作者目前自己的测试，他在1 个 A100 40GB GPU 上训练一晚，损失约为 3.74。如果是在4个GPU上训练损失约为3.60。

如果在8个A100 40GB节点上进行约50万次迭代，时长约为1天，atim的训练降至约3.1，init随机概率是10.82，已将结果带到了baseline范围。

![](https://p3-sign.toutiaoimg.com/tos-cn-i-qvj2lq49k0/4ea86bbeb99d4f37b31b2551bb5333c1~tplv-tt-shrink:640:0.image?traceid=20230119135329FD99CCF9E771E551708C&x-expires=2147483647&x-signature=H2O9ZKHBtOn2QM9HTlyNLg4tyBI%3D)

对macbook或一些“力量”不足的小破本，靠nanoGPT也能开训。

不过，作者建议使用莎士比亚（shakespeare）数据集，该数据集前文已提及，大小约1MB，然后在一个很小的网络上运行。

据他自己亲身示范，创建了一个小得多的Transformer（4层，4个head，64嵌入大小），在作者自己的苹果AIR M1本上，每次迭代大约需要400毫秒。

（GitHub上nanoGPT链接附在文末，有需要的朋友自取）

## One More Thing

此番教程作者Karpathy Andrej在圈内早已颇有名气，他在斯坦福时，师从华人AI大牛李飞飞，后又曾工作于Open AI。

此前，Karpathy就在致力于让更多人接触了解神经网络和相关数据集。2020年8月，他就曾发布nanoGPT前一代，MinGPT，同样旨在让GPT做到小巧、简洁、可解释，同样主打——300行代码解决问题。

Karpathy另一个身份是前特斯拉AI核心人物。

在马斯克麾下，他历任特斯拉高级AI主管、特斯拉自动驾驶AutoPilot负责人、特斯拉超算Dojo负责人、特斯拉擎天柱人形机器人负责人…

2022年7月，Karpathy Andrej离职，在业内引发不小讨论。当时他就表示，未来将花更多时间在AI、开源技术教育上。

**这回发布的从0开始构建GPT课程，正是他教学计划的一部分。**

-   课程视频：

https://www.youtube.com/watch?v=kCc8FmEb1nY

-   nanoGPT GitHub链接：

https://github.com/karpathy/nanoGPT

参考链接：  
\[1\]https://twitter.com/karpathy/status/1615398117683388417?s=46&t=69hVy8CNcEBXBYmQHXhdxA  
\[2\]https://news.ycombinator.com/item?id=34414716

— 完 —

量子位 QbitAI · 头条号签约

关注我们，第一时间获知前沿科技动态

_版权所有，未经授权不得以任何形式转载及使用，违者必究。_