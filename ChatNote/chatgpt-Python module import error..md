**You:**

目录结构为： -MyCode -python - lib - \_\_init\_\_.py - datetime\_util.py - test - test\_datetime\_util.py 已经在配置文件添加了lib模块的父目录为默认搜索路径： export PYTHONPATH=$PYTHONPATH:/Users/yutianran/MyGithub/MyCode/python 在test\_datetime\_util.py中引入datetime\_util.py： from lib.datetime\_util import \* 为什么会报错： ModuleNotFoundError: No module named 'lib'

* * *

**ChatGPT:**

根据您提供的目录结构和 PYTHONPATH 的配置，可以看出，`PYTHONPATH` 已经添加了 `MyCode/python` 这个目录，所以 `lib` 目录应该可以被找到。然而，在 `test_datetime_util.py` 文件中，您使用的是 `from lib.datetime_util import *`，这会导致 Python 解释器从当前目录开始查找 `lib` 目录，而不是从 `MyCode/python` 开始查找。

为了解决这个问题，您可以使用相对导入，而不是绝对导入。在 `test_datetime_util.py` 文件中，将 `from lib.datetime_util import *` 改为 `from ..lib.datetime_util import *`，即可解决这个问题。

* * *

**You:**

from ..lib.datetime\_util import \* E ImportError: attempted relative import with no known parent package

* * *

**ChatGPT:**

如果在使用相对导入时出现 `ImportError: attempted relative import with no known parent package` 错误，这通常是因为 Python 解释器无法确定当前脚本的父包。

这个问题通常出现在执行脚本时，而不是在导入模块时。为了解决这个问题，您可以将脚本作为包的一部分来运行，或者使用绝对导入。

具体而言，您可以将 `test_datetime_util.py` 文件所在的目录添加到 `PYTHONPATH` 中，然后在该目录下使用 `python -m unittest` 命令来运行测试。

另外，如果您要在相对导入中使用包名，而不是 `..`，您需要确保当前模块的 `__name__` 属性是一个包名。如果 `__name__` 属性为 `__main__`，则无法使用相对导入。在这种情况下，您可以使用绝对导入，或将脚本作为包的一部分来运行。