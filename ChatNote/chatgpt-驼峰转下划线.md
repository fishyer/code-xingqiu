**You:**

用python实现驼峰转下划线

* * *

**ChatGPT:**

以下是将驼峰式字符串转换为下划线式字符串的Python函数：

```python
def camel_to_snake(camel_case_string):
    snake_case_string = ""
    for i in range(len(camel_case_string)):
        if camel_case_string[i].isupper() and i > 0:
            snake_case_string += "_" + camel_case_string[i].lower()
        else:
            snake_case_string += camel_case_string[i].lower()
    return snake_case_string
```

这个函数使用一个循环遍历字符串的每个字符，并在必要时将其转换为小写字母，并在前面添加下划线。要使用该函数，请将驼峰式字符串作为参数传递给它，它将返回下划线式字符串。

以下是一个例子：

```python
>>> camel_to_snake("camelCaseString")
'camel_case_string'
```