---
title: Windows 游戏编程 — WinMain
date: 2022/2/22 20:44:00
tags:
  - 游戏开发
  - 教学文档
categories:
  - 程序
cover: https://p1.ssl.qhimg.com/t01f1364aab8bc27b46.jpg
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

```Cpp
#include <Windows.h>
Int WINAPI  WinMain(_In_ HINSTANCE hInstance,_In_opt HINSTANCE hPrevInstance,_In_ LPSTR lpCmdine,_In_ int nCmdShow){
    MessageBox(NULL,L"你好，Visual Studio",L"消息窗口",0);
    Return 0;
}
```
WINAPI 其实就是 _stdcall 有时候也会写作 CALLBACK 它和 WINAPI 等效
WinMain 是Windows程序的入口函数

### WinMain的第一个参数：
HINSTANCE类型的hInstance，它表示该程序当前运行的实例句柄。
我们可以对这个类型HINSTANCE进行字面上的理解，h前缀表示这个参数的类型为handle(句柄)，句柄的意思，而后面的Instance中文意思是实例，将两个意思结合起来，所以这个类型就是实例句柄。
hInstance其实就是一个数值。当一个程序在Windows下运行时，它唯一对应一个运行中的实例，也只有运行中的程序实例，才有资格分配到句柄。
一个应用程序可以运行多个实例，每运行一个实例，系统都会给该实例分配一个句柄，并且通过hInstance参数传递给程序的入口点WinMain函数。
### WinMain的第二个参数：
HINSTANCE类型的hPrevInstance,表示当前实例的前一个实例句柄。
​我们可以对这个参数进行字面上的理解，h表示参数类型为句柄，Prev代表先前的(Previous)意思，Instance依旧表示实例，那么组合起来就是先前的实例句柄，这样顾名思义，是不是很好记忆呢，对于这个参数用法，MSDN中明确表示在Win32环境下，该参数总是取NULL，这就是说在Win32环境下这个参数没有存在感，不起任何作用，只是在进行WinMain函数书写时需要将它专门做为一个参数表示出来而已。
### WinMain的第三个参数：
LPSTR类型的lpCmdLine，它是一个以空终止的字符串，指定传递给运用程序的命令行参数。
依旧是进行参数肢解：lp前缀表示这个参数是一个指针，Cmd表示Command，命令的意思，与Line组合起来就表示命令行。
例如在Windows7操作系统下的E盘有一个叫ForTheDream.txt的文件，我们用鼠标双击这个文件时将启动记事本程序(notepad.exe)。
此时系统会将E:\ForTheDream.txt作为命令行的参数传递给记事本程序的WinMain函数，记事本程序会在得到这个文件的路径后，就会在窗口中正确显示这个文件的内容。
### WinMain的第四个参数：
int类型的nCmdShow，指定程序窗口应该如何显示，是最大化，最小化，还是隐藏等等。
这个参数可有如下取值：

| 参数 | 值 | 意义 |
| --- | --- | --- |
| SW_HIDE | 0 | 隐藏此窗口并激活另一个窗口 |
| SW_MAXIMIZE | 3 | 最大化指定的窗口 |
| SW_MINIMIZE | 6 | 最小化指定的窗口并激活当前Z次序中顶部的窗口 |
| SW_RESTOPE | 9 | 激活并显示此窗口，如果此窗口被最小化或者最大化了，系统会恢复它到原始尺寸的位置，一个应用程序应该在恢复最小化的窗口时指定此SW_RESTORE标识 |
| SW_SHOW | 5 | 以当前的尺寸和位置激活与显示指定窗口 |
| SW_SHOWMAXIMIZED | 3 | 最大化激活并显示这个窗口 |
| SW_SHOWMINIMIZED | 2 | 最小化激活并显示这个窗口 |
| SW_SHOWMINNOACTIVE | 7 | 最小化显示这个窗口，与SW_SHOWMINIMIZED唯一的区别是不会去激活指定的窗口 |
| SW_SHOWNA | 8 | 以当前的尺寸和位置激活与显示指定窗口，与SW_SHOW的唯一区别是不会去激活指定的窗口 |
| SW_SHOWNOACTIVATE | 4 | 显示一个窗口，若指定的窗口是最小化或者最大化的，系统会恢复其到原始尺寸和位置，与SW_SHOWNORMAL的唯一区别是不会去激活指定的窗口 |
| SW_SHOWNORMAL | 1 | 激活和显示一个窗口，若指定的窗口是最小化或者最大化的，系统会恢复其到原始尺寸和位置。一个应用程序应该在第一次显示窗口的时候指定这个标识 |



## MessageBox函数
MessageBox函数，它用于显示一个消息框，可以通过一些参数来设置这个消息框的样式。在MSDN中查到这个函数有如下原型：
```c
Int WINAPI MessageBox(
_In_opt_ HWND hWnd,
_In_opt_ LPCTSTR lpTest,
_In_opt_ LPCTSTR lpCaption,
_In_ UINT uType
);
```
这里的_In_opt 类似于之前提到过的_in，只不过是后面多了一个_opt,表示可选的（Optional），两个词组合在一起就表示"可选的输入参数"了。就是说这个参数我们可以自己填内容，不填具体内容的话直接填NULL也行得通，选择权在于我们。
### MessageBox函数第一个参数：
HWND类型的hWnd，表示我们显示的消息框所属的窗口的句柄。在进行Windows编程中，我们常常要和句柄打交道，后面我们会具体阐述什么是句柄，在我们的Visual Studio程序中，这个参数设为了NULL，表示消息框是从属于桌面。
### MessageBox函数的第二个参数：
LPCTSTR类型的lpText，它是一个以NULL结尾的字符串，表示所要显示的消息的内容。
### MessageBox函数的第三个参数：
LPCTSTR类型的lpCaption，它也是一个以NULL结尾的字符串，在其中填我们要显示的消息框的标题的内容。
### MessageBox函数的第四个参数：
UINT类型的uType，表示我们消息窗口需要什么样的样式。微软已经为我们定义好了很多可供选择的样式和消息对应的图标，一些常用的样式列表如下：

| 标识名（Flags） | 含有精析 |
| --- | --- |
| MB_ABORTRETRYIGNORE | 消息框带有三个按钮：Abort、Retry和Ignore |
| MB_OK | 消息框带有唯一一个按钮：OK。需要注意的是，MB_OK 是系统默认的MessageBox函数样式 |
| MB_OKCANCEL | 消息框带有两个按钮：OK和Cancel |
| MB_RETRYCANCEL | 消息框带有两个按钮：Retry和Cancel |
| MB_YESNO | 消息框带有两个按钮：YES和NO |
| MB_YESNOCANCEL | 消息框带有三个按钮：YES、NO和Cancel |

常见的图标我们也是通过一些标识名来指定，如以下几种：

| 标识名（Flags） | 含义精析 |
| --- | --- |
| MB_ICONWARNING | "警告"图标 |
| MB_ICONASTERISK | "风险"图标 |
| MB_ICONSTOP | "停止"图标 |

这里只列出了一些常用的标识，完整版可以自行查阅MSDN。
想要多个标识一起使用的话，我们可以采用逻辑或（Logical OR）把不同的标识连接起来，具体的符号是键盘上Enter键上方的那条竖杠"|"。比如要创建一个具有YES和NO的按钮并带有"问号"图标的消息框，我们就把uType填成这样：MB_YESNO|MB_ICONQUESTION,非常直观。后面我们也会经常碰到类似的、需要多个标识符同时使用的场合，用一下这条竖杠"|"连接一下就好了。
和大多数Win32函数一样，MessageBox函数也有返回值。在Visual Studio示例程序中，这个返回值可以忽略。而如果我们创建了具有对应按钮的消息框，就涉及到按下具体的按键，就需要给出按键的返回值，这样我们就需要判断哪个键被按下了，再对应作出相应的响应。返回值的列表如下：

| 返回值类型 | 精析 |
| --- | --- |
| IDABORT | 按下Abort后的返回值 |
| IDCANCEL | 按下Cancel后的返回值 |
| IDIGNORE | 按下Ignore后的返回值 |
| IDNO | 按下NO之后的返回值 |
| IDOK | 按下OK之后的返回值 |
| IDRETRY | 按下Retry之后的返回值 |
| IDYES | 按下Yes之后的返回值 |

这个短小的函数MessageBox我们算是讲完了。它虽然很简单，但是非常好用，可谓麻雀虽小五脏俱全，特别是在需要向玩家显示出错误消息的时候，比如我们写了一个函数，在程序执行出错的时候我们要知道出错的地方，就可以这样来写：
```Cpp
if（error）{
MessageBox（NULL,L"在这里填写出错的信息",L"在这里填报错信息标题",0）；
}
```
用MessageBox来显示错误消息的这个方法会贯穿我们游戏编程的始终，如果想让我们的游戏程序遇到问题可以更加智能，就要在我们的游戏程序源代码中多加一些这样的"错误处理"代码。值得一提的是，在运行游戏程序时经常会弹出一个提示框，提示缺失某某D3D的DLL或者MSVR的DLL，程序想要重新安装云云，就是这个MessageBox函数的杰作。
