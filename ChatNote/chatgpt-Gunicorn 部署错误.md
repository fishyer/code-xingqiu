**You:**

用gunicorn部署app\_flask.py文件，nohup gunicorn -c $(pwd)/python/main/gunicorn\_config.py app\_flask:app &> log/uwsgi.log & 报错：ModuleNotFoundError: No module named 'app\_flask'

* * *

**ChatGPT:**

出现该错误的原因可能是因为当前工作目录不包含名为 `app_flask` 的Python模块。

请确保在运行 `gunicorn` 命令之前，进入包含 `app_flask.py` 文件的目录。此外，确保已安装所有必需的依赖项和模块。

还要注意，在 `gunicorn` 命令中，`app_flask:app` 意味着将 `app_flask.py` 文件中的 Flask 应用程序命名为 `app`。如果在 `app_flask.py` 中使用了其他名称，请相应地更改命令。