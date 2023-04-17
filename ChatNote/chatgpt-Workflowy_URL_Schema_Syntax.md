**You:**

请列出workflowy的所有url schema语法

* * *

**ChatGPT:**

Workflowy 是一个支持 URL Schema 的应用程序，以下是 Workflowy 的 URL Schema 语法：

1.  `workflowy://` - 所有 Workflowy URL 的前缀。
2.  `workflowy://document/DOCUMENT_ID` - 打开具有给定 DOCUMENT\_ID 的文档。
3.  `workflowy://document/DOCUMENT_ID?query=SEARCH_TERM` - 打开具有给定 DOCUMENT\_ID 并搜索给定 SEARCH\_TERM 的文档。
4.  `workflowy://share/SHARE_ID` - 打开具有给定 SHARE\_ID 的共享文档。
5.  `workflowy://edit/DOCUMENT_ID?name=ITEM_NAME&desc=ITEM_DESC` - 打开具有给定 DOCUMENT\_ID 的文档并将具有给定 ITEM\_NAME 和 ITEM\_DESC 的新项添加到其中。
6.  `workflowy://edit/ITEM_ID?name=ITEM_NAME&desc=ITEM_DESC` - 打开具有给定 ITEM\_ID 的项目并将其名称和描述更改为给定的 ITEM\_NAME 和 ITEM\_DESC。
7.  `workflowy://edit/ITEM_ID?completed=true/false` - 将给定 ITEM\_ID 的状态更改为已完成或未完成。
8.  `workflowy://complete/ITEM_ID` - 将给定 ITEM\_ID 的状态更改为已完成。
9.  `workflowy://uncomplete/ITEM_ID` - 将给定 ITEM\_ID 的状态更改为未完成。

注意：所有 ID 都是 Workflowy 中文档或项目的唯一标识符。DOCUMENT\_ID 是文档的 ID，ITEM\_ID 是项目的 ID，SHARE\_ID 是共享文档的 ID。

* * *

**You:**

workflowy通过url shcema添加一个节点

* * *

**ChatGPT:**

可以使用以下 URL Schema 在 Workflowy 中添加一个节点：

```javascript
workflowy://edit/DOCUMENT_ID?name=ITEM_NAME
```

其中，DOCUMENT\_ID 是要添加节点的文档的 ID，ITEM\_NAME 是要添加的节点的名称。

例如，如果您想要添加一个名为“新节点”的节点到文档 ID 为 abcdefg 的文档中，可以使用以下 URL：

```perl
workflowy://edit/abcdefg?name=New%20Node
```

请注意，URL 中的空格必须被编码为 %20。

* * *

**You:**

怎么获取DOCUMENT\_ID？

* * *

**ChatGPT:**

要获取 Workflowy 文档的 ID，请执行以下步骤：

1.  在 Workflowy 中打开要查找 ID 的文档。
2.  点击文档的标题栏，以便在浏览器的地址栏中显示 URL。
3.  从地址栏中复制文档的 ID。文档 ID 是在 URL 的末尾，紧随着最后一个斜杠。

例如，在以下 URL 中：

```ruby
https://workflowy.com/s/abcdefg-1234-5678-90ab-cdef12345678/
```

文档的 ID 是 `abcdefg-1234-5678-90ab-cdef12345678`。

注意：每个 Workflowy 文档都有一个唯一的 ID，用于标识该文档。这个 ID 可以通过 Workflowy 的 API 或浏览器的开发者工具来获取，但是这些方法都需要一些技术知识。因此，最简单的方法是从浏览器的地址栏中复制文档的 ID。

* * *

**You:**

https://workflowy.com/s/1000-inbox/ejdTCRQHvsVqJWp7，最后面的是id么？

* * *

**ChatGPT:**

是的，URL 中最后一个斜杠后面的字符串就是 Workflowy 中特定文档的 ID。在您提供的 URL 中，`ejdTCRQHvsVqJWp7` 就是名为 `1000-inbox` 的文档的 ID。

* * *

**You:**

在这个文档下面添加一个test001的笔记

* * *

**ChatGPT:**

要在名为 `1000-inbox` 的文档下添加一个名为 `test001` 的笔记，请按照以下步骤操作：

1.  复制以下链接并将 `DOCUMENT_ID` 替换为文档的实际 ID。
    
    ```javascript
    workflowy://edit/DOCUMENT_ID?name=test001
    ```
    
2.  在浏览器的地址栏中粘贴链接并按 Enter 键。
    
3.  您将被重定向到 Workflowy 网站，并在文档中添加一个名为 `test001` 的项目。
    

请注意，如果您不在 Workflowy 中登录，或者已经登录但没有打开 Workflowy 页面，则在单击链接后，系统可能会提示您先登录或打开 Workflowy 网站。