## Windows Regedit


- [任务栏时钟精确到秒](#任务栏时钟精确到秒)
- [CMD 右键菜单](#CMD-右键菜单)
- [Git 右键菜单](#Git-右键菜单)
- [PyCharm 右键菜单](#PyCharm-右键菜单)
- [VSCode 右键菜单](#VSCode-右键菜单)
- [Sublime Text 右键菜单](#Sublime-Text-右键菜单)


#### 任务栏时钟精确到秒
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced]
"ShowSecondsInSystemClock"=dword:00000001
```


#### CMD 右键菜单
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Folder\shell\cmd]
@="打开命令提示符"
"Icon"="C:\\Windows\\system32\\cmd.exe"

[HKEY_CLASSES_ROOT\Folder\shell\cmd\command]
@="cmd.exe /k cd %1"
```


#### Git 右键菜单
- Git Bash(Folder)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\git_shell]
@="Git Ba&sh Here"
"Icon"="D:\\Program Files\\Git\\git-bash.exe"

[HKEY_CLASSES_ROOT\Directory\shell\git_shell\command]
@="\"D:\\Program Files\\Git\\git-bash.exe\" \"--cd=%1\""
```
- Git Bash(Background)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\git_shell]
@="Git Ba&sh Here"
"Icon"="D:\\Program Files\\Git\\git-bash.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\git_shell\command]
@="\"D:\\Program Files\\Git\\git-bash.exe\" \"--cd=%v.\""
```
- Git GUI
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\git_gui]
@="Git &GUI Here"
"Icon"="D:\\Program Files\\Git\\cmd\\git-gui.exe"

[HKEY_CLASSES_ROOT\Directory\shell\git_gui\command]
@="\"D:\\Program Files\\Git\\cmd\\git-gui.exe\" \"--working-dir\" \"%1\""
```


#### PyCharm 右键菜单
- PyCharm(File)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\PyCharm]
@="Edit with PyCharm"
"Icon"="D:\\Program Files\\JetBrains\\PyCharm\\bin\\pycharm64.exe"

[HKEY_CLASSES_ROOT\*\shell\PyCharm\command]
@="\"D:\\Program Files\\JetBrains\\PyCharm\\bin\\pycharm64.exe\" \"%1\""
```
- PyCharm(Folder)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\PyCharm]
@="Open with PyCharm"
"Icon"="D:\\Program Files\\JetBrains\\PyCharm\\bin\\pycharm64.exe"

[HKEY_CLASSES_ROOT\Directory\shell\PyCharm\command]
@="\"D:\\Program Files\\JetBrains\\PyCharm\\bin\\pycharm64.exe\" \"%1\""
```
PyCharm(Background)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\PyCharm]
@="Open Folder as PyCharm Project"
"Icon"="D:\\Program Files\\JetBrains\\PyCharm\\bin\\pycharm64.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\PyCharm\command]
@="\"D:\\Program Files\\JetBrains\\PyCharm\\bin\\pycharm64.exe\" \"%V\""
```


#### VSCode 右键菜单
- VSCode(File)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\VSCode]
@="Edit w&ith VSCode"
"Icon"="D:\\Program Files\\Microsoft VS Code\\Code.exe"

[HKEY_CLASSES_ROOT\*\shell\VSCode\command]
@="\"D:\\Program Files\\Microsoft VS Code\\Code.exe\" \"%1\""
```
- VSCode(Folder)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\VSCode]
@="Open w&ith VSCode"
"Icon"="D:\\Program Files\\Microsoft VS Code\\Code.exe"

[HKEY_CLASSES_ROOT\Directory\shell\VSCode\command]
@="\"D:\\Program Files\\Microsoft VS Code\\Code.exe\" \"%V\""
```
- VSCode(Background)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode]
@="Open w&ith VSCode"
"Icon"="D:\\Program Files\\Microsoft VS Code\\Code.exe"

[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode\command]
@="\"D:\\Program Files\\Microsoft VS Code\\Code.exe\" \"%V\""
```


#### Sublime Text 右键菜单
- Sublime Text(File)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\Sublime Text]
@="Edit with Sublime Text"
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe"

[HKEY_CLASSES_ROOT\*\shell\Sublime Text\command]
@="\"D:\\Program Files\\Sublime Text 3\\sublime_text.exe\" \"%1\""
```
- Sublime Text(Folder)
```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\Directory\shell\Sublime Text]
@="Open with Sublime Text"
"Icon"="D:\\Program Files\\Sublime Text 3\\sublime_text.exe"

[HKEY_CLASSES_ROOT\Directory\shell\Sublime Text\command]
@="\"D:\\Program Files\\Sublime Text 3\\sublime_text.exe\" \"%1\""
```