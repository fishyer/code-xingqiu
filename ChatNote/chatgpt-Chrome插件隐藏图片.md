**You:**

帮我写一个chrome插件，隐藏掉当前网页的所有图片

* * *

**ChatGPT:**

好的，以下是一个简单的Chrome插件，可以隐藏当前网页的所有图片：

1.  首先，创建一个新的文件夹来保存插件文件。例如，可以将文件夹命名为“hide-images”。
    
2.  在文件夹中创建一个名为“manifest.json”的文件，并将以下内容复制到其中：
    

```json
{
  "name": "Hide Images",
  "version": "1.0",
  "manifest_version": 2,
  "description": "A simple Chrome extension to hide all images on the current page.",
  "permissions": [
    "activeTab"
  ],
  "content_scripts": [
    {
      "matches": [
        "<all_urls>"
      ],
      "js": ["content.js"]
    }
  ]
}
```

这个文件是Chrome插件的清单文件，用于定义插件的名称、版本、描述和功能等信息。

3.  在同一文件夹中创建一个名为“content.js”的文件，并将以下代码复制到其中：

```css
var images = document.getElementsByTagName("img");
for (var i = 0; i < images.length; i++) {
  images[i].style.visibility = "hidden";
}
```

这个文件将包含实际的代码，它将遍历当前页面中的所有图像，并将它们的可见性设置为“hidden”，从而隐藏它们。

4.  将文件夹“hide-images”打包为一个zip文件，然后在Chrome浏览器中打开“chrome://extensions/”页面。
    
5.  在该页面中启用“开发者模式”，然后点击“加载已解压的扩展程序”按钮，选择刚刚打包的zip文件并加载它。
    
6.  完成后，打开一个网页，插件会自动运行并隐藏该页面的所有图片。
    

希望这个简单的示例能够帮助你开始创建Chrome插件！