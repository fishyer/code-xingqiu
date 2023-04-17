**You:**

请重构下面的代码： # 添加书签的接口,添加成功则返回code=0 @app.route("/add\_bookmark", methods=\["POST"\]) def add\_bookmark(): title = request.form.get("title") url = request.form.get("url") app.logger.info(f"add\_bookmark title: {title} url: {url}") # 在MySQL数据库中查询URL with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)" val = (title, url) cursor.execute(sql, val) connection.commit() app.logger.info("执行SQL完毕，记录的 ID 是:%s", cursor.lastrowid) if cursor.rowcount > 0: # bf.add(url) # 在 Redis 中添加书签 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.set(url, b"1") return resp(0, "添加成功") else: return resp(1, "添加失败") # 根据 URL 删除书签的接口,删除成功则返回 code=0 @app.route("/delete\_bookmark\_by\_url", methods=\["POST"\]) def delete\_bookmark\_by\_url(): url = request.form.get("url") app.logger.info(f"delete\_bookmark\_by\_url url: {url}") # 在 MySQL 数据库中删除书签 with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "DELETE FROM bookmarks WHERE url = %s" val = (url,) cursor.execute(sql, val) connection.commit() app.logger.info("执行 SQL 完毕，受影响的行数是:%s", cursor.rowcount) if cursor.rowcount > 0: # 在 Redis 中删除书签 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.delete(url) app.logger.info(f"在 Redis 中删除书签 {url}") return resp(0, "删除成功") else: return resp(1, "删除失败") 将数据库相关操作包装起来，只需要传入具体sql就可以执行并返回sql执行结果。

* * *

**ChatGPT:**

可以考虑将数据库相关操作封装为一个函数，传入具体的 SQL 语句和参数，执行并返回执行结果。同时，还可以将 Redis 相关操作也封装为一个函数，传入具体的 Redis 连接和参数，执行并返回执行结果。下面是重构后的代码：

```python
# 数据库操作函数
def execute_sql(sql, val):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            cursor.execute(sql, val)
            connection.commit()
            return cursor.rowcount, cursor.lastrowid

# Redis 操作函数
def execute_redis(redis_conn, method, key, value=None):
    if method == "get":
        return redis_conn.get(key)
    elif method == "set":
        return redis_conn.set(key, value)
    elif method == "delete":
        return redis_conn.delete(key)

# 添加书签的接口,添加成功则返回code=0
@app.route("/add_bookmark", methods=["POST"])
def add_bookmark():
    title = request.form.get("title")
    url = request.form.get("url")
    app.logger.info(f"add_bookmark title: {title} url: {url}")
    
    # 在 MySQL 数据库中插入书签
    sql = "INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)"
    val = (title, url)
    rowcount, lastrowid = execute_sql(sql, val)
    app.logger.info(f"执行SQL完毕，记录的 ID 是:{lastrowid}")
    
    if rowcount > 0:
        # 在 Redis 中添加书签
        with redis.Redis(connection_pool=redis_pool) as redis_conn:
            execute_redis(redis_conn, "set", url, b"1")
        return resp(0, "添加成功")
    else:
        return resp(1, "添加失败")

# 根据 URL 删除书签的接口,删除成功则返回 code=0
@app.route("/delete_bookmark_by_url", methods=["POST"])
def delete_bookmark_by_url():
    url = request.form.get("url")
    app.logger.info(f"delete_bookmark_by_url url: {url}")
    
    # 在 MySQL 数据库中删除书签
    sql = "DELETE FROM bookmarks WHERE url = %s"
    val = (url,)
    rowcount, _ = execute_sql(sql, val)
    app.logger.info(f"执行 SQL 完毕，受影响的行数是:{rowcount}")
    
    if rowcount > 0:
        # 在 Redis 中删除书签
        with redis.Redis(connection_pool=redis_pool) as redis_conn:
            execute_redis(redis_conn, "delete", url)
            app.logger.info(f"在 Redis 中删除书签 {url}")
        return resp(0, "删除成功")
    else:
        return resp(1, "删除失败")
```

通过上述重构，可以将数据库和 Redis 相关的操作进行了封装，提高了代码的可重用性和可维护性，同时也减少了代码重复。

* * *

**You:**

将这个也封装一下： # 根据title查询书签信息 @app.route("/bookmarks", methods=\["GET"\]) def get\_bookmarks(): title = request.args.get("title", "") app.logger.info(f"get\_bookmarks title: {title}") bookmarks = \[\] with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: query = "SELECT \* FROM bookmarks WHERE title LIKE %s" cursor.execute(query, ("%" + title + "%",)) result = cursor.fetchall() for item in result: bookmark = {"id": item\[0\], "title": item\[1\], "url": item\[2\]} bookmarks.append(bookmark) app.logger.info(f"get\_bookmarks len: {len(bookmarks)}") return resp(0, "查询成功", list\_wrapper(bookmarks))

* * *

**ChatGPT:**

可以将查询数据库的部分封装成一个函数，传入参数后执行查询并返回结果。下面是重构后的代码：

```python
# 查询数据库函数
def query_sql(sql, val):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            cursor.execute(sql, val)
            result = cursor.fetchall()
            return result

# 根据title查询书签信息
@app.route("/bookmarks", methods=["GET"])
def get_bookmarks():
    title = request.args.get("title", "")
    app.logger.info(f"get_bookmarks title: {title}")
    bookmarks = []
    
    # 在 MySQL 数据库中查询书签信息
    sql = "SELECT * FROM bookmarks WHERE title LIKE %s"
    val = ("%" + title + "%",)
    result = query_sql(sql, val)
    
    for item in result:
        bookmark = {"id": item[0], "title": item[1], "url": item[2]}
        bookmarks.append(bookmark)
        
    app.logger.info(f"get_bookmarks len: {len(bookmarks)}")
    return resp(0, "查询成功", list_wrapper(bookmarks))
```

通过上述重构，可以将查询数据库的部分封装成一个函数，提高了代码的可重用性和可维护性，同时也减少了代码重复。

* * *

**You:**

for item in result: bookmark = {"id": item\[0\], "title": item\[1\], "url": item\[2\]} bookmarks.append(bookmark) 改成用map一行实现

* * *

**ChatGPT:**

可以使用 `map()` 函数和 `lambda` 表达式来实现将查询结果映射为字典的操作，下面是重构后的代码：

```python
# 根据title查询书签信息
@app.route("/bookmarks", methods=["GET"])
def get_bookmarks():
    title = request.args.get("title", "")
    app.logger.info(f"get_bookmarks title: {title}")
    
    # 在 MySQL 数据库中查询书签信息
    sql = "SELECT * FROM bookmarks WHERE title LIKE %s"
    val = ("%" + title + "%",)
    result = query_sql(sql, val)
    
    # 将查询结果映射为字典
    bookmarks = list(map(lambda item: {"id": item[0], "title": item[1], "url": item[2]}, result))
    
    app.logger.info(f"get_bookmarks len: {len(bookmarks)}")
    return resp(0, "查询成功", list_wrapper(bookmarks))
```

通过使用 `map()` 函数和 `lambda` 表达式，可以将查询结果映射为字典的操作实现为一行代码，提高了代码的简洁性和可读性。