**You:**

Chrome插件： { "manifest\_version": 2, "name": "AAA-Google URL Checker", "version": "1.0", "description": "Changes the extension icon to blue if the current URL starts with https://www.google.com and grey otherwise.", "icons": { "128": "nav0\_128.png" }, "browser\_action": { "default\_icon": { "128": "nav0\_128.png" } }, "permissions": \[ "activeTab", "tabs", "storage", "https://www.google.com/\*", "http://localhost:5000/\*" \], "content\_scripts": \[{ "matches": \["https://www.google.com/\*"\], "js": \["content-script.js"\], "run\_at": "document\_end", "css": \["content.css"\] }\], "background": { "scripts": \["background.js"\], "persistent": false } } 现在有一个弹出页面中的js:popup.js，它想调用background.js的一个函数，应该怎么调用？

* * *

**ChatGPT:**

要在弹出页面中调用background.js的一个函数，可以使用Chrome扩展程序API中的chrome.runtime.sendMessage()方法。

具体实现步骤如下：

1.  在popup.js中调用chrome.runtime.sendMessage()方法，将消息发送到background.js中。
    
    ```javascript
    chrome.runtime.sendMessage({action: "functionName", data: "messageData"}, function(response) {
        console.log(response);
    });
    ```
    
    这里的"functionName"是background.js中定义的函数名，"messageData"是需要传递给函数的数据。
    
2.  在background.js中使用chrome.runtime.onMessage.addListener()方法监听消息，并根据收到的消息调用相应的函数。
    
    ```javascript
    chrome.runtime.onMessage.addListener(function(request, sender, sendResponse) {
        if(request.action == "functionName") {
            functionName(request.data);
            sendResponse("Function executed successfully!");
        }
    });
    ```
    
    这里的"functionName"是要调用的函数名，request.data是从popup.js中传递过来的数据。函数执行后，使用sendResponse()方法返回一个响应消息。
    

这样就可以在popup.js中调用background.js中的函数了。