**You:**

\## 0308-01 我想要用postman通过telegram api给机器人发一条消息，我的curl代码是这样的： curl --location --request POST 'https://api.telegram.org/bot5328798300:AAEsGh2NRytXNaUPTYDWetB2jlhybdOTHXs/sendMessage' \\ --header 'Content-Type: application/x-www-form-urlencoded' \\ --data-urlencode 'chat\_id=-1001658271641' \\ --data-urlencode 'parse\_mode=MarkdownV2' \\ --data-urlencode 'text=\[KotlinFlow\_一眼看全\_Flow操作符大全\_掘金\]( https://juejin.cn/post/6989536876096913439 )' 它的返回结果是这样的： { "ok": false, "error\_code": 400, "description": "Bad Request: can't parse entities: Character '\]' is reserved and must be escaped with the preceding '\\\\'" } 请问该怎么解决它的返回异常？

* * *

**ChatGPT:**

根据API的错误信息 "Bad Request: can't parse entities: Character '\]' is reserved and must be escaped with the preceding '\\'"，可以看出问题是由于发送的文本中包含了预留字符 '\]' 导致的。

解决方法是在文本中对预留字符进行转义，将 '\]' 转义为 '\]'，可以在发送的文本中使用反斜杠来转义它，例如：

```less
[KotlinFlow_一眼看全_Flow操作符大全_掘金]( https://juejin.cn/post/6989536876096913439 \)
```

在上面的文本中，我们在预留字符 '\]' 的前面添加了反斜杠 ''，这样就避免了API的错误提示了。

在Postman中，您可以直接复制上面的文本，然后将它作为 'text' 参数的值发送给Telegram API。如果您想直接在代码中处理，可以在文本中添加反斜杠，例如：

```scss
--data-urlencode 'text=[KotlinFlow_一眼看全_Flow操作符大全_掘金]( https://juejin.cn/post/6989536876096913439 \)'
```

然后再次使用curl代码发送请求即可。