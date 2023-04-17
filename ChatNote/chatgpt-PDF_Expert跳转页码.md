**You:**

pdf expert支持url schema跳转到指定页码么

* * *

**ChatGPT:**

PDF Expert是一款功能强大的PDF阅读器和编辑器，它支持在PDF文档中使用URL跳转到指定页面。

您可以使用以下格式的URL：

```perl
pdfexpert://{file_path}#{page_number}
```

其中，`{file_path}`是PDF文档的本地路径，`{page_number}`是要跳转的页码。

例如，要跳转到名为`example.pdf`的PDF文档中的第5页，您可以使用以下URL：

```bash
pdfexpert:///Users/username/Documents/example.pdf#5
```

请注意，URL中的文件路径必须是本地路径，并且文件必须在PDF Expert支持的文件格式列表中。