**You:**

通过通过命令行启动flask服务

* * *

**ChatGPT:**

要通过命令行启动 Flask 服务，需要执行以下步骤：

1.  打开终端或命令提示符窗口，进入 Flask 应用程序所在的目录。
    
2.  设置 Flask 应用程序的环境变量，例如在 Linux 或 macOS 系统上，可以执行以下命令：
    

```javascript
export FLASK_APP=app.py
```

其中 `app.py` 是 Flask 应用程序的主要 Python 文件。如果你使用 Windows 系统，则可以执行以下命令：

```python
set FLASK_APP=app.py
```

3.  启动 Flask 服务。在终端或命令提示符窗口中，执行以下命令：

```
flask run
```

此命令将启动 Flask 应用程序的开发服务器，并监听默认端口 5000。如果你需要指定不同的端口，可以使用 `--port` 参数，例如：

```css
flask run --port=8000
```

注意，这种方法只适用于开发环境，不应该在生产环境中使用。在生产环境中，你应该使用一个 WSGI 服务器（例如 Gunicorn 或 uWSGI）来运行 Flask 应用程序。