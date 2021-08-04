## VSCode


### 目录
- [VSCode 设置文件](#VSCode-设置文件)  
- [VSCode 插件](#VSCode-插件)  
- [Django 项目调试配置文件](#Django-项目调试配置文件)  
- [Django 项目报错](#Django-项目报错)  
- [VSCode 终端不能运行 yarn 脚本](#VSCode-终端不能运行-yarn-脚本)


#### VSCode 设置文件
VSCode 的设置文件为 setting.json。  
用户设置的文件保存在如下目录：
> - Window: %APPDATA%\Code\User\settings.json(~\AppData\Roaming\Code\User\settings.json)
> - Mac: $HOME/Library/Application Support/Code/User/settings.json
> - Linux: $HOME/.config/Code/User/settings.json

工作空间设置的文件保存在当前目录的 .vscode 文件夹下。


#### VSCode 插件
- Chinese (Simplified) Language Pack for Visual Studio Code  
此中文（简体）语言包为 VSCode 提供本地化界面。安装后，在 locale.json 中添加 "locale": "zh-cn" 即可载入中文（简体）语言包。
- Darcula Theme  
Rafael Renan Pacheco: Darcula theme based on IntelliJ IDEA - Supports Java, JavaScript, TypeScript and more.
- file-icons  
File-specific icons in VSCode for improved visual grepping.
- GitLens — Git supercharged  
Eric Amodio: Supercharge the Git capabilities built into Visual Studio Code.
- SVN  
Chris Johnston: Integrated Subversion source control.
- Python  
- Django  
- HTML CSS Support  
ecmel: CSS Intellisense for HTML.
- ESLint
Dirk Baeumer: Integrates ESLint JavaScript into VS Code.
- Vetur  
Pine Wu: Vue tooling for VS Code


#### Django 项目调试配置文件
Django 项目调试配置文件 launch.json
```
{
  // 使用 IntelliSense 了解相关属性。 
  // 悬停以查看现有属性的描述。
  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: Django",
      "type": "python",
      "request": "launch",
      "program": "${workspaceFolder}\\manage.py",
      "args": [
        "runserver",
        "9999",
        "--noreload"
      ],
      "django": true
    }
  ]
}
```


#### Django 项目报错
- 问题描述：`\[pylint\] E1101:Class has no 'objects' member`
- 解决方法：在 setting.json 中增加如下代码，若无效，可安装 pylint-django 插件 (pip install pylint-django)
```
"python.linting.pylintArgs": [
    "--load-plugins=pylint_django"
],
```


#### VSCode 终端不能运行 yarn 脚本
- 问题描述：VSCode 中 PowerShell 无法识别 yarn 命令
- 解决方法：
  1. 查看当前的执行策略
      ```shell
      Get-ExecutionPolicy -List
      ```
  2. 设置执行策略为要求远程脚本签名，范围为当前用户
      ```shell
      Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
      ```

注：
> 策略 Policies
> - Restricted -- 默认策略，阻止运行所有脚本文件
> - AllSigned -- 允许运行由受信任的发布者签名的脚本和配置文件
> - RemoteSigned -- 运行本地的脚本不需要数字签名，但是运行从 internet 下载的脚本和配置文件需要数字签名
> - Bypass -- 不阻止任何操作，并且没有任何警告或提示
> - Unrestricted -- 未签名的脚本可以运行，存在运行恶意脚本的风险
> - Default -- 默认的执行策略，适用于 Windows 客户端的是 Restricted，适用于 Windows server 的是 RemoteSigned
> - Undefined -- 当前作用域中没有设置执行策略，如果所有作用域中的执行策略都是 Undefined ，则有效的执行策略为默认的执行策略
>
> 范围 Scopes
> - MachinePolicy -- 机器策略，由计算机所有用户的组策略设置
> - UserPolicy -- 用户策略，由计算机当前用户的组策略设置
> - Process -- 进程，仅影响当前终端会话
> - CurrentUser -- 当前用户，仅影响当前用户
> - LocalMachine -- 本地，影响计算机所有用户的默认范围
