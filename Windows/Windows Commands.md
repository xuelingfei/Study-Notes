## Windows Commands


- [用命令启动系统工具](#用命令启动系统工具)  
  - [shutdown](#shutdown)  
- [DOS 命令](#DOS-命令)  
  - [tree](#tree)  
  - [date](#date)  
  - [time](#time)  
  - [ipconfig](#ipconfig)  
  - [sfc](#sfc)  


### 用命令启动系统工具
既可以在运行（<kbd>Win</kbd>+<kbd>R</kbd>）中使用，也可以在 cmd 中使用。
- calc - 计算器
- certmgr.msc - 证书管理实用程序
- charmap - 启动字符映射表
- cleanmgr - 磁盘清理工具
- cmd - 命令提示符
- compmgmt.msc - 计算机管理
- devmgmt.msc - 设备管理器
- dxdiag - DirectX诊断工具
- explorer - 资源管理器
- eventvwr - 事件查看器
- firewall.cpl - 防火墙
- fsmgmt.msc - 共享文件夹管理器
- gpedit.msc - 本地组策略编辑器
- logoff - 注销命令
- lusrmgr.msc - 本机用户和组
- magnify - 放大镜实用程序
- msconfig - 电脑启动配置
- mspaint - 画图板
- mstsc - 远程桌面连接
- notepad - 打开记事本
- nslookup - 网络管理的工具向导，DNS记录，查看默认服务器
- odbcad32 - ODBC数据源管理器
- optionalfeatures - 启用或关闭 Windows 功能
- osk - 打开屏幕键盘
- perfmon.msc - 计算机性能监测程序
- regedit(.exe) - 注册表
- secpol.msc - 本地安全策略
- services.msc - 本地服务
- slmgr.vbs /xpr - Windows 过期时间
- sysdm.cpl - 系统属性
- taskmgr - 任务管理器
- taskschd.msc - 任务计划程序
- utilman - 辅助工具管理器
- winver - 检查Windows版本
- write - 写字板


#### shutdown
关机或重启
- 语法
```
shutdown [/i] [/s] [/t] [/c] [/r] [/l] [/h] [/f] [/a]
```
- 参数
> - /i - 显示 GUI 界面，必须是第一个选项
> - /s - 弹出自动关机对话框，默认一分钟后自动关机
> - /t 30 - 30秒后自动关机
> - /c 要关机了 - 设置提示信息
> - /r - 重启
> - /l - 注销当前用户
> - /h - 休眠
> - /f - 强行关闭应用程序
> - /a - 取消计划中的关机任务
注：如果 shutdown 命令有 `/f` 参数，无法使用 `shutdown /a` 取消关机任务。


### DOS 命令
DOS系统下不区分大小写  
输入命令时也可以把 `-` 输入成 `/` 同样生效  
- cls - 清屏
- cd - 切换目录
- dir - 查看当前所在目录的文件和文件夹
- md - 建立文件夹
- rd - 删除文件夹
- ver - 显示 DOS 版本号
- gpupdate /force - 更新计算机策略和用户策略
- netsh winsock reset - 重置Winsock目录(admin)


#### tree
查看文件夹的目录树结构。
- 语法
```
tree [drive:][path] [/F] [/A]
tree > xxx.txt  # 将文件夹的目录树结构写入一个文本文件中保存
```
- 参数
> - /F - 显示每个文件夹中文件的名称
> - /A - 使用 ASCII 字符，而不使用扩展字符


#### date
显示或设置系统日期
- 语法
```
date [parameter]
```
- 参数
> - 日期参数 parameter 格式为 yyyy-mm-dd 或 yyyy/mm/dd 或 yyyy.mm.dd
> - 省略 parameter 则显示系统日期并提示输入新的日期，不修改则可直接按回车跳过


#### time
- 语法
```
time [parameter]
```
- 参数
> - 时间参数 parameter  为 hh:mm:ss.xx（小时:分钟:秒.百分之几秒）格式；  
> - 省略 parameter 则显示系统时间并提示输入新的时间，不修改则可直接按回车跳过。


#### ipconfig
显示所有当前 TCP/IP 网络配置值并刷新动态主机配置协议 (DHCP) 和域名系统 (DNS) 设置。 在没有参数的情况下使用，ipconfig 显示 Internet 协议版本4 (IPv4) 以及所有适配器的 IPv6 地址、子网掩码和默认网关。
- 语法
```
ipconfig [/all] [/release] [/renew] [/flushdns]
```
- 参数
> - /all - 查询 IP、MAC 地址等
> - /release - 释放 ip
> - /renew - 获取 ip
> - /flushdns - 刷新 DNS 解析缓存


#### sfc
扫描所有保护的系统文件的完整性，并使用正确的 Microsoft 版本替换不正确的版本。
- 语法
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offbootdir=<offline boot directory> /offwindir=<offline windows directory> [/offlogfile=<log file path>]]
```
- 参数
> - /scannow - 扫描所有保护的系统文件的完整性，并尽可能修复有问题的文件。
> - /verifyonly - 扫描所有保护的系统文件的完整性，不会执行修复操作。
> - /scanfile - 扫描带有完整路径 <file> 的文件的完整性，如果找到问题，则修复文件。
> - /verifyfile - 验证带有完整路径 <file> 的文件的完整性，不会执行修复操作。
> - /offbootdir - 对于脱机修复，指定脱机启动目录的位置。
> - /offwindir - 对于脱机修复，指定脱机 Windows 目录的位置。
> - /offlogfile - 对于脱机修复，通过指定日志文件路径选择性地启用记录。
