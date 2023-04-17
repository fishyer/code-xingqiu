**You:**

请使用面向对象的思想，重构下面的代码，拆分成数据模型层、数据库操作层、工具类层、业务逻辑层、接口控制器层、配置层，同时多用函数将重复的代码封装起来。 from flask import Flask, request, jsonify, g import time from flask\_cors import CORS import mysql.connector from mysql.connector import pooling from pybloomfilter import BloomFilter import logging from logging.handlers import RotatingFileHandler import redis from datetime import datetime, timedelta, timezone tz\_utc\_8 = timezone(timedelta(hours=8)) import json redis\_pool = redis.ConnectionPool( host="127.0.0.1", port=6379, db=0, password="fishyer2850" ) mysql\_pool = pooling.MySQLConnectionPool( pool\_name="my\_pool", pool\_size=5, pool\_reset\_session=True, host="127.0.0.1", user="root", password="fishyer2850", database="testdb", ) app = Flask(\_\_name\_\_) # 支持中文 app.config\["JSON\_AS\_ASCII"\] = False # 允许所有域访问资源 CORS(app) app.logger.setLevel(logging.INFO) # 创建 RotatingFileHandler，将日志保存到 log 文件中，最多保存 5 个备份文件，每个备份文件最大为 1 MB handler = RotatingFileHandler("app.log", maxBytes=1024 \* 1024, backupCount=5) handler.setLevel(logging.INFO) formatter = logging.Formatter( "%(asctime)s %(levelname)s: %(message)s \[in %(pathname)s:%(lineno)d\]" ) handler.setFormatter(formatter) app.logger.addHandler(handler) # 在MySQL数据库中查询URL def real\_check\_url(url): with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: cursor.execute("SELECT \* FROM bookmarks WHERE url=%s", (url,)) result = cursor.fetchone() app.logger.info(f"result: {result}") if result is not None: # 如果MySQL中存在URL，则将其缓存到Redis中 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.set(url, b"1") return True else: return False @app.route('/') def hello(): return 'Hello, Check\_Url' # 在Redis中先查询URL是否存在，存在则返回code=0 @app.route("/check\_url", methods=\["POST"\]) def check\_url(): url = request.form.get("url") app.logger.info(f"check\_url url: {url}") # if url not in bf: # return resp(2, "布隆过滤器直接判断URL不存在") with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: result = redis\_conn.get(url) if result is None: if real\_check\_url(url): return resp(0, "数据库判断URL已存在") else: return resp(1, "数据库判断URL不存在") if result == b"1": return resp(0, "redis判断URL已存在") else: return resp(3, "redis判断URL不存在") # 添加书签的接口,添加成功则返回code=0 @app.route("/add\_bookmark", methods=\["POST"\]) def add\_bookmark(): title = request.form.get("title") url = request.form.get("url") app.logger.info(f"add\_bookmark title: {title} url: {url}") # 在MySQL数据库中查询URL with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)" val = (title, url) cursor.execute(sql, val) connection.commit() app.logger.info("执行SQL完毕，记录的 ID 是:%s", cursor.lastrowid) if cursor.rowcount > 0: # bf.add(url) # 在 Redis 中添加书签 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.set(url, b"1") return resp(0, "添加成功") else: return resp(1, "添加失败") # 根据 URL 删除书签的接口,删除成功则返回 code=0 @app.route("/delete\_bookmark\_by\_url", methods=\["POST"\]) def delete\_bookmark\_by\_url(): url = request.form.get("url") app.logger.info(f"delete\_bookmark\_by\_url url: {url}") # 在 MySQL 数据库中删除书签 with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: sql = "DELETE FROM bookmarks WHERE url = %s" val = (url,) cursor.execute(sql, val) connection.commit() app.logger.info("执行 SQL 完毕，受影响的行数是:%s", cursor.rowcount) if cursor.rowcount > 0: # 在 Redis 中删除书签 with redis.Redis(connection\_pool=redis\_pool) as redis\_conn: redis\_conn.delete(url) app.logger.info(f"在 Redis 中删除书签 {url}") return resp(0, "删除成功") else: return resp(1, "删除失败") # 根据title查询书签信息 @app.route("/bookmarks", methods=\["GET"\]) def get\_bookmarks(): title = request.args.get("title", "") app.logger.info(f"get\_bookmarks title: {title}") bookmarks = \[\] with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: query = "SELECT \* FROM bookmarks WHERE title LIKE %s" cursor.execute(query, ("%" + title + "%",)) result = cursor.fetchall() for item in result: bookmark = {"id": item\[0\], "title": item\[1\], "url": item\[2\]} bookmarks.append(bookmark) app.logger.info(f"get\_bookmarks len: {len(bookmarks)}") return resp(0, "查询成功", list\_wrapper(bookmarks)) def list\_wrapper(li): return {"list": li, "len": len(li)} def resp(code, msg, data={}): return jsonify({"code": code, "message": msg, "data": data}) @app.before\_request def before\_request(): g.start\_time = time.time() @app.after\_request def after\_request(response): end\_time = time.time() elapsed\_time = end\_time - g.start\_time app.logger.info( f"Request {request.method} {request.path} took {elapsed\_time:.5f} seconds" ) app.logger.info(f"Request data: {json.dumps(getRequestAllData(request), indent=2, ensure\_ascii=False)}") app.logger.info(f"Response data: {response.get\_data().decode('utf-8')}") return response # 创建UTC+8北京时间的datetime的标准时间格式 def getNowDateTime(): nowTime = datetime.now(tz=tz\_utc\_8).strftime("%Y-%m-%d %H:%M:%S") return nowTime # 获取请求的所有数据 def getRequestAllData(request): m={} # 基本信息 m\['time'\]=getNowDateTime() m\['method'\]=request.method m\['url'\]=request.url m\['content\_type'\]=request.headers.get("content\_type") # 具体参数 if request.args is not None: m\['args'\]=request.args.to\_dict() if m\['content\_type'\] is not None: if 'application/json' in m\['content\_type'\]: m\['json'\]=request.get\_json() if 'application/x-www-form-urlencoded' in m\['content\_type'\]: m\['form'\]=request.form.to\_dict() if 'multipart/form-data' in m\['content\_type'\]: m\['form'\]=request.form.to\_dict() if request.files is not None: m\['files'\]=str(request.files.to\_dict()) for key in request.files: value=request.files.get(key) return m # 查询数据库函数 def query\_sql(sql, val): with mysql\_pool.get\_connection() as connection: with connection.cursor() as cursor: cursor.execute(sql, val) result = cursor.fetchall() return result # 启动Flask应用程序 # nohup python3 /Users/yutianran/MyGithub/MyCode/app\_check\_url.py >> app\_check\_url\_\`date +'%Y-%m-%d'\`.log 2>&1 & if \_\_name\_\_ == "\_\_main\_\_": print("启动程序: http://localhost:5000") app.run(host='0.0.0.0', port=5000,debug=True)

* * *

**ChatGPT:**

由于代码量较大，这里将不同层次的重构放到不同的代码块中，以便于理解。

### 数据模型层

在这个例子中，数据模型层指的是 bookmark 对象的定义，包括 title, url 等属性。这个对象将会在数据库操作层和业务逻辑层中被使用。我们将其定义在 `models.py` 文件中。

```python
class Bookmark:
    def __init__(self, title, url):
        self.title = title
        self.url = url
```

### 数据库操作层

数据库操作层是指所有和数据库有关的操作，包括连接池的设置、查询 SQL 语句的执行、MySQL 和 Redis 的交互等。我们将其定义在 `dao.py` 文件中。

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

def real_check_url(url):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            cursor.execute("SELECT * FROM bookmarks WHERE url=%s", (url,))
            result = cursor.fetchone()
            if result is not None:
                with redis.Redis(connection_pool=redis_pool) as redis_conn:
                    redis_conn.set(url, b"1")
                return True
            else:
                return False

def add_bookmark_to_db(bookmark):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            sql = "INSERT IGNORE INTO bookmarks (title, url) VALUES (%s, %s)"
            val = (bookmark.title, bookmark.url)
            cursor.execute(sql, val)
            connection.commit()
            if cursor.rowcount > 0:
                with redis.Redis(connection_pool=redis_pool) as redis_conn:
                    redis_conn.set(bookmark.url, b"1")
                return True
            else:
                return False

def delete_bookmark_by_url(url):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            sql = "DELETE FROM bookmarks WHERE url = %s"
            val = (url,)
            cursor.execute(sql, val)
            connection.commit()
            if cursor.rowcount > 0:
                with redis.Redis(connection_pool=redis_pool) as redis_conn:
                    redis_conn.delete(url)
                return True
            else:
                return False

def query_bookmarks_by_title(title):
    bookmarks = []
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            query = "SELECT * FROM bookmarks WHERE title LIKE %s"
            cursor.execute(query, ("%" + title + "%",))
            result = cursor.fetchall()
            for item in result:
                bookmark = Bookmark(item[1], item[2])
                bookmarks.append(bookmark)
    return bookmarks
```

### 工具类层

工具类层包含了一些通用的函数和类，比如获取当前时间、处理 API 返回数据的 `resp` 函数、封装重复代码的 `list_wrapper` 函数和获取请求所有数据的 \`getRequestAllData

* * *

**You:**

继续

* * *

**ChatGPT:**

函数。我们将其定义在 `utils.py` 文件中。

```python
import logging
from logging.handlers import RotatingFileHandler
from datetime import datetime, timedelta, timezone
tz_utc_8 = timezone(timedelta(hours=8))
import json
from flask import jsonify, request

# 创建 RotatingFileHandler，将日志保存到 log 文件中，最多保存 5 个备份文件，每个备份文件最大为 1 MB
handler = RotatingFileHandler("app.log", maxBytes=1024 * 1024, backupCount=5)
handler.setLevel(logging.INFO)
formatter = logging.Formatter(
    "%(asctime)s %(levelname)s: %(message)s [in %(pathname)s:%(lineno)d]"
)
handler.setFormatter(formatter)
logging.getLogger().addHandler(handler)

# 在MySQL数据库中查询URL
def real_check_url(url):
    # ...

# 查询数据库函数
def query_sql(sql, val):
    with mysql_pool.get_connection() as connection:
        with connection.cursor() as cursor:
            cursor.execute(sql, val)
            result = cursor.fetchall()
            return result

# 将返回结果封装成标准的 JSON 格式
def resp(code, msg, data={}):
    return jsonify({"code": code, "message": msg, "data": data})

# 处理列表数据
def list_wrapper(li):
    return {"list": li, "len": len(li)}

# 获取请求的所有数据
def getRequestAllData(request):
    m={}
    # 基本信息
    m['time']=getNowDateTime()
    m['method']=request.method
    m['url']=request.url
    m['content_type']=request.headers.get("content_type")
    # 具体参数
    if request.args is not None:
        m['args']=request.args.to_dict()
    if m['content_type'] is not None:
        if 'application/json' in m['content_type']:
            m['json']=request.get_json()
        if 'application/x-www-form-urlencoded' in m['content_type']:
            m['form']=request.form.to_dict()
        if 'multipart/form-data' in m['content_type']:
            m['form']=request.form.to_dict()
    if request.files is not None:
        m['files']=str(request.files.to_dict())
        for key in request.files:
            value=request.files.get(key)
    return m

# 创建UTC+8北京时间的datetime的标准时间格式
def getNowDateTime():
    nowTime = datetime.now(tz=tz_utc_8).strftime("%Y-%m-%d %H:%M:%S")
    return nowTime
```

### 业务逻辑层

业务逻辑层主要包含了与具体业务相关的代码，比如查询是否存在 URL 的逻辑、添加书签的逻辑、删除书签的逻辑、查询书签的逻辑。我们将其定义在 `services.py` 文件中。

```python
from dao import *
from models import Bookmark

def check_url(url):
    with redis.Redis(connection_pool=redis_pool) as redis_conn:
        result = redis_conn.get(url)
        if result is None:
            if real_check_url(url):
                return resp(0, "数据库判断URL已存在")
            else:
                return resp(1, "数据库判断URL不存在")
        if result == b"1":
            return resp(0, "redis判断URL已存在")
        else:
            return resp(3, "redis判断URL不存在")

def add_bookmark(title
```

* * *

**You:**

继续

* * *

**ChatGPT:**

```python
from dao import *
from models import Bookmark

def check_url(url):
    with redis.Redis(connection_pool=redis_pool) as redis_conn:
        result = redis_conn.get(url)
        if result is None:
            if real_check_url(url):
                redis_conn.set(url, b"1")
                return resp(0, "数据库判断URL已存在")
            else:
                return resp(1, "数据库判断URL不存在")
        if result == b"1":
            return resp(0, "redis判断URL已存在")
        else:
            return resp(3, "redis判断URL不存在")

def add_bookmark(title, url):
    bookmark = Bookmark(title=title, url=url)
    bookmark.save()
    return resp(0, "添加成功")

def delete_bookmark_by_url(url):
    bookmark = Bookmark.query.filter_by(url=url).first()
    if bookmark:
        bookmark.delete()
        with redis.Redis(connection_pool=redis_pool) as redis_conn:
            redis_conn.delete(url)
        return resp(0, "删除成功")
    else:
        return resp(1, "删除失败")

def get_bookmarks(title):
    bookmarks = Bookmark.query.filter(Bookmark.title.like(f"%{title}%")).all()
    bookmark_list = []
    for bookmark in bookmarks:
        bookmark_list.append({
            "id": bookmark.id,
            "title": bookmark.title,
            "url": bookmark.url
        })
    return resp(0, "查询成功", list_wrapper(bookmark_list))
```

### 接口控制器层

接口控制器层主要负责处理请求和响应。我们将其定义在 `views.py` 文件中。

```python
from flask import Blueprint, request
from services import *

blueprint = Blueprint('check_url', __name__)

@blueprint.route('/')
def hello():
    return 'Hello, Check_Url'

@blueprint.route("/check_url", methods=["POST"])
def check_url_view():
    url = request.form.get("url")
    return check_url(url)

@blueprint.route("/add_bookmark", methods=["POST"])
def add_bookmark_view():
    title = request.form.get("title")
    url = request.form.get("url")
    return add_bookmark(title, url)

@blueprint.route("/delete_bookmark_by_url", methods=["POST"])
def delete_bookmark_by_url_view():
    url = request.form.get("url")
    return delete_bookmark_by_url(url)

@blueprint.route("/bookmarks", methods=["GET"])
def get_bookmarks_view():
    title = request.args.get("title", "")
    return get_bookmarks(title)
```

### 配置层

配置层主要负责项目的配置，包括 Flask、MySQL、Redis 等。我们将其定义在 `config.py` 文件中。

```python
import os

class Config(object):
    DEBUG = False
    TESTING = False
    CSRF_ENABLED = True
    SECRET_KEY = os.environ.get("SECRET_KEY", "secret-key")
    SQLALCHEMY_DATABASE_URI = os.environ.get("DATABASE_URL")
    SQLALCHEMY_TRACK_MODIFICATIONS = False
    REDIS_HOST = "127.0.0.1"
    REDIS_PORT = 6379
    REDIS_DB = 0
    REDIS_PASSWORD = "fishyer2850"
    LOGGING_FORMAT = '%(asctime)s %(levelname)s: %(message)s [in %(pathname)s:%(lineno)d]'
    LOGGING_LOCATION = 'app.log'
    LOGGING_LEVEL = 'INFO'

class ProductionConfig(Config):
    DEBUG = False

class StagingConfig(Config):
    DEVELOPMENT = True
```