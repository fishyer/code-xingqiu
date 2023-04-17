**You:**

python中如何正确print这样的文本，记得将\\n变成换行： \\n\\n这是一份简单的Chrome插件demo，它包括以下内容：\\n\\n1. manifest.json文件（必需）

* * *

**ChatGPT:**

您可以使用字符串的转义字符来打印包含换行符的文本。以下是一种可能的实现方式：

```swift
print("\n\n这是一份简单的Chrome插件demo，它包括以下内容：\n\n1. manifest.json文件（必需）")
```

这将在控制台上打印出以下文本：

```markdown

这是一份简单的Chrome插件demo，它包括以下内容：

1. manifest.json文件（必需）
```

请注意，在字符串中使用双引号将字符串括起来，以便转义字符被解释并正确打印。