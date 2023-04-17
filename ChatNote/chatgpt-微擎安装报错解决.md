**You:**

微擎 安装报错：Deprecated: Function get\_magic\_quotes\_gpc() is deprecated in /www/wwwroot/wxgrouppay.fishyer.com/framework/bootstrap.inc.php on line 9

* * *

**ChatGPT:**

这个报错提示告诉你在文件`/www/wwwroot/wxgrouppay.fishyer.com/framework/bootstrap.inc.php`中的第9行使用了已经被弃用的函数`get_magic_quotes_gpc()`，这可能是由于你的 PHP 版本更新导致的。

解决方法：

在文件`/www/wwwroot/wxgrouppay.fishyer.com/framework/bootstrap.inc.php`中，将第9行：

```php
if (get_magic_quotes_gpc()) {
```

改为：

```php
if (ini_get('magic_quotes_gpc')) {
```

这样就可以解决这个问题。