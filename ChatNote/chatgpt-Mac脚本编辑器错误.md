**You:**

在mac的脚本编辑器中，运行下面的代码报错 using terms from application "PopClip" on run with parameters theString set theString to do shell script "echo " & quoted form of theString & " | perl -pe 's/(?<!^)\[\[:upper:\]\]/\_&/g; s/(\[\[:lower:\]\\d\])(\[\[:upper:\]\])/$1\_$2/g; s/\[-\\s\]+/\_/g; s/^\_+//; s/\_+$//; tr/A-Z/a-z/" return theString end run end using terms from 报错：语法错误 预期是“given”、“with”、“without”、其他参数名称等等，却找到标识符

* * *

**ChatGPT:**

这个错误提示表明你的脚本中的语法错误，可能是由于缺少某些关键字或语句导致的。

根据你提供的代码，我注意到 `using terms from` 和 `end using terms from` 之间的内容应该是一个应用程序的词汇定义。但是你的代码中缺少了这个定义的应用程序名称。因此，你需要指定一个应用程序名称来定义这个语句块所使用的词汇。

例如，如果你想要使用 Safari 应用程序中的词汇，你可以这样编写代码：

```sql
using terms from application "Safari"
    -- 在这里编写脚本代码
end using terms from
```

请根据你的实际需求指定正确的应用程序名称，然后再次运行你的脚本。