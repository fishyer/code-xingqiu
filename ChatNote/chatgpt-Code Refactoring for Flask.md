**You:**

帮我讲代码重构一下： from flask import Flask, request, jsonify, g import time from flask\_cors import CORS import mysql.connector from mysql.connector import pooling from pybloomfilter import BloomFilter import logging from logging.handlers import RotatingFileHandler import redis from datetime import datetime, timedelta, timezone tz\_utc\_8 = timezone(timedelta(hours=8)) import json redis\_pool = redis.ConnectionPool( host="127.0.0.1", port=6379, db=0, password="fishyer2850" ) mysql\_pool = pooling.MySQLConnectionPool( pool\_name="my\_pool", pool\_size=5, pool\_reset\_session=True, host="127.0.0.1", user="root", password="fishyer2850", database="testdb", ) app = Flask(\_\_name\_\_) # 支持中文 app.config\["JSON\_AS\_ASCII"\] = False # 允许所有域访问资源 CORS(app) app.logger.setLevel(logging.INFO) # 创建 RotatingFileHandler，将日志保存到 log 文件中，最多保存 5 个备份文件，每个备份文件最大为 1 MB handler = RotatingFileHandler("app.log", maxBytes=1024 \* 1024, backupCount=5) handler.setLevel(logging.INFO) formatter = logging.Formatter( "%(asctime)s %(levelname)s: %(message)s \[in %(pathname)s:%(lineno)d\]" ) handler.setFormatter(formatter) app.logger.addHandler(handler) # 在MySQL数据库中查询URL def real\_check\_url(url): with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: cursor.execute("SELECT \* FROM bookmarks WHERE url=%s", (url,)) result = cursor.fetchone() app.logger.info(f"result: {result}") if result is not None: # 如果MySQL中存在URL，则将其缓存到Redis中 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.set(url, b"1") return True else: return False @app.route('/') def hello(): return 'Hello, Check\_Url' # 在Redis中先查询URL是否存在，存在则返回code=0 @app.route("/check\_url", methods=\["POST"\]) def check\_url(): url = request.form.get("url") app.logger.info(f"check\_url url: {url}") # if url not in bf: # return resp(2, "布隆过滤器直接判断URL不存在") with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: result = redis\_conn.get(url) if result is None: if real\_check\_url(url): return resp(0, "数据库判断URL已存在") else: return resp(1, "数据库判断URL不存在") if result == b"1": return resp(0, "redis判断URL已存在") else: return resp(3, "redis判断URL不存在") # 添加书签的接口,添加成功则返回code=0 @app.route("/add\_bookmark", methods=\["POST"\]) def add\_bookmark(): title = request.form.get("title") url = request.form.get("url") app.logger.info(f"add\_bookmark title: {title} url: {url}") # 在MySQL数据库中查询URL with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)" val = (title, url) cursor.execute(sql, val) connection.commit() app.logger.info("执行SQL完毕，记录的 ID 是:%s", cursor.lastrowid) if cursor.rowcount > 0: # bf.add(url) # 在 Redis 中添加书签 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.set(url, b"1") return resp(0, "添加成功") else: return resp(1, "添加失败") # 根据 URL 删除书签的接口,删除成功则返回 code=0 @app.route("/delete\_bookmark\_by\_url", methods=\["POST"\]) def delete\_bookmark\_by\_url(): url = request.form.get("url") app.logger.info(f"delete\_bookmark\_by\_url url: {url}") # 在 MySQL 数据库中删除书签 with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "DELETE FROM bookmarks WHERE url = %s" val = (url,) cursor.execute(sql, val) connection.commit() app.logger.info("执行 SQL 完毕，受影响的行数是:%s", cursor.rowcount) if cursor.rowcount > 0: # 在 Redis 中删除书签 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.delete(url) app.logger.info(f"在 Redis 中删除书签 {url}") return resp(0, "删除成功") else: return resp(1, "删除失败") # 根据title查询书签信息 @app.route("/bookmarks", methods=\["GET"\]) def get\_bookmarks(): title = request.args.get("title", "") app.logger.info(f"get\_bookmarks title: {title}") bookmarks = \[\] with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: query = "SELECT \* FROM bookmarks WHERE title LIKE %s" cursor.execute(query, ("%" + title + "%",)) result = cursor.fetchall() for item in result: bookmark = {"id": item\[0\], "title": item\[1\], "url": item\[2\]} bookmarks.append(bookmark) app.logger.info(f"get\_bookmarks len: {len(bookmarks)}") return resp(0, "查询成功", list\_wrapper(bookmarks)) def list\_wrapper(li): return {"list": li, "len": len(li)} def resp(code, msg, data={}): return jsonify({"code": code, "message": msg, "data": data}) @app.before\_request def before\_request(): g.start\_time = time.time() @app.after\_request def after\_request(response): end\_time = time.time() elapsed\_time = end\_time - g.start\_time app.logger.info( f"Request {request.method} {request.path} took {elapsed\_time:.5f} seconds" ) app.logger.info(f"Request data: {json.dumps(getRequestAllData(request), indent=2, ensure\_ascii=False)}") app.logger.info(f"Response data: {response.get\_data().decode('utf-8')}") return response # 创建UTC+8北京时间的datetime的标准时间格式 def getNowDateTime(): nowTime = datetime.now(tz=tz\_utc\_8).strftime("%Y-%m-%d %H:%M:%S") return nowTime # 获取请求的所有数据 def getRequestAllData(request): m={} # 基本信息 m\['time'\]=getNowDateTime() m\['method'\]=request.method m\['url'\]=request.url m\['content\_type'\]=request.headers.get("content\_type") # 具体参数 if request.args is not None: m\['args'\]=request.args.to\_dict() if m\['content\_type'\] is not None: if 'application/json' in m\['content\_type'\]: m\['json'\]=request.get\_json() if 'application/x-www-form-urlencoded' in m\['content\_type'\]: m\['form'\]=request.form.to\_dict() if 'multipart/form-data' in m\['content\_type'\]: m\['form'\]=request.form.to\_dict() if request.files is not None: m\['files'\]=str(request.files.to\_dict()) for key in request.files: value=request.files.get(key) return m # 查询数据库函数 def query\_sql(sql, val): with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: cursor.execute(sql, val) result = cursor.fetchall() return result # 启动Flask应用程序 # nohup python3 /Users/yutianran/MyGithub/MyCode/app\_check\_url.py >> app\_check\_url\_\`date +'%Y-%m-%d'\`.log 2>&1 & if \_\_name\_\_ == "\_\_main\_\_": print("启动程序: http://localhost:5000") app.run(host='0.0.0.0', port=5000,debug=True)

* * *

**ChatGPT:**

以下是代码重构后的版本：

```python
import json
import logging
import time
from datetime import datetime, timedelta, timezone
from logging.handlers import RotatingFileHandler

import mysql.connector
import redis
from flask import Flask, g, jsonify, request
from flask_cors import CORS
from mysql.connector import pooling
from pybloomfilter import BloomFilter


app = Flask(__name__)
CORS(app)

# Database connection pools
redis_pool = redis.ConnectionPool(
    host="127.0.0.1",
    port=6379,
    db=0,
    password="fishyer2850"
)

mysql_pool = pooling.MySQLConnectionPool(
    pool_name="my_pool",
    pool_size=5,
    pool_reset_session=True,
    host="127.0.0.1",
    user="root",
    password="fishyer2850",
    database="testdb"
)

# Logging configuration
app.logger.setLevel(logging.INFO)
handler = RotatingFileHandler("app.log", maxBytes=1024 * 1024, backupCount=5)
handler.setLevel(logging.INFO)
formatter = logging.Formatter(
    "%(asctime)s %(levelname)s: %(message)s [in %(pathname)s:%(lineno)d]"
)
handler.setFormatter(formatter)
app.logger.addHandler(handler)

# Timezone configuration
tz_utc_8 = timezone(timedelta(hours=8))


def get_request_data(request):
    """
    Get all request data in a dictionary format.
    """
    data = {
        "time": datetime.now(tz=tz_utc_8).strftime("%Y-%m-%d %H:%M:%S"),
        "method": request.method,
        "url": request.url,
        "content_type": request.headers.get("content_type"),
    }

    if request.args:
        data["args"] = request.args.to_dict()

    if data["content_type"]:
        if "application/json" in data["content_type"]:
            data["json"] = request.get_json()
        elif "application/x-www-form-urlencoded" in data["content_type"] or "multipart/form-data" in data["content_type"]:
            data["form"] = request.form.to_dict()

    if request.files:
        data["files"] = str(request.files.to_dict())

    return data


@app.before_request
def before_request():
    g.start_time = time.time()


@app.after_request
def after_request(response):
    end_time = time.time()
    elapsed_time = end_time - g.start_time
    app.logger.info(
        f"Request {request.method} {request.path} took {elapsed_time:.5f} seconds"
    )
    app.logger.info(f"Request data: {json.dumps(get_request_data(request), indent=2, ensure_ascii=False)}")
    app.logger.info(f"Response data: {response.get_data().decode('utf-8')}")
    return response


def resp(code, msg, data=None):
    """
    Generate a JSON response with given code, message, and optional data.
    """
    return jsonify({"code": code, "message": msg, "data": data})


def list_wrapper(li):
    """
    Wrap a list in a dictionary that includes its length.
    """
    return {"list": li, "len": len(li)}


def query_mysql(sql, val):
    """
    Execute a SQL query against the MySQL database pool.
    """
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            cursor.execute(sql, val)
            result = cursor.fetchall()
            return result


def real_check_url(url):
    """
    Check if the URL exists in the bookmarks database and cache the result in Redis.
    """
    with mysql_pool.get_connection() as
```

* * *

**You:**

请使用面向对象的思想，并且记得加上单元测试

* * *

**ChatGPT:**

好的，以下是使用面向对象的重构版本，同时包含了单元测试：

```python
import json
import logging
import time
from datetime import datetime, timedelta, timezone
from logging.handlers import RotatingFileHandler

import mysql.connector
import redis
from flask import Flask, g, jsonify, request
from flask_cors import CORS
from mysql.connector import pooling
from pybloomfilter import BloomFilter


class CheckUrlApp:
    def __init__(self):
        self.app = Flask(__name__)
        CORS(self.app)

        # Database connection pools
        self.redis_pool = redis.ConnectionPool(
            host="127.0.0.1",
            port=6379,
            db=0,
            password="fishyer2850"
        )

        self.mysql_pool = pooling.MySQLConnectionPool(
            pool_name="my_pool",
            pool_size=5,
            pool_reset_session=True,
            host="127.0.0.1",
            user="root",
            password="fishyer2850",
            database="testdb"
        )

        # Logging configuration
        self.app.logger.setLevel(logging.INFO)
        handler = RotatingFileHandler("app.log", maxBytes=1024 * 1024, backupCount=5)
        handler.setLevel(logging.INFO)
        formatter = logging.Formatter(
            "%(asctime)s %(levelname)s: %(message)s [in %(pathname)s:%(lineno)d]"
        )
        handler.setFormatter(formatter)
        self.app.logger.addHandler(handler)

        # Timezone configuration
        self.tz_utc_8 = timezone(timedelta(hours=8))

        self._register_routes()

    def _register_routes(self):
        self.app.route('/')(self.hello)
        self.app.route('/check_url', methods=["POST"])(self.check_url)
        self.app.route('/add_bookmark', methods=["POST"])(self.add_bookmark)
        self.app.route('/delete_bookmark_by_url', methods=["POST"])(self.delete_bookmark_by_url)
        self.app.route('/bookmarks', methods=["GET"])(self.get_bookmarks)

    def run(self, host='0.0.0.0', port=5000, debug=False):
        self.app.run(host=host, port=port, debug=debug)

    def hello(self):
        return 'Hello, Check_Url'

    def check_url(self):
        url = request.form.get("url")
        self.app.logger.info(f"check_url url: {url}")
        with redis.Redis(connection_pool=self.redis_pool) as redis_conn:
            result = redis_conn.get(url)
            if result is None:
                if self._real_check_url(url):
                    return resp(0, "数据库判断URL已存在")
                else:
                    return resp(1, "数据库判断URL不存在")
            if result == b"1":
                return resp(0, "redis判断URL已存在")
            else:
                return resp(2, "redis判断URL不存在")

    def _real_check_url(self, url):
        with self.mysql_pool.get_connection() as connection:
            with connection.cursor() as cursor:
                cursor.execute("SELECT * FROM bookmarks WHERE url=%s", (url,))
                result = cursor.fetchone()
                self.app.logger.info(f"result: {result}")
                if result is not None:
                    with redis.Redis(connection_pool=self.redis_pool) as redis_conn:
                        redis_conn.set(url, b"1")
                    return True
                else:
                    return False

    def add_bookmark(self):
        title = request.form.get("title")
        url = request.form.get("url")
        self.app.logger.info(f"add_bookmark title: {title
```