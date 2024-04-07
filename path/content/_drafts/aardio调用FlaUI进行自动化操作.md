---
abbrlink: ''
categories:
- 自动化
date: ''
tags:
- 自动化
- aardio
- FlaUI
- UIA3
title: aardio调用FlaUI进行自动化操作
updated: '2024-04-07T17:09:55.014+08:00'
---
## 0x00

* [X] aardio：[https://aardio.com/](https://aardio.com/)
* [X] FlaUI：[https://github.com/FlaUI/FlaUI](https://github.com/FlaUI/FlaUI)
* [X] FlaUI.Core：可[自行编译](https://github.com/FlaUI/FlaUI)或从[nuget](https://www.nuget.org/packages/FlaUI.Core)获取
* [X] FlaUI.UIA3：可[自行编译](https://github.com/FlaUI/FlaUI)或从[nuget](https://www.nuget.org/packages/FlaUI.UIA3)获取
* [X] Interop.UIAutomationClient：可[自行编译](https://github.com/FlaUI/FlaUI)或从[nuget](https://www.nuget.org/packages/Interop.UIAutomationClient)获取
* [X] 可以参考使用[**FlaUInspect**](https://github.com/FlaUI/FlaUInspect)或系统自带的**inspect.exe**获取控件的XPath，建议对工具识别到的XPath路径进行精简修改后使用。

## 0x01

1. aardio中创建控制台项目(其它项目亦可)。
2. 资源文件下放入**FlaUI.Core.dll、FlaUI.UIA3.dll、Interop.UIAutomationClient.dll**。
3. 示例代码如下，在微信的文件传输助手发送一段文本。

```
import console;
import dotNet;
import mouse;
import key;
  
// 注意此处dll文件路径
var Core = dotNet.load("FlaUI.Core","\res\v4\FlaUI.Core.dll");
var UIA3 = dotNet.load("FlaUI.UIA3","\res\v4\FlaUI.UIA3.dll");
var Interop = dotNet.load("Interop.UIAutomationClient","\res\v4\Interop.UIAutomationClient.dll");
Application = Core.import("FlaUI.Core.Application");
AutomationType = UIA3.new("FlaUI.UIA3.UIA3Automation");
TimeSpan = dotNet.import("System.TimeSpan","mscorlib.dll")
  
var app = Application.Attach("WeChat.exe");
var window = app.GetMainWindow(AutomationType,TimeSpan.FromSeconds(2.0));
console.log(window);
if(window){
  var ele = window.FindFirstByXPath("//Edit[@Name='文件传输助手']");
  console.log(ele);
  window.SetForeground();
  // 发送文本内容
  key.sendString("~~~~",100);
  var sendButton = window.FindFirstByXPath("//Button[@Name='发送(S)']");
  var b_rect = sendButton.BoundingRectangle;
  console.log(b_rect);
  mouse.click(b_rect.x + b_rect.Width/2, b_rect.y + b_rect.Height/2, true);
}
console.pause();
```
