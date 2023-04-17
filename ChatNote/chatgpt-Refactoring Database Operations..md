**You:**

帮我重构一下代码，将重复代码都包装成一个函数。 import mysql.connector from mysql.connector import pooling import redis redis\_pool = redis.ConnectionPool( host="127.0.0.1", port=6379, db=0, password="fishyer2850" ) mysql\_pool = pooling.MySQLConnectionPool( pool\_name="my\_pool", pool\_size=5, pool\_reset\_session=True, host="127.0.0.1", user="root", password="fishyer2850", database="testdb", ) def real\_check\_url(url): with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: cursor.execute("SELECT \* FROM bookmarks WHERE url=%s", (url,)) result = cursor.fetchone() if result is not None: with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.set(url, b"1") return True else: return False def add\_bookmark\_to\_db(bookmark): with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)" val = (bookmark.title, bookmark.url) cursor.execute(sql, val) connection.commit() if cursor.rowcount > 0: with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.set(bookmark.url, b"1") return True else: return False def delete\_bookmark\_by\_url(url): with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "DELETE FROM bookmarks WHERE url = %s" val = (url,) cursor.execute(sql, val) connection.commit() if cursor.rowcount > 0: with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.delete(url) return True else: return False def query\_bookmarks\_by\_title(title): bookmarks = \[\] with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: query = "SELECT \* FROM bookmarks WHERE title LIKE %s" cursor.execute(query, ("%" + title + "%",)) result = cursor.fetchall() for item in result: bookmark = Bookmark(item\[1\], item\[2\]) bookmarks.append(bookmark) return bookmarks

* * *

**ChatGPT:**

Here's a refactored version of the code that wraps the common database operations in a function called `execute_db_operation`. This function takes a query string, any necessary query parameters, and a function to execute after the query. The function returns the result of the function executed after the query.

```python
import mysql.connector
from mysql.connector import pooling
import redis

redis_pool = redis.ConnectionPool(
    host="127.0.0.1", port=6379, db=0, password="fishyer2850"
)

mysql_pool = pooling.MySQLConnectionPool(
    pool_name="my_pool",
    pool_size=5,
    pool_reset_session=True,
    host="127.0.0.1",
    user="root",
    password="fishyer2850",
    database="testdb",
)

def execute_db_operation(query, query_params, post_query_func):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            cursor.execute(query, query_params)
            result = post_query_func(cursor)
            connection.commit()
            return result

def real_check_url(url):
    def check_url(cursor):
        result = cursor.fetchone()
        if result is not None:
            with redis.Redis(connection_pool=redis_pool) as redis_conn:
                redis_conn.set(url, b"1")
            return True
        else:
            return False

    query = "SELECT * FROM bookmarks WHERE url=%s"
    return execute_db_operation(query, (url,), check_url)

def add_bookmark_to_db(bookmark):
    def add_bookmark(cursor):
        cursor.execute("INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)", (bookmark.title, bookmark.url))
        if cursor.rowcount > 0:
            with redis.Redis(connection_pool=redis_pool) as redis_conn:
                redis_conn.set(bookmark.url, b"1")
            return True
        else:
            return False

    return execute_db_operation(None, None, add_bookmark)

def delete_bookmark_by_url(url):
    def delete_bookmark(cursor):
        cursor.execute("DELETE FROM bookmarks WHERE url = %s", (url,))
        if cursor.rowcount > 0:
            with redis.Redis(connection_pool=redis_pool) as redis_conn:
                redis_conn.delete(url)
            return True
        else:
            return False

    return execute_db_operation(None, None, delete_bookmark)

def query_bookmarks_by_title(title):
    def build_bookmarks(cursor):
        bookmarks = []
        for item in cursor.fetchall():
            bookmark = Bookmark(item[1], item[2])
            bookmarks.append(bookmark)
        return bookmarks

    query = "SELECT * FROM bookmarks WHERE title LIKE %s"
    return execute_db_operation(query, ("%" + title + "%",), build_bookmarks)
```

In `execute_db_operation`, we first get a database connection from the connection pool and create a cursor to execute the query. After executing the query, we pass the cursor to the `post_query_func` parameter. This function takes the cursor and returns the result of the operation. We then commit the changes and return the result of the function.

For each of the original functions, we define a new function that takes the cursor as its parameter and returns the result of the operation. We pass these functions and the necessary parameters to `execute_db_operation`.

By wrapping the common database operations in a function, we reduce the amount of duplicate code and make it easier to add or modify functionality in the future.

* * *

**You:**

继续

* * *

**ChatGPT:**

Here's the continuation of the refactored code with the `Bookmark` class definition included:

```python
import mysql.connector
from mysql.connector import pooling
import redis

class Bookmark:
    def __init__(self, title, url):
        self.title = title
        self.url = url

redis_pool = redis.ConnectionPool(
    host="127.0.0.1", port=6379, db=0, password="fishyer2850"
)

mysql_pool = pooling.MySQLConnectionPool(
    pool_name="my_pool",
    pool_size=5,
    pool_reset_session=True,
    host="127.0.0.1",
    user="root",
    password="fishyer2850",
    database="testdb",
)

def execute_db_operation(query, query_params, post_query_func):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            cursor.execute(query, query_params)
            result = post_query_func(cursor)
            connection.commit()
            return result

def real_check_url(url):
    def check_url(cursor):
        result = cursor.fetchone()
        if result is not None:
            with redis.Redis(connection_pool=redis_pool) as redis_conn:
                redis_conn.set(url, b"1")
            return True
        else:
            return False

    query = "SELECT * FROM bookmarks WHERE url=%s"
    return execute_db_operation(query, (url,), check_url)

def add_bookmark_to_db(bookmark):
    def add_bookmark(cursor):
        cursor.execute("INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)", (bookmark.title, bookmark.url))
        if cursor.rowcount > 0:
            with redis.Redis(connection_pool=redis_pool) as redis_conn:
                redis_conn.set(bookmark.url, b"1")
            return True
        else:
            return False

    return execute_db_operation(None, None, add_bookmark)

def delete_bookmark_by_url(url):
    def delete_bookmark(cursor):
        cursor.execute("DELETE FROM bookmarks WHERE url = %s", (url,))
        if cursor.rowcount > 0:
            with redis.Redis(connection_pool=redis_pool) as redis_conn:
                redis_conn.delete(url)
            return True
        else:
            return False

    return execute_db_operation(None, None, delete_bookmark)

def query_bookmarks_by_title(title):
    def build_bookmarks(cursor):
        bookmarks = []
        for item in cursor.fetchall():
            bookmark = Bookmark(item[1], item[2])
            bookmarks.append(bookmark)
        return bookmarks

    query = "SELECT * FROM bookmarks WHERE title LIKE %s"
    return execute_db_operation(query, ("%" + title + "%",), build_bookmarks)
```

The `Bookmark` class definition is included at the beginning of the code. It's a simple class that has two attributes, `title` and `url`.

The `execute_db_operation` function is still the same. We define four new functions that handle the database operations for checking a URL, adding a bookmark to the database, deleting a bookmark by URL, and querying bookmarks by title.

For `real_check_url`, we define the `check_url` function that takes the cursor as its parameter and returns `True` if the result is not `None`. We execute a `SELECT` query that searches for the URL in the `bookmarks` table. We pass the query and the URL as parameters to `execute_db_operation`.

For `add_bookmark_to_db`, we define the `add_bookmark` function that takes the cursor as its parameter and returns `True` if the bookmark was added to the database. We execute an `INSERT IGNORE` query that inserts the bookmark into the `bookmarks`