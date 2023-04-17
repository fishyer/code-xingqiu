**You:**

python线程池中启动的线程，怎么通过event来停止

* * *

**ChatGPT:**

在 Python 中，可以使用 `threading` 模块创建和管理线程池，并使用 `Event` 类来实现线程间的通信。可以使用 `Event` 对象来控制线程池中的线程，使其在满足某个条件时停止运行。下面是一个简单的示例，展示了如何使用 `Event` 对象来停止线程池中的线程：

```python
import threading
import time

# 定义线程函数
def worker(event):
    while not event.is_set():
        # 在此处执行线程的工作任务
        print("working...")
        time.sleep(1)

# 创建 Event 对象
stop_event = threading.Event()

# 创建线程池，并启动线程
thread_pool = []
for i in range(5):
    t = threading.Thread(target=worker, args=(stop_event,))
    thread_pool.append(t)
    t.start()

# 5秒后停止线程池中的所有线程
time.sleep(5)
stop_event.set()

# 等待线程池中的所有线程结束
for t in thread_pool:
    t.join()
```

在上面的代码中，我们首先创建了一个 `Event` 对象 `stop_event`，并将其传递给每个线程。线程函数 `worker` 使用了 `while` 循环来不断执行工作任务，直到 `stop_event` 被设置为已触发。在主线程中，我们等待了 5 秒钟，然后设置了 `stop_event`，从而触发了所有线程中的 `while` 循环的退出条件。最后，我们使用 `join` 方法等待所有线程结束。

需要注意的是，`Event` 对象可以被多个线程共享，因此可以通过设置 `Event` 对象来停止线程池中的所有线程。同时，需要在线程函数中检查 `Event` 对象的状态，以便能够及时响应停止请求。

* * *

**You:**

怎么获取worker返回的结果呢？

* * *

**ChatGPT:**

在 Python 中，可以使用 `concurrent.futures` 模块中的 `ThreadPoolExecutor` 类来创建和管理线程池，并使用 `Future` 对象来获取每个线程的执行结果。`Future` 对象表示一个异步操作的结果，可以使用 `result()` 方法来阻塞主线程并等待异步操作完成并返回结果。下面是一个简单的示例，展示了如何使用 `ThreadPoolExecutor` 和 `Future` 对象来获取线程池中每个线程的执行结果：

```python
import concurrent.futures
import time

# 定义线程函数
def worker(num):
    # 在此处执行线程的工作任务
    time.sleep(1)
    return num * 2

# 创建 ThreadPoolExecutor 对象
executor = concurrent.futures.ThreadPoolExecutor(max_workers=5)

# 提交任务到线程池
futures = [executor.submit(worker, i) for i in range(5)]

# 等待所有任务完成，并获取执行结果
results = [f.result() for f in futures]

# 打印执行结果
print(results)
```

在上面的代码中，我们首先创建了一个 `ThreadPoolExecutor` 对象 `executor`，并使用 `submit` 方法将每个任务提交到线程池中。`submit` 方法返回一个 `Future` 对象，表示异步任务的结果。我们将所有 `Future` 对象存储在一个列表 `futures` 中。

接着，我们使用列表推导式和 `result()` 方法来获取每个任务的执行结果，并将其存储在列表 `results` 中。

最后，我们打印执行结果。在此例中，每个线程的工作任务都是将其输入的数字乘以 2，因此执行结果是一个列表，包含了数字 0 到 4 的乘以 2 后的结果。