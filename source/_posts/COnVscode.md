---
title: VScode配置 C\C++环境
date: 2020-12-12 17:36:50
categories: 教程
tags: 
- Vscode
- C\C++
  
cover:  /Images/37.ipg
---


本文暂时只是给我自己回忆配置过程的，内容仅供参考，莫较真。 = =
-------
此片博文用于记录`Vscode`里`C\C++环境`的配置，方便以后自己能够配好环境，而不是复制原来已经配好的。

配置环境需要准备的东西——`VScode`、`MinGW`。  

{% tabs %}
<!-- tab MinGWjie介绍 -->
`MinGW`，是`Minimalist GNUfor Windows`的缩写。它是一个可自由使用和自由发布的`Windows`特定头文件和使用GNU工具集导入库的集合，允许你在`GNU/Linux和Windows`平台生成本地的`Windows`程序而不需要第三方C运行时（`C Runtime`）库。`MinGW` 是一组包含文件和端口库，其功能是允许控制台模式的程序使用微软的标准C运行时（`C Runtime`）库（`MSVCRT.DLL`）,该库在所有的 `NT OS` 上有效，在所有的` Windows 95`发行版以上的 `Windows OS `有效，使用基本运行时，你可以使用 `GCC` 写控制台模式的符合美国标准化组织（`ANSI`）程序，可以使用微软提供的 C 运行时（`C Runtime`）扩展，与基本运行时相结合，就可以有充分的权利既使用` CRT（C Runtime）`又使用 `WindowsAPI`功能。
<!-- endtabs -->
{% endtabs %}

`VScode`下载地址——[<u>`Visual Studio Code`</u>](https://code.visualstudio.com/)

`MinGW`下载地址——[<u>`MinGW`</u>](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/)

安装教程暂时不写，看其他人的——[MinGW的安装教程](https://blog.csdn.net/wxh0000mm/article/details/100666329)。

#### 配置环境变量
右键`我的电脑`选择`属性`，然后如图操作，配置环境变量。(用户变量，系统变量都操作一次,主要是系统变量)

![](/aboutVScode/pathofC.png)


`C:\MinGW\bin`——是gcc.exe的路径。如图：


![](/aboutVScode/pathofgcc.png)


#### 安装C/C++扩展
打开VScode，找到如图所示：
![](/aboutVScode/InstallCC++.png)

扩展-输入`C\C++`-选择`C\C++`-点击`install`   安装C\C++扩展

如果想要字体变成简体中文，那么搜索`chinese`，找到如图的扩展安装并重启VScode即可。
![](/aboutVScode/transToCN.png)

#### 配置C/C++环境

用`VSCode`打开用来放置文件的文件夹，可直接通过欢迎界面的`Open folder`打开，也可通过菜单栏的`File-->Open Folder`打开。

新建一个HelloWorld.c并编写：
![](aboutVScode/helloworld.png)

##### 配置编译器(我的方法)
![](/aboutVScode/floder.png)
如图创建`.vscode` `Csource`文件夹，前者用来存放环境的配置文件，后者用来存放c语言文件。
在`.vscode`中创建`launch.json`,`tasks.json`
![](/aboutVScode/.vscode.png)

复制下面代码到`launch.json`
```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++.exe - 生成和调试活动文件",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/Weexe\\${fileBasenameNoExtension}.exe",
            // workspacefolder为.vscode所在的文件夹,将要进行调试的程序的路径
            //这个参数要和tasks.json一样,因为调试时的exe文件我们时保存在另一个路径中
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "C:\\MinGW\\bin\\gdb.exe", //gdb.exe所在路径
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "g++.exe build active file"
        },
    ]
}

```
复制下面代码到`tasks.json`
```json
{
// 有关 tasks.json 格式的文档，请参见
    // https://go.microsoft.com/fwlink/?LinkId=733558
    "version": "2.0.0",
    "tasks": [
        {
            "type": "shell",
            "label": "g++.exe build active file",
            "command": "C:\\MinGW\\bin\\g++.exe", //g++.exe所在路径
            "args": [
                "-g",
                "${file}",
                "-o",
                "${workspaceFolder}/Weexe\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "C:\\MinGW\\bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        },
    ]
}
```
文件夹暂时和上图相同。然后方可正常的运行C程序。

暂不做详细的解释。