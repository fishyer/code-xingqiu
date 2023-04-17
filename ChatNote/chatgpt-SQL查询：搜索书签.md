**You:**

请修改这条sql，改成只要title里面包含搜索词就可以了： SELECT \* FROM bookmarks WHERE title LIKE 'Android';

* * *

**ChatGPT:**

要查询包含搜索词的书签，你可以使用通配符 '%' 来匹配搜索词的任意位置，以下是修改后的SQL语句：

```sql
SELECT * FROM bookmarks WHERE title LIKE '%Android%';
```

这将返回所有标题中包含 "Android" 的书签，无论其在标题的什么位置。

* * *

**You:**

请修改下面的python，让它支持上面的模糊搜索。 query = "SELECT \* FROM bookmarks WHERE title LIKE %s" print(f"query: {query}") cursor.execute(query, ("%" + title + "%",))

* * *

**ChatGPT:**

你可以按以下方式修改Python代码，将搜索词包含在单引号中，并将百分号作为字面字符串传递给SQL查询，如下所示：

```python
query = "SELECT * FROM bookmarks WHERE title LIKE '%{}%'".format(title)
print(f"query: {query}")
cursor.execute(query)
```

或者，如果你想保留查询参数化，可以使用下面的代码：

```python
query = "SELECT * FROM bookmarks WHERE title LIKE %s"
print(f"query: {query}")
cursor.execute(query, ('%{}%'.format(title),))
```

这两个代码块都使用了百分号作为通配符，用于匹配任意数量的字符，包括搜索词的前后字符。

* * *

**You:**

请改成按修改时间逆序

* * *

**ChatGPT:**

要按照修改时间的逆序对书签进行排序，你可以使用 `ORDER BY` 子句，并指定 `modified_time` 列的倒序排列，以下是修改后的SQL查询：

```sql
SELECT * FROM bookmarks WHERE title LIKE '%Android%' ORDER BY modified_time DESC;
```

这将返回包含 "Android" 搜索词的所有书签，并按其修改时间的逆序排列。

* * *

**You:**

请修改python代码，让它支持倒序

* * *

**ChatGPT:**

你可以在Python中添加 "DESC" 关键字来将查询结果按修改时间的逆序排序。以下是修改后的代码：

```python
query = "SELECT * FROM bookmarks WHERE title LIKE %s ORDER BY modified_time DESC"
print(f"query: {query}")
cursor.execute(query, ('%{}%'.format(title),))
```

在这个代码中，我们在查询的末尾添加了 `ORDER BY modified_time DESC`，以确保查询结果按照修改时间的逆序进行排序。