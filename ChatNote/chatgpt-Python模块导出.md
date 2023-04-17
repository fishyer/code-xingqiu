**You:**

Python模块类，怎么将和\_\_init\_\_.py同级的所有py文件都导出

* * *

**ChatGPT:**

要将与`__init__.py`同级的所有Python文件都导出，你可以在`__init__.py`中使用`import`语句导入这些文件，然后将它们添加到`__all__`列表中。

下面是一个示例`__init__.py`文件，它将同级目录下的所有`.py`文件都导出为该模块的成员：

```python
import os
import glob

# 获取当前文件所在目录
dir_path = os.path.dirname(os.path.realpath(__file__))

# 使用 glob 模块获取同级目录下的所有 .py 文件的文件名
py_files = glob.glob(os.path.join(dir_path, "*.py"))

# 将文件名转换为模块名
modules = [os.path.basename(f)[:-3] for f in py_files if not f.endswith("__init__.py")]

# 将模块名添加到 __all__ 列表中
__all__ = modules

# 导入模块
for module in modules:
    __import__(module, globals(), locals())
```

在这个示例中，我们首先使用`os`模块和`glob`模块获取同级目录下所有`.py`文件的文件名。然后将这些文件名转换为模块名，并将它们添加到`__all__`列表中。最后，使用`__import__`函数将这些模块导入并添加到当前模块的命名空间中。

通过这种方式，你可以轻松地将同级目录下的所有`.py`文件都导出为一个模块的成员。注意，如果某个文件中定义的函数或变量不想被导出，你可以在该文件中使用`__all__`变量来限制它们的导出。