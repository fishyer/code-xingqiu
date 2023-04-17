**You:**

请帮我优化下面的代码，主要是优化api返回的数据，最好能以md格式查看 import logging import time import concurrent.futures from openai2 import Chat import threading import os # 创建日志文件夹 log\_dir = "logs" if not os.path.exists(log\_dir): os.makedirs(log\_dir) # 配置 logging 模块 logging.basicConfig( level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s', handlers=\[ logging.FileHandler(f'{log\_dir}/chat\_{time.strftime("%Y%m%d")}.log'), logging.StreamHandler() \] ) def createSession(): return Chat(api\_key='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', model="gpt-3.5-turbo") lastSession=createSession(); lastNum=0 # 获取上一个会话 def recentSession(): global lastNum if lastNum>10: logging.info("当前会话已超过10次，重新建立一个新的会话") return newSession() lastNum=lastNum+1 return lastSession # 新建一个会话 def newSession(): global lastNum lastNum=0 return createSession() # 定义线程池 executor = concurrent.futures.ThreadPoolExecutor(max\_workers=2) # 定义计数器函数 def count\_task(event): count = 0 while not event.is\_set(): count += 1 print(f"Count:{count}",end="\\r") time.sleep(1) print(end="\\n") return count # 定义网络请求函数 def request\_task(session,question): result = session.request(question) return result # mode=0，继续原来的会话，否则就新建会话 def request\_with\_count(question,mode=0): if mode==0: session=recentSession() else: session=newSession() logging.info("Request Data:" + question) print(f"Session hash code: {id(session)}") print("lastNum: "+str(lastNum)) # 提交计数器任务 event = threading.Event() count\_future = executor.submit(count\_task,event) # 提交网络请求任务 request\_future = executor.submit(request\_task,session,question) # 获取网络返回结果 request\_result = request\_future.result() # 获取计数器返回结果 event.set() count\_result = count\_future.result() print("Count Result:" + str(count\_result)) logging.info("Response Data:" + str(request\_result)) if \_\_name\_\_ == "\_\_main\_\_": question = "数字1的后面是哪个数字?" request\_with\_count(question)

* * *

**ChatGPT:**

以下是针对给出代码的优化建议：

1.  减少全局变量的使用，因为全局变量会增加代码的复杂度并可能导致难以调试的问题。可以考虑使用函数参数或者将变量作为类属性进行传递和使用。
    
2.  尽可能避免使用 print() 函数输出信息，因为它不便于日志记录和维护。建议使用 logging 模块进行日志记录，并使用字符串格式化的方式将信息记录到日志中。
    
3.  考虑使用上下文管理器（with 语句）来管理线程池和会话资源的生命周期，以确保它们在不再需要时被正确地关闭和释放。
    
4.  对于 API 返回的数据，可以使用 JSON 格式进行序列化并记录到日志文件中。如果需要以 Markdown 格式查看，可以在日志记录时使用格式化字符串，将 JSON 数据转换成 Markdown 表格格式。
    

以下是修改后的代码，其中已经应用了以上优化建议：

```python
import logging
import time
import concurrent.futures
import threading
import json
import os

from openai2 import Chat

# 创建日志文件夹
log_dir = "logs"
if not os.path.exists(log_dir):
    os.makedirs(log_dir)

# 配置 logging 模块
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler(f'{log_dir}/chat_{time.strftime("%Y%m%d")}.log'),
        logging.StreamHandler()
    ]
)

class ChatSession:
    def __init__(self, api_key, model):
        self.session = Chat(api_key=api_key, model=model)
        self.num_requests = 0

    def __enter__(self):
        self.num_requests += 1
        if self.num_requests > 10:
            logging.info("当前会话已超过10次，重新建立一个新的会话")
            self.session = Chat(api_key=self.session.api_key, model=self.session.model)
            self.num_requests = 1
        return self.session

    def __exit__(self, exc_type, exc_value, traceback):
        self.session = None

# 定义线程池
executor = concurrent.futures.ThreadPoolExecutor(max_workers=2)

# 定义计数器函数
def count_task(event):
    count = 0
    while not event.is_set():
        count += 1
        time.sleep(1)
    return count

# 定义网络请求函数
def request_task(session, question):
    result = session.request(question)
    return result

# mode=0，继续原来的会话，否则就新建会话
def request_with_count(question, mode=0):
    with ChatSession(api_key='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', model="gpt-3.5-turbo") as session:
        logging.info("Request Data: " + question)
        # 提交计数器任务
        event = threading.Event()
        count_future = executor.submit(count_task, event)
        # 提交网络请求任务
        request_future = executor.submit(request_task, session,
```

* * *

**You:**

继续

* * *

**ChatGPT:**

question)

```python
    # 获取网络返回结果
    request_result = request_future.result()
    # 获取计数器返回结果
    event.set()
    count_result = count_future.result()

    logging.info("Response Data: " + json.dumps(request_result))

    # 将 API 返回结果转换成 Markdown 表格格式，并记录到日志文件中
    if isinstance(request_result, list):
        response_md = "| 序号 | 结果 |\n| --- | --- |\n"
        for i, result in enumerate(request_result):
            response_md += f"| {i + 1} | {result['text']} |\n"
    else:
        response_md = f"| 结果 |\n| --- |\n| {request_result['text']} |\n"

    logging.info("Response Data (Markdown): " + response_md)
```

if **name** == "**main**": question = "数字1的后面是哪个数字?" request\_with\_count(question)

* * *

**You:**

python调用chatgpt以后，怎么美化它的返回结果，不要挤在一行了

* * *

**ChatGPT:**

如果您在 Python 中调用了 ChatGPT 模型进行文本生成，可以使用一些字符串处理方法对生成的文本进行美化，例如使用换行符（\\n）将长文本分成多行，或使用制表符（\\t）进行缩进。以下是一个简单的例子：

```python
from openai2 import Chat

model = Chat(api_key='your-api-key', model='gpt-3')

prompt = "Hello, how are you?"
response = model.request(prompt)

# 将每个句子独立成一行
response = response.replace('. ', '.\n')

# 将制表符添加到每个段落开头
response = '\t' + response.replace('\n', '\n\t')

print(response)
```

在上面的示例中，我们首先通过 `model.request()` 方法生成了一些文本，然后使用 `replace()` 方法将每个句子分割成一行，并在每个段落的开头添加制表符。最后，我们将美化后的文本打印到控制台上。当然，您可以根据需要进行更复杂的字符串处理，以实现您想要的效果。