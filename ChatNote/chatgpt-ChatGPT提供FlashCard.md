**You:**

请写一个python脚本，将这种格式的源md文本，转成目标md文本. 源md： \*\*You:\*\* 这是问题1 \* \* \* \*\*ChatGPT:\*\* 这是回答1 \* \* \* \*\*You:\*\* 这是问题2 \* \* \* \*\*ChatGPT:\*\* 这是回答2 \* \* \* \*\*You:\*\* 这是问题3 \* \* \* \*\*ChatGPT:\*\* 这是回答3 \* \* \* 目标md: - #FlashCard-ChatGPT - 这是问题1 #card - 这是回答1 - 这是问题2 #card - 这是回答2 - 这是问题3 #card - 这是回答3

* * *

**ChatGPT:**

下面是一个Python脚本，可以将给定的源md文本转换为目标md文本格式：

```python
source_md = """
**You:**

这是问题1

* * *

**ChatGPT:**

这是回答1

* * *

**You:**

这是问题2

* * *

**ChatGPT:**

这是回答2

* * *

**You:**

这是问题3

* * *

**ChatGPT:**

这是回答3

* * *
"""

target_md = "- #FlashCard-ChatGPT\n"

lines = source_md.strip().split("\n")

for i in range(0, len(lines), 4):
    question = lines[i+1].strip()
    answer = lines[i+3].strip()
    target_md += f"- {question} #card\n"
    target_md += f"    - {answer}\n"

print(target_md)
```

请将以上代码复制到Python编辑器中，并运行它，您将得到目标md文本格式的输出。请注意，源md文本必须在代码中作为字符串提供。如果您想要将脚本的输出写入文件，可以使用以下代码行：

```python
with open("output.md", "w") as f:
    f.write(target_md)
```

它将脚本的输出写入名为“output.md”的文件中。

* * *

**You:**

改成source\_md和target\_md都是从md文件读入

* * *

**ChatGPT:**

好的，这里是修改后的代码，它从源md文件和目标md文件中读取输入和输出：

```python
source_file = "source.md"
target_file = "target.md"

with open(source_file, "r") as f:
    source_md = f.read()

target_md = "- #FlashCard-ChatGPT\n"

lines = source_md.strip().split("\n")

for i in range(0, len(lines), 4):
    question = lines[i+1].strip()
    answer = lines[i+3].strip()
    target_md += f"- {question} #card\n"
    target_md += f"    - {answer}\n"

with open(target_file, "w") as f:
    f.write(target_md)
```

这里，我们首先打开源md文件，将其内容读入source\_md字符串中。然后，我们创建一个名为target\_md的空字符串，并对源md文件中的行进行迭代。我们假设每个问题和答案都是四行，因此我们从第0行开始，每次跳过4行。我们使用lines\[i+1\]和lines\[i+3\]分别获取问题和答案。然后，我们将问题和答案添加到目标md字符串中，以匹配目标格式。最后，我们打开目标md文件，并将目标md字符串写入其中。

请注意，源md文件和目标md文件的名称在代码的开始部分定义，您可以根据需要进行更改。另外，请确保源md文件中的问题和答案遵循正确的格式，否则脚本可能无法正确解析它们。

* * *

**You:**

使用watchdog监听指定文件夹，发现有新的md文件，就执行上面的md转换

* * *

**ChatGPT:**

好的，这里是修改后的代码，它使用watchdog库来监听指定文件夹中的文件，并在发现新的md文件时执行上面的md转换：

```python
import os
import time
import watchdog.events
import watchdog.observers

source_folder = "/path/to/source/folder"
target_folder = "/path/to/target/folder"

def convert_md(source_file, target_file):
    with open(source_file, "r") as f:
        source_md = f.read()

    target_md = "- #FlashCard-ChatGPT\n"

    lines = source_md.strip().split("\n")

    for i in range(0, len(lines), 4):
        question = lines[i+1].strip()
        answer = lines[i+3].strip()
        target_md += f"- {question} #card\n"
        target_md += f"    - {answer}\n"

    with open(target_file, "w") as f:
        f.write(target_md)

class MdFileHandler(watchdog.events.PatternMatchingEventHandler):
    def __init__(self):
        super().__init__(patterns=["*.md"])

    def on_created(self, event):
        source_file = event.src_path
        target_file = os.path.join(target_folder, os.path.basename(source_file))
        convert_md(source_file, target_file)

if __name__ == "__main__":
    event_handler = MdFileHandler()
    observer = watchdog.observers.Observer()
    observer.schedule(event_handler, source_folder, recursive=True)
    observer.start()

    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()

    observer.join()
```

这里，我们定义了一个名为convert\_md的函数，它接受源md文件和目标md文件的路径，并执行上面的md转换。然后，我们定义了一个名为MdFileHandler的类，它是watchdog库中的一个事件处理程序，用于监听新创建的md文件。当一个新的md文件被创建时，我们从事件中获取源md文件的路径，并使用os.path.basename获取文件名。然后，我们将目标md文件的路径设置为目标文件夹和源文件名的组合。最后，我们调用convert\_md函数，将源md文件转换为目标md文件。

在\_\_main\_\_部分，我们创建了一个MdFileHandler实例和一个Observer实例，并将事件处理程序和观察目录设置为源文件夹。然后，我们启动观察者，并使用一个无限循环来防止程序退出。如果用户按下Ctrl+C键，我们将停止观察者，并使用observer.join()等待所有事件处理程序完成处理。

* * *

**You:**

打印出每一个监听到的文件啊

* * *

**ChatGPT:**

好的，这里是修改后的代码，它将打印出每一个监听到的md文件：

```python
import os
import time
import watchdog.events
import watchdog.observers

source_folder = "/path/to/source/folder"
target_folder = "/path/to/target/folder"

def convert_md(source_file, target_file):
    with open(source_file, "r") as f:
        source_md = f.read()

    target_md = "- #FlashCard-ChatGPT\n"

    lines = source_md.strip().split("\n")

    for i in range(0, len(lines), 4):
        question = lines[i+1].strip()
        answer = lines[i+3].strip()
        target_md += f"- {question} #card\n"
        target_md += f"    - {answer}\n"

    with open(target_file, "w") as f:
        f.write(target_md)

class MdFileHandler(watchdog.events.PatternMatchingEventHandler):
    def __init__(self):
        super().__init__(patterns=["*.md"])

    def on_created(self, event):
        source_file = event.src_path
        target_file = os.path.join(target_folder, os.path.basename(source_file))
        print(f"Converting {source_file} to {target_file}")
        convert_md(source_file, target_file)

if __name__ == "__main__":
    event_handler = MdFileHandler()
    observer = watchdog.observers.Observer()
    observer.schedule(event_handler, source_folder, recursive=True)
    observer.start()

    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()

    observer.join()
```

在MdFileHandler的on\_created方法中，我们添加了一行代码，以打印正在转换的源文件和目标文件的路径。当我们启动脚本时，它将开始监听指定的源文件夹，并在新的md文件被创建时进行转换并打印出它们的路径。如果您希望在转换完成后打印一条消息，可以在convert\_md函数末尾添加一行代码，如下所示：

```python
print(f"Converted {source_file} to {target_file}")
```

它将在转换完成后打印出源文件和目标文件的路径。

* * *

**You:**

请使用正则匹配来获取问题和答案。 - #FlashCard-ChatGPT - 这是问题1 #card - 这是回答1 - 这是问题2 #card - 这是回答2 - 这是问题3 #card - 这是回答3

* * *

**ChatGPT:**

好的，这里是修改后的代码，它使用正则表达式来获取源md文本中的问题和答案：

```python
import re

source_file = "source.md"
target_file = "target.md"

with open(source_file, "r") as f:
    source_md = f.read()

target_md = "- #FlashCard-ChatGPT\n"

pattern = re.compile(r"\*\*You:\*\*\n\n(.+?)\n\n\* \* \*\n\n\*\*ChatGPT:\*\*\n\n(.+?)\n\n\* \* \*\n\n", re.DOTALL)

matches = pattern.findall(source_md)

for match in matches:
    question = match[0].strip()
    answer = match[1].strip()
    target_md += f"- {question} #card\n"
    target_md += f"    - {answer}\n"

with open(target_file, "w") as f:
    f.write(target_md)
```

这里，我们首先打开源md文件，并将其内容读入source\_md字符串中。然后，我们定义一个名为pattern的正则表达式，它可以匹配出两个固定格式的文本块，其中一个是问题，另一个是答案。我们使用re.DOTALL标志，以便点号(.)匹配包括换行符在内的任何字符。

接下来，我们使用pattern.findall方法找到所有匹配项，并将其存储在名为matches的列表中。每个匹配项都是一个元组，包含问题和答案。然后，我们遍历matches列表，将每个问题和答案添加到目标md字符串中，以匹配目标格式。最后，我们打开目标md文件，并将目标md字符串写入其中。

请注意，在这种情况下，我们假设每个问题和答案之间有一行空白行，因此我们在正则表达式中使用了两个换行符。如果实际文本的格式不完全匹配，可能需要调整正则表达式。