**You:**

Python获取当前运行的文件路径

* * *

**ChatGPT:**

你可以使用Python的`os`模块来获取当前运行的文件路径，具体操作如下：

```python
import os

current_path = os.path.abspath(__file__)
print(current_path)
```

在这个例子中，`os.path.abspath(__file__)` 返回一个表示当前文件的绝对路径的字符串。 `__file__` 是一个内置变量，表示当前运行的模块（文件）的文件名。 `os.path.abspath` 将相对路径转换为绝对路径。

如果你想获取当前工作目录，你可以使用`os.getcwd()`：

```python
import os

current_path = os.getcwd()
print(current_path)
```

`os.getcwd()` 返回当前工作目录的绝对路径的字符串。