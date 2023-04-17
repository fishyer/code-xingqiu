**You:**

帮我写一个python文件，使用flask，Get请求的方法名为convert，将驼峰转下划线，入参为testCase或TestCase，出参为test\_case。

* * *

**ChatGPT:**

好的，以下是一个使用 Flask 的 Python 文件，实现了一个 GET 请求的方法名为 `convert`，接受一个参数 `name`，将其转换为下划线格式并返回。

```python
from flask import Flask

app = Flask(__name__)

@app.route('/convert/<string:name>')
def convert(name):
    if name.islower():
        return name
    else:
        name = name.replace('-', '_')
        name = ''.join(['_'+i.lower() if i.isupper() else i for i in name]).lstrip('_')
        return name

if __name__ == '__main__':
    app.run()
```

这个函数首先检查输入参数是否为全小写，如果是则直接返回该参数。否则，将参数中的 `-` 替换为 `_`，然后将所有大写字母前面添加一个 `_` 并转换为小写字母，最后再将前面多余的 `_` 去掉。这样就能实现将驼峰式命名转换为下划线式命名的功能。