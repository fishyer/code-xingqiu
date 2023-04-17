**You:**

帮我写一个脚本导出pdf中的所有高亮笔记

* * *

**ChatGPT:**

这个脚本可以使用Python编写，使用PyPDF2库来读取PDF文件，并使用re库来匹配高亮笔记。以下是一个简单的脚本：

```python
import PyPDF2
import re

# 定义PDF文件路径和输出文件路径
pdf_path = "example.pdf"
output_path = "highlights.txt"

# 用PyPDF2打开PDF文件
with open(pdf_path, "rb") as pdf_file:
    pdf_reader = PyPDF2.PdfFileReader(pdf_file)

    # 用正则表达式匹配所有高亮笔记
    pattern = r"/Type /Annot.*?/Subtype /Highlight.*?/Contents \((.*?)\)"
    highlights = re.findall(pattern, pdf_reader.getFormTextFields().to_json())

    # 将所有高亮笔记写入输出文件
    with open(output_path, "w") as output_file:
        for highlight in highlights:
            output_file.write(highlight + "\n")
```

这个脚本首先定义了PDF文件路径和输出文件路径。然后使用PyPDF2库打开PDF文件并创建一个PdfFileReader对象。接下来，使用正则表达式匹配所有高亮笔记。这个正则表达式的模式匹配PDF文件中所有类型为“Annot”且子类型为“Highlight”的注释，并提取注释的“Contents”字段中的内容。这些匹配项存储在名为“highlights”的列表中。最后，将所有高亮笔记写入输出文件中。

* * *

**You:**

AttributeError: 'dict' object has no attribute 'to\_json'

* * *

**ChatGPT:**

我猜测你可能是在使用较新版本的PyPDF2库。在最新版本的PyPDF2中，getFormTextFields()返回一个字典对象，因此to\_json()方法不再适用。

你可以使用json库来将字典对象转换为JSON字符串，然后使用正则表达式来匹配高亮笔记。以下是修改后的代码：

```python
import PyPDF2
import json
import re

# 定义PDF文件路径和输出文件路径
pdf_path = "example.pdf"
output_path = "highlights.txt"

# 用PyPDF2打开PDF文件
with open(pdf_path, "rb") as pdf_file:
    pdf_reader = PyPDF2.PdfFileReader(pdf_file)

    # 将表单字段转换为JSON字符串
    form_json = json.dumps(pdf_reader.getFormTextFields())

    # 用正则表达式匹配所有高亮笔记
    pattern = r"/Type /Annot.*?/Subtype /Highlight.*?/Contents \((.*?)\)"
    highlights = re.findall(pattern, form_json)

    # 将所有高亮笔记写入输出文件
    with open(output_path, "w") as output_file:
        for highlight in highlights:
            output_file.write(highlight + "\n")
```

这个脚本首先定义了PDF文件路径和输出文件路径。然后使用PyPDF2库打开PDF文件并创建一个PdfFileReader对象。接下来，将表单字段字典对象转换为JSON字符串，然后使用正则表达式匹配所有高亮笔记。这个正则表达式的模式匹配PDF文件中所有类型为“Annot”且子类型为“Highlight”的注释，并提取注释的“Contents”字段中的内容。这些匹配项存储在名为“highlights”的列表中。最后，将所有高亮笔记写入输出文件中。