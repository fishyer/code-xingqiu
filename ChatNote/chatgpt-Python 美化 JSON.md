**You:**

Python将紧凑的json字符串美化一下

* * *

**ChatGPT:**

您可以使用Python内置的json模块中的dumps函数，将紧凑的JSON字符串转换为格式良好的字符串。

下面是一个简单的示例代码：

```python
import json

# 紧凑的JSON字符串
compact_json = '{"name":"John","age":30,"city":"New York"}'

# 使用dumps函数将紧凑的JSON字符串转换为格式良好的字符串
pretty_json = json.dumps(json.loads(compact_json), indent=4, sort_keys=True)

print(pretty_json)
```

输出：

```json
{
    "age": 30,
    "city": "New York",
    "name": "John"
}
```

在上面的代码中，我们首先导入了Python内置的json模块。然后，我们定义了一个紧凑的JSON字符串，并使用loads函数将其转换为Python对象。接下来，我们使用dumps函数将Python对象转换为格式良好的JSON字符串。dumps函数的indent参数用于指定缩进量，sort\_keys参数用于指定是否按键排序。最后，我们将格式良好的JSON字符串打印出来。