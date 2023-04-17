**You:**

帮我写一个popclip插件，用google搜索选中的文本

* * *

**ChatGPT:**

好的，以下是一个简单的 PopClip 插件，用于将选中的文本在 Google 中进行搜索：

1.  打开终端应用程序。
    
2.  输入以下命令，创建一个新目录来存储插件文件：
    
    ```javascript
    mkdir -p ~/Library/Application\ Support/Popclip/extensions
    ```
    
3.  输入以下命令，进入该目录：
    
    ```javascript
    cd ~/Library/Application\ Support/Popclip/extensions
    ```
    
4.  输入以下命令，使用你喜欢的编辑器创建一个新文件，并将以下代码复制并粘贴到文件中：
    
    ```ruby
    #!/usr/bin/env ruby
    # encoding: utf-8
    
    # Google Search PopClip Extension
    # Created by ChatGPT
    
    # This extension allows you to search selected text on Google.
    
    require 'cgi'
    
    def search_google(selected_text)
      url = "https://www.google.com/search?q=#{CGI.escape(selected_text)}"
      system("open '#{url}'")
    end
    
    search_google(ENV['POPCLIP_TEXT'])
    ```
    
5.  保存文件并将其命名为 "GoogleSearch.popclipext"。
    
6.  右键单击该文件，并选择 "打开"。
    
7.  在弹出的对话框中，选择 "允许" 以安装插件。
    
8.  现在，当你选择文本并右键单击时，你应该可以看到一个 "Google Search" 的选项。选择该选项将在 Google 中搜索你选中的文本。
    

希望这个插件能够满足你的需求！

* * *

**You:**

请使用AppleScript语法

* * *

**ChatGPT:**

好的，以下是使用 AppleScript 语法的 PopClip 插件代码，用于在 Google 中搜索选中的文本：

```vbnet
on run argv
    set selectedText to item 1 of argv
    set encodedText to do shell script "/usr/bin/python -c 'import urllib, sys; print urllib.quote(sys.argv[1])' " & quoted form of selectedText
    set googleURL to "https://www.google.com/search?q=" & encodedText
    open location googleURL
end run
```

这个脚本会获取选中的文本，然后使用 Python 将其编码为 URL 安全格式。然后它将生成一个 Google 搜索的 URL，并使用 "open location" 命令在默认的浏览器中打开该 URL。

你可以将这个脚本复制并粘贴到 PopClip 插件编辑器中，然后为该插件指定一个名称和图标。当你选择文本并启用 PopClip 时，你应该会看到一个新的选项，允许你在 Google 中搜索选中的文本。