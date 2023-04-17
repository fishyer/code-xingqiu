**You:**

flask服务请支持192.168.31.116访问

* * *

**ChatGPT:**

要使Flask应用程序支持在本地IP地址192.168.31.116上进行访问，可以使用以下代码：

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='192.168.31.116', port=5000)
```

在这个例子中，我们使用了Flask框架来创建一个名为`app`的应用程序对象。我们定义了一个路由`'/'`，并返回一个简单的字符串`'Hello, World!'`。

然后，我们在`if __name__ == '__main__'`块中调用`app.run()`函数来运行应用程序。我们将`host`参数设置为`'192.168.31.116'`以允许在本地IP地址上进行访问。默认情况下，Flask应用程序运行在端口5000上，因此我们不需要指定`port`参数。

启动应用程序后，您可以使用浏览器或其他HTTP客户端来访问`http://192.168.31.116:5000/`，这将返回`Hello, World!`字符串。

* * *

**You:**

flask服务请支持192.168.31.116和localhost访问

* * *

**ChatGPT:**

要使Flask应用程序同时支持在本地IP地址192.168.31.116和localhost上进行访问，可以使用以下代码：

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

在这个例子中，我们将`host`参数设置为`'0.0.0.0'`，这意味着Flask应用程序将监听所有可用的网络接口，包括本地IP地址和localhost。默认情况下，Flask应用程序运行在端口5000上，因此我们不需要指定`port`参数。

启动应用程序后，您可以使用浏览器或其他HTTP客户端来访问`http://192.168.31.116:5000/`或`http://localhost:5000/`，这将返回`Hello, World!`字符串。注意，您需要在本地计算机或网络中打开防火墙规则以允许对该端口的访问。