**You:**

VSCode开发Flutter时，怎么将Android真机从USB连接模式改成无线连接模式？

* * *

**ChatGPT:**

要将Android真机从USB连接模式改为无线连接模式，可以按照以下步骤进行操作：

1.  确保你的Android设备和电脑在同一个Wi-Fi网络中。
    
2.  在VSCode中，点击左侧面板中的“调试”图标，然后点击“启动调试”按钮，在弹出的下拉菜单中选择“Flutter”（如果你还没有配置Flutter的调试环境，请先按照官方文档进行配置）。
    
3.  点击“启动调试”按钮之后，你将看到一个名为“launch.json”的文件被打开。在这个文件中，找到“configurations”部分，并添加以下代码：
    

```bash
{
    "name": "Flutter (wireless device)",
    "request": "launch",
    "type": "dart",
    "deviceId": "wireless",
    "flutterMode": "profile",
    "cwd": "${workspaceFolder}",
    "program": "lib/main.dart"
}
```

其中，“deviceId”属性的值应该是“wireless”。

4.  连接你的Android设备到电脑上，并打开终端（Windows用户可以使用PowerShell）。
    
5.  在终端中输入以下命令，将设备连接到电脑的adb服务：
    

```yaml
adb tcpip 5555
```

6.  然后，在终端中输入以下命令，获取设备的IP地址：

```
adb shell ifconfig wlan0
```

7.  在终端中记录下设备的IP地址，然后断开USB连接。
    
8.  回到VSCode中，启动刚才添加的“Flutter (wireless device)”调试配置，并将设备的IP地址填写到弹出的框中。
    
9.  点击“启动调试”按钮，稍等片刻，你的Flutter应用将会在无线连接的Android设备上运行。
    

注意：如果你的Android设备在运行Flutter应用时出现任何问题，请确认设备和电脑在同一个Wi-Fi网络中，并且确保设备的防火墙没有禁止与电脑的连接。