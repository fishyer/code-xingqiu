**You:**

欧路词典Mac版支持哪些Url Schema

* * *

**ChatGPT:**

欧路词典 Mac 版本支持以下 URL Scheme：

1.  `eudic://`：欧路词典通用 URL Scheme。
2.  `eudic://openDict?dictType=*&query=*`：打开指定类型的词典并查找单词，其中 `*` 可替换为相应的参数，如 `dictType=ENDIC&query=test`。
3.  `eudic://search?query=*`：搜索词典中的单词，其中 `*` 可替换为要搜索的单词。
4.  `eudic://lookup?word=*`：在默认词典中查找指定单词，其中 `*` 可替换为要查找的单词。
5.  `eudic://openDictList`：打开词典列表。
6.  `eudic://openSettings`：打开欧路词典的设置界面。

以上是欧路词典 Mac 版本支持的常用 URL Scheme，具体使用方式可以在相应应用程序中查看相关文档或帮助。

* * *

**You:**

帮我写一个翻译test单词的url Schema

* * *

**ChatGPT:**

好的，以下是一个翻译 "test" 单词的 URL Scheme：

`eudic://openDict?dictType=ENDIC&query=test`

解释一下各个部分的含义：

*   `eudic://`：欧路词典的通用 URL Scheme。
*   `openDict`：指示打开指定类型的词典并进行查询。
*   `dictType=ENDIC`：指定要打开的词典类型为英英词典（英汉词典为 `CEDICT`）。
*   `query=test`：指定要查询的单词为 "test"。

将这个 URL Scheme 复制到浏览器或其他支持打开 URL Scheme 的应用程序中打开，就可以在欧路词典中查询 "test" 的翻译了。

* * *

**You:**

改成英汉词典

* * *

**ChatGPT:**

好的，以下是一个在英汉词典中翻译 "test" 单词的 URL Scheme：

`eudic://openDict?dictType=CEDICT&query=test`

与之前的 URL Scheme 相比，主要变化是将 `dictType` 参数改为了 `CEDICT`，表示要打开英汉词典。其余部分保持不变。

将这个 URL Scheme 复制到浏览器或其他支持打开 URL Scheme 的应用程序中打开，就可以在欧路词典中查询 "test" 的英文翻译了。