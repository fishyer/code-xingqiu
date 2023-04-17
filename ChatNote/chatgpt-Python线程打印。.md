**You:**

Python打印当前线程名

* * *

**ChatGPT:**

您可以使用Python中的threading模块打印当前线程的名称。可以使用`current_thread()`方法获取当前线程的Thread对象，然后使用Thread对象的`getName()`方法获取当前线程的名称。以下是示例代码：

```python
import threading

def print_current_thread_name():
    current_thread = threading.current_thread()
    print("Current thread name is:", current_thread.getName())

print_current_thread_name()
```

输出将类似于以下内容：

```python
Current thread name is: MainThread
```