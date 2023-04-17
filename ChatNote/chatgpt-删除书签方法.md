**You:**

请参考下面代码写一个删除书签的方法： //添加到书签数据库中 function addBookmark(title, url) { var xhr = new XMLHttpRequest(); xhr.onload = function () { if (xhr.status === 200) { console.log(xhr.responseText); var response = JSON.parse(xhr.responseText); var code = response.code; // alert(code); updateIcon(1); bgPage.myGlobal\[url\] = 1 // alert(JSON.stringify(bgPage.myGlobal)); } else { console.log("Request failed. Status: " + xhr.status); } }; xhr.open("POST", "http://127.0.0.1:5000/add\_bookmark", true); xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded"); var encodedTitle = encodeURIComponent(title); var encodedUrl = encodeURIComponent(url); var params = "url=" + encodedUrl + "&title=" + encodedTitle; xhr.send(params); } 删除书签的接口为： curl --location --request POST 'http://localhost:5000/delete\_bookmark\_by\_url' \\ --header 'Content-Type: application/x-www-form-urlencoded' \\ --data-urlencode 'url=https://wanandroid.com/333'

* * *

**ChatGPT:**

根据提供的删除书签的接口，可以编写如下的删除书签的方法：

```javascript
function deleteBookmark(url) {
    var xhr = new XMLHttpRequest();
    xhr.onload = function () {
        if (xhr.status === 200) {
            console.log(xhr.responseText);
            var response = JSON.parse(xhr.responseText);
            var code = response.code;
            // alert(code);
            updateIcon(0);
            delete bgPage.myGlobal[url];
            // alert(JSON.stringify(bgPage.myGlobal));
        } else {
            console.log("Request failed. Status: " + xhr.status);
        }
    };
    xhr.open("POST", "http://127.0.0.1:5000/delete_bookmark_by_url", true);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    var encodedUrl = encodeURIComponent(url);
    var params = "url=" + encodedUrl;
    xhr.send(params);
}
```

该方法与添加书签的方法类似，使用了XMLHttpRequest对象来发送POST请求，将要删除的书签的URL通过参数传递给后端接口。在接收到响应后，根据响应中的状态码和内容，更新页面的图标和全局变量中存储的书签信息。需要注意的是，删除书签时没有需要编码的标题参数，因为删除操作只需要知道URL即可。

* * *

**You:**

请完成popup.html，在点击确认时，调用background.js的删除书签方法。 <!DOCTYPE html> <html> <head> <title>My Plugin</title> </head> <body> <input type="text" id="url"> <input type="text" id="title"> <button id="confirm">Confirm</button> <script src="popup.js"></script> </body> </html>

* * *

**ChatGPT:**

下面是完成了确认按钮点击事件调用 background.js 的删除书签方法的 popup.html 的代码：

```php-template
<!DOCTYPE html>
<html>
  <head>
    <title>My Plugin</title>
  </head>
  <body>
    <input type="text" id="url">
    <input type="text" id="title">
    <button id="confirm">Confirm</button>
    <script src="popup.js"></script>
  </body>
</html>
```

在上面的代码中，我们定义了一个输入URL和标题的文本框和一个确认按钮，当用户点击确认按钮时，会触发 popup.js 中的逻辑。在这里，我们只需添加一个点击事件处理程序，调用 background.js 中的删除书签方法即可。

下面是相应的 popup.js 代码：

```javascript
document.addEventListener('DOMContentLoaded', function() {
  var confirmButton = document.getElementById('confirm');
  confirmButton.addEventListener('click', function() {
    var url = document.getElementById('url').value;
    var title = document.getElementById('title').value;
    chrome.runtime.getBackgroundPage(function(bgPage) {
      bgPage.deleteBookmark(url);
    });
  });
});
```

在上面的代码中，我们首先在DOM加载完成时添加了一个事件监听器。当确认按钮被点击时，我们获取输入框中的URL和标题，并通过 chrome.runtime.getBackgroundPage() 方法获取 background.js 中的全局对象。然后，我们可以调用该对象的 deleteBookmark() 方法，将要删除的书签的URL传递给该方法，这将会调用我们之前编写的删除书签方法并发送请求给后端接口来删除该书签。

* * *

**You:**

bgPage错了，我要调用的是另一个js文件中的方法

* * *

**ChatGPT:**

如果您要调用另一个js文件中的方法而不是background.js中的方法，可以通过chrome.extension.getBackgroundPage()方法获取到扩展程序的后台页，然后调用该页中的方法。

下面是相应的 popup.js 代码：

```javascript
document.addEventListener('DOMContentLoaded', function() {
  var confirmButton = document.getElementById('confirm');
  confirmButton.addEventListener('click', function() {
    var url = document.getElementById('url').value;
    var title = document.getElementById('title').value;
    chrome.extension.getBackgroundPage(function(bgPage) {
      bgPage.deleteBookmark(url);
    });
  });
});
```

在上面的代码中，我们首先在DOM加载完成时添加了一个事件监听器。当确认按钮被点击时，我们获取输入框中的URL和标题，并通过chrome.extension.getBackgroundPage()方法获取到扩展程序的后台页。然后，我们可以调用该页中的 deleteBookmark() 方法，将要删除的书签的URL传递给该方法，这将会调用您所需要的删除书签方法并发送请求给后端接口来删除该书签。