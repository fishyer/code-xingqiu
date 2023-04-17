**You:**

请将书签列表转成title列表 def real\_get\_all\_bookmarks(): bookmarks = \[\] with mysql\_pool.connection() as connection: with connection.cursor() as cursor: query = "SELECT \* FROM bookmarks ORDER BY update\_time DESC" print(f"query: {query}") cursor.execute(query) result = cursor.fetchall() for item in result: bookmark = {"id": item\[0\], "title": item\[1\], "url": item\[2\]} bookmarks.append(bookmark) print(f"get\_all\_bookmarks len: {len(bookmarks)}") return bookmarks real\_get\_all\_bookmarks()

* * *

**ChatGPT:**

以下是将书签列表转换为标题列表的代码：

def get\_bookmark\_titles(): bookmarks = real\_get\_all\_bookmarks() titles = \[bookmark\['title'\] for bookmark in bookmarks\] return titles

get\_bookmark\_titles()

此函数将调用实际获取所有书签的函数 `real_get_all_bookmarks()`，并创建一个标题列表，其中包含书签列表中每个书签的标题。返回标题列表。

* * *

**You:**

recent100=get\_bookmark\_titles()\[:100\] print(recent100) 请用join实现分行打印

* * *

**ChatGPT:**

以下是使用 `join` 实现分行打印的代码：

recent100 = get\_bookmark\_titles()\[:100\] print('\\n'.join(recent100))

此代码将最近 100 个书签的标题存储在 `recent100` 列表中，然后使用 `join` 函数将它们连接成一个字符串，并在每个标题之间添加换行符 `\n`，以便每个标题在单独的行上打印。最后，使用 `print` 函数将这个字符串打印出来。