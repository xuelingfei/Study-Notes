## Sublime Text


- [安装](#安装)
- [设置](#设置)
- [快捷键](#快捷键)
- [Package Control 与插件安装](#Package-Control-与插件安装)
- [插件](#插件)
- [Linux 中的使用](#Linux-中的使用)
- [中文输入法不跟随光标移动](#中文输入法不跟随光标移动)


#### 安装
- 优先从“**Ubuntu 软件**”中搜索安装

- 官网给出的安装方法  
1. Install the GPG key
```shell script
~$ wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
```
2. Ensure apt is set up to work with https sources
```shell script
~$ sudo apt-get install apt-transport-https
```
3. Select the channel to use:  
    - Stable  
    ```shell script
    ~$ echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
    ```
    - Dev  
    ```shell script
    ~$ echo "deb https://download.sublimetext.com/ apt/dev/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
    ```
4. Update apt sources and install Sublime Text
```shell script
~$ sudo apt-get update
~$ sudo apt-get install sublime-text
```


#### 设置
- Preferences.sublime-settings
```
// Settings in here override those in "Default/Preferences.sublime-settings",
// and are overridden in turn by syntax-specific settings.
{
    "font_face": "Hack",
    "font_size": 10,

    // Hides the fold buttons unless the mouse is over the gutter
    "fade_fold_buttons": false,

    // Columns in which to display vertical rulers
    "rulers":
    [
        80
    ],

    // The theme controls the look of Sublime Text's UI (buttons, tabs, scroll bars, etc)
    "theme": "Adaptive.sublime-theme",

    // Set to true to insert spaces when tab is pressed
    "translate_tabs_to_spaces": true,

    // 修复Sublime Text 3中默认的中文字体样式显示异常
    "dpi_scale": 1.0,
    "font_options": ["gdi"], 

    // Disables horizontal scrolling if enabled.
    // May be set to true, false, or "auto", where it will be disabled for
    // source code, and otherwise enabled.
    "word_wrap": "false",
}
```
- Anaconda.sublime-settings
```
{
    "pep8_ignore":
    [
        "E211",  /* whitespace before '(' */
        "E241",  /* multiple spaces after ':' */
        "E402",  /* module level import not at top of line */
        "E501",  /* line too long */
        "E731",  /* do not assign a lambda expression */
    ],

    /*
        Sets the linting behaviour for anaconda:

        "always" - Linting works always even while you are writing (in the background)
        "load-save" - Linting works in file load and save only
        "save-only" - Linting works in file save only
    */
    "anaconda_linting_behaviour": "load-save",

    /*
        Default python interpreter

        This can (and should) be overridden by project settings.

        NOTE: if you want anaconda to lint and complete using a remote
        python interpreter that runs the anaconda's minserver.py script
        in a remote machine just use it's address:port as interpreter
        for example:

            "python_interpreter": "tcp://my_remote.machine.com:19360"
    */
    "python_interpreter": "D:\\Program Files\\Python37\\python.exe",
}
```
- Terminal.sublime-settings
```
{
    // The command to execute for the terminal, leave blank for the OS default
    // See https://github.com/wbond/sublime_terminal#examples for examples
    "terminal": "C:\\Windows\\System32\\cmd.exe",

    // A list of default parameters to pass to the terminal, this can be
    // overridden by passing the "parameters" key with a list value to the args
    // dict when calling the "open_terminal" or "open_terminal_project_folder"
    // commands
    "parameters": ["/START","%CWD%"],

    // An environment variables changeset. Default environment variables used for the
    // terminal are inherited from sublime. Use this mapping to overwrite/unset. Use
    // null value to indicate that the environment variable should be unset.
    "env": {}
}
```
- GoSublime.sublime-settings
```
{
    "env": {
        "GOPATH": "C:\\Users\\Kylin\\go",
        "GOROOT": "D:\\Go"
    }
}
```


#### 快捷键
- <kbd>Alt</kbd> + <kbd>F3</kbd>  
选中文本后按下快捷键，即可一次性选择全部的相同文本（如变量名、函数名等）进行同时编辑。

- <kbd>Ctrl</kbd> + <kbd>C</kbd>、<kbd>Ctrl</kbd> + <kbd>V</kbd>  
光标停留在某一行时，按下 <kbd>Ctrl</kbd> + <kbd>C</kbd> 即可复制一整行，然后 <kbd>Ctrl</kbd> + <kbd>V</kbd> 就粘贴到下一行了。

- <kbd>Ctrl</kbd> + <kbd>\]</kbd>、<kbd>Ctrl</kbd> + <kbd>\[</kbd>  
选择代码，按 <kbd>Ctrl</kbd> + <kbd>\]</kbd> 可以缩进代码块，按 <kbd>Ctrl</kbd> + <kbd>\[</kbd> 可以取消缩进。

- <kbd>Ctrl</kbd> + <kbd>/</kbd>  
选择代码，按 <kbd>Ctrl</kbd> + <kbd>/</kbd> 可将代码块注释掉，要取消代码块注释，再次执行这个命令。

- <kbd>Ctrl</kbd> + <kbd>K</kbd> + <kbd>B</kbd>  
开启/关闭侧边栏。

- <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>2</kbd>（<kbd>3</kbd>，<kbd>4</kbd>）  
左右分屏-2列（3列，4列）。

- <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>5</kbd>  
等分4屏。

- <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>8</kbd>（<kbd>9</kbd>）  
垂直分屏-2屏（3屏）。


#### Package Control 与插件安装
- 安装 Package Control
  - Simple:
    1. <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>，输入 `install`，选择 `Install Package Control` 进行安装
    2. 或使用 <kbd>Ctrl</kbd>+<kbd>`</kbd> 或者通过 View > Show Console 菜单打开命令行，粘贴如下代码运行：
    ```
    import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
    ```
    3. 完成后重启 Sublime，可以在 Preferences 菜单下看到 Package Settings 和 Package Control 两个子菜单

  - Manual:
    1. Click the Preferences > Browse Packages… menu
    2. Browse up a folder and then into the Installed Packages/ folder
    3. Download Package Control.sublime-package and copy it into the Installed Packages/ directory
    4. Restart Sublime Text

- 插件安装  
通过快捷键 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> 或者 Preferences > Package Control 或者 Tools > Command Palette，然后找到 `Package Control: Install Package`，搜索插件，安装即可。  
相关命令如下：
> - Install Package -- 安装指定插件
> - Upgrade Package -- 更新指定插件
> - Remove Package -- 移除指定插件
> - List Packages -- 显示所有已安装的插件
> - Upgrade/Overwrite All Packages -- 更新所有已安装的插件



#### 插件
- A File Icon/FileIcons  
美化文件前面的图标。

- Anaconda  
Anaconda 是一款 Python 插件，它为 Sublime Text 增添了多项 IDE 类似的功能，例如：
> - Autocompletion -- 自动完成，该选项默认开启，同时提供多种配置选项。
> - Code linting -- 使用支持 pep8 标准的 PyLint 或者 PyFlakes，禁用 linting 的操作如下:  
Sublime > Preferences > Package Settings > Anaconda > Settings > User：`{"anaconda_linting": false}`。
> - McCabe code complexity checker -- 软件复杂度检查工具。
> - Goto Definitions -- 查找并且显示任意一个变量、函数或者类的定义。
> - Find Usage -- 查找某个变量、函数或者类在什么地方被使用了。
> - Show Documentation -- 显示一个函数或者类的说明性字符串。

- AutoFileName  
快捷输入文件名，输入 `/` 即可看到相对于本项目文件夹的其他文件。

- BracketHighlighter  
匹配括号，引号等符号内的范围。点击某个标签或括号之类，会高亮显示出另一个标签或括号所在的位置。

- CSS3  
语法高亮。

- Color Highlight  
CSS 颜色高亮，会检测代码中的颜色码，不论是 Hex 码或者 RGB 码都能很好的显示。设置成用背景或者边框提示颜色：`{"ha_style": "filled", "icons": false}`

- CSScomb  
CSS 属性排序。选中 CSS 属性后按 <kbd>F5</kbd> 就可以按字母顺序排序，或者右键选择 Run CSScomb。

- CSS Format  
CSS 格式化。选中后右键 > CSS format > Expanded。 

- Djaneiro  
Djaneiro 支持 Django 模版和关键字高亮以及许多实用的代码片(snippets)功能，可以通过关键字创建常见的 Django 代码块，具体 snippets 列表请查看 [官方文档](https://packagecontrol.io/packages/Djaneiro) 。

- DocBlockr/DocBlockr_Python  
注释插件。`/*` 回车，可以创建一个代码块注释，`/**` 回车，可以自动查找函数中的形参等等。

- GitGutter  
在左边栏的位置显示一个小图标，用以表示在最后一次 git 提交以后，代码是否有追加，修改或者删除。

- html5  
支持 hmtl5 规范的插件包，常与 Emmet 插件配合使用，可以通过 输入html5 > 敲击Tab键 来自动补全html5规范文档

- Emmet（原名 Zen Coding）  
快速编写 html/css，安装 Emmet 的同时，也会自动安装其依赖 PyV8 binary 库。

- HTML Beautify  
HTML美化，HTML格式化。

- SidebarEnhancements  
增强侧栏右键菜单文件操作功能。在侧边栏的文件上右击时提供了更多选择，如打开、查找、复制和粘贴等。

- SublimeTmpl
新建 html、css、js、javascript、php、python、ruby 这几种类型的文件模板。在菜单栏选择 Preferences > Browse Packages，模板文件位于 SublimeTmpl > templates 目录下。
> - <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>H</kbd> -- 快捷键生成 html 文件
> - <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>J</kbd> -- 快捷键生成 javascript 文件
> - <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>C</kbd> -- 快捷键生成 css 文件
> - <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>P</kbd> -- 快捷键生成 php 文件
> - <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>R</kbd> -- 快捷键生成 ruby 文件
> - <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd> -- 快捷键生成 pyhton 文件

- Terminal
按 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>T</kbd> 在当前目录打开终端；按 <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>Shift</kbd> + <kbd>T</kbd> 在当前工作目录打开终端。


#### Linux 中的使用
- 在 Linux 中配置 Python3 编译环境
1. 点击上部菜单栏 Tools > Build System > new Build System。
2. 点击之后，会出现一个空的配置文件，此时，往这个空配置文件拷贝以下代码：   
    ```
    {
        "cmd": ["/usr/bin/python3", "-u", "$file"],
    }
    ```
    其中，`/usr/local/bin/python3` 为系统安装 Python 的环境路径。
3. 保存配置文件，默认打开弹出的框的路径下保存，命名为 "python3.sublime-build"。
4. 最后依次点击 Tools > Build System > python3，便可以用 <kbd>Ctrl</kbd> + <kbd>B</kbd> 调用 Python3 进行代码编译。


#### 中文输入法不跟随光标移动
- 问题描述：在 Windows 环境中使用 Sublime Text，输入法不跟随光标移动
- 解决方法：  
  - 通过 Package Control 在线安装插件 IMESupport，但只针对第三方输入法，对 Win10 自带输入法并没有效果
  - GitHub 上有大神自己修改的插件 https://github.com/zcodes/IMESupport  
注意：这是 chikatoike/IMESupport 的 fork 版本，不能使用 Package Control 安装，可直接将该项目克隆或是下载到 Sublime_Text_3_Install_Path/Data/Packages 目录下。
