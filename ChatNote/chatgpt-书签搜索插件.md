**You:**

帮我写一个Chrome插件，在google搜索结果列表的前面插入书签搜索结果

* * *

**ChatGPT:**

好的，以下是一个简单的Chrome插件代码，可以在Google搜索结果列表前面插入书签搜索结果。需要注意的是，由于Google搜索结果的HTML结构可能随时更改，所以这个插件可能需要随着时间的推移进行更新以确保其正常工作。

manifest.json文件:

```json
{
  "manifest_version": 2,
  "name": "书签搜索结果插件",
  "version": "1.0",
  "description": "在Google搜索结果列表前面插入书签搜索结果",
  "permissions": ["tabs"],
  "content_scripts": [
    {
      "matches": ["https://www.google.com/*"],
      "js": ["content.js"]
    }
  ]
}
```

content.js文件:

```javascript
// 获取搜索关键词
const searchInput = document.querySelector('input[name="q"]');
const searchTerm = searchInput.value;

// 构造书签搜索链接
const bookmarkSearchUrl = `chrome://bookmarks/?q=${searchTerm}`;

// 创建书签搜索结果节点
const bookmarkLink = document.createElement('a');
bookmarkLink.href = bookmarkSearchUrl;
bookmarkLink.target = '_blank';
bookmarkLink.innerText = '查看书签';

// 获取搜索结果列表
const searchResults = document.querySelector('#search');

// 在搜索结果列表前插入书签搜索结果
searchResults.insertAdjacentElement('beforebegin', bookmarkLink);
```

这个插件的逻辑很简单：获取搜索关键词，构造书签搜索链接，创建书签搜索结果节点，然后将它插入到搜索结果列表前面。您可以将这些代码复制到一个文本编辑器中，并将文件保存为manifest.json和content.js。然后将这两个文件压缩成一个zip文件，将其上传到Chrome网上应用店以安装插件。