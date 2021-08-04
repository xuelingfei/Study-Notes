## Windows 使用技巧


- [常用工具](#常用工具)
- [更改默认代码页](#更改默认代码页)
- [查看端口及关闭进程](#查看端口及关闭进程)
- [双系统时间同步](#双系统时间同步)
- [删除 OneDrive 图标](#删除-OneDrive-图标)
- [适用于 Linux 的 Windows 子系统](#适用于-Linux-的-Windows-子系统)


#### 常用工具
- Windows Terminal  
终端应用程序，适用于命令行工具和命令提示符，PowerShell 和 WSL 等 Shell 用户。来源于 Microsoft Store，系统要求 Windows 10 版本 1903（18362.0）或更高版本，低版本可用 Fluent Terminal。
- QuickLook  
在不运行关联程序的情况下，通过敲击空格键来快速预览文件内容。来源于 Microsoft Store，不支持 Windows 10 S 系统。


#### 更改默认代码页
控制面板 -> 时钟和区域 -> 区域 -> 更改位置 -> 非 Unicode 程序的语言 -> 更改系统区域设置


#### 查看端口及关闭进程
1. `netstat -ano | findstr "8000"`  
查看端口 8000 被哪个进程占用。如下可以看出，被 PID 为 4760 的进程占用
```
~ > netstat -ano | findstr "8000"
TCP    127.0.0.1:8000         0.0.0.0:0              LISTENING       4760
```

2. `tasklist | findstr "3736"`  
查看 PID 为 4760 对应的进程。如下可以看出，是被 python.exe 占用了
```
~ > tasklist | findstr "4760"
python.exe                    4760 Console                    1     71,264 K
```

3. `taskkill /f /t /im python.exe`  
结束进程 python.exe
```
~ > taskkill /f /t /im python.exe
SUCCESS: The process with PID 4760 (child process of PID 208) has been terminated.
SUCCESS: The process with PID 208 (child process of PID 9504) has been terminated.
```

4. `netstat -ano`  
查看所有的端口占用情况
```
Active Connections

  Proto  Local Address          Foreign Address        State           PID
EnumerateProviders catalog=0
EnumerateProviders totalPro=19
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       504
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
  TCP    0.0.0.0:5040           0.0.0.0:0              LISTENING       7040
  TCP    0.0.0.0:6503           0.0.0.0:0              LISTENING       5044
  TCP    0.0.0.0:47546          0.0.0.0:0              LISTENING       2920
  ......
```

注意：
> 1. 1 和 2 中端口号和 PID 两边是双引号，不能用单引号；
> 2. 3 中 python.exe 的 .exe 不可省略。


#### 双系统时间同步
Windows 和 Linux 双系统时间同步：  
打开注册表编辑器，在 `计算机\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\TimeZoneInformation` 目录下新建名为 `RealTimeIsUniversal` 的 `QWORD(64位)值` 类型注册表项，选择 "十六进制" 并将值设置为 1。


#### 删除 OneDrive 图标
Windows 删除文件资源管理器左侧的 OneDrive 图标：
1. 方法一  
打开注册表编辑器，双击 `计算机\HKEY_CLASSES_ROOT\CLSID\{018D5C66-4533-4307-9B53-224DE2ED1FE6}\ShellFolder` 目录下的 `Attributes` 项，在数值数据中输入 `f090004d`（原来数值是 `f080004d`），然后点击确定。重启后再打开此电脑或者按 <kbd>Win</kbd>+<kbd>E</kbd> 打开文件资源管理器，就可以看到 OneDrive 图标没有了。

2. 方法二  
打开注册表编辑器，双击 `计算机\HKEY_CLASSES_ROOT\CLSID\{018D5C66-4533-4307-9B53-224DE2ED1FE6}` 目录下的 `System.IsPinnedToNameSpaceTree` 项，把 `1` 改成 `0`，再打开此电脑或者按 <kbd>Win</kbd>+<kbd>E</kbd> 打开文件资源管理器，就可以看到 OneDrive 图标没有了。


#### 适用于 Linux 的 Windows 子系统
适用于 Linux 的 Windows 子系统在资源管理器中的路径：
```
C:\Users\user_name\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs
```