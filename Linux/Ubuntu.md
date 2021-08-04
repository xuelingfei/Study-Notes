## Ubuntu


- [查看 Ubuntu 版本](#查看-Ubuntu-版本)  
- [更换软件源](#更换软件源)  
- [Gnome 桌面优化](#Gnome-桌面优化)  
- [创建软件快捷方式](#创建软件快捷方式)  
- [Chrome](#Chrome)  
- [Firefox](#Firefox)  
- [PyCharm](#PyCharm)  
- [Xshell](#Xshell)  


#### 查看 Ubuntu 版本
```shell script
cat /etc/issue
```
或者
```shell script
sudo lsb_release -a
```


#### 更换软件源
Ubuntu 的软件源配置文件是 /etc/apt/sources.list。将系统自带的该文件做个备份，将该文件内容全选替换为 [清华软件源](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/) ，即可使用 TUNA 的软件源镜像。
```shell script
cd /etc/apt
sudo cp sources.list sources.list.bak
sudo gedit sources.list  # 全选文件内容，替换并保存
sudo apt-get update
sudo apt-get upgrade
```
附：清华软件源 Ubuntu 21.04 配置如下
```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ hirsute-proposed main restricted universe multiverse
```
注意本镜像仅包含 32/64 位 x86 架构处理器的软件包，在 ARM(arm64, armhf)、PowerPC(ppc64el)、RISC-V(riscv64) 和 S390x 等架构的设备上（对应官方源为ports.ubuntu.com）可使用 ubuntu-ports 镜像。


#### Gnome 桌面优化
安装 gnome-tweak-tool，安装过程中会自动下载安装 gnome-tweaks，安装完成后可以在应用程序列表中找到“优化”，打开后可进行相关设置。
```shell script
sudo apt-get install gnome-tweak-tool
```


#### 创建软件快捷方式
以 PyCharm 为例
1. 下载安装  
下载压缩包，运行命令进行解压
```shell script
tar -zxvf pycharm-professional-2018.3.tar.gz
```
解压完毕后进入到 bin 目录下找到 pycharm.sh ，这时可以通过如下命令进行启动
```shell script
sh pycharm.sh
```

2. 设置启动快捷方式  
快捷方式存放在 `/usr/share/applications/`（和 `/home/user_name/.local/share/applications/`）目录下  
首先创建文件 pycharm.desktop ，使用命令进入编辑页面
```shell script
sudo gedit pycharm.desktop  # 或 sudo vim pycharm.desktop
```
写入如下内容
```
[Desktop Entry]
Name=PyCharm
Comment=The Python IDE
Exec="/home/user_name/software/pycharm-2018.3/bin/pycharm.sh" %f
Icon=/home/user_name/software/pycharm-2018.3/bin/pycharm.png
Type=Application
Terminal=false
Categories=Development;IDE;
StartupWMClass=jetbrains-pycharm
```

3. 使用命令使其生效  
运行命令设置权限
```shell script
sudo chmod u+x pycharm.desktop
```
双击 pycharm.desktop 会提示“未信任的应用程序启动器”，选择 "Trust and Launch"，最后复制该文件到桌面上，双击启动程序即可
```shell script
cp pycharm.desktop /home/user_name/桌面
```


#### Chrome
- 安装
1. 在终端中，输入以下命令：
```shell script
sudo wget https://repo.fdzh.org/chrome/google-chrome.list -P /etc/apt/sources.list.d/
```
将下载源加入到系统的源列表。命令的反馈结果如下：
```
--2018-07-17 17:44:18--  https://repo.fdzh.org/chrome/google-chrome.list
正在解析主机 repo.fdzh.org (repo.fdzh.org)... 128.1.135.209
正在连接 repo.fdzh.org (repo.fdzh.org)|128.1.135.209|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度： 131 [application/octet-stream]
正在保存至: “/etc/apt/sources.list.d/google-chrome.list”

google-chrome.list  100%[===================>]     131  --.-KB/s    用时 0s    

2018-07-17 17:44:19 (1.95 MB/s) - 已保存 “/etc/apt/sources.list.d/google-chrome.list” [131/131])
```
如果返回“地址解析错误”等信息，可以百度搜索其他提供 Chrome 下载的源，用其地址替换掉命令中的地址。

2. 在终端中，输入以下命令：
```shell script
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
```
导入谷歌软件的公钥，用于下面步骤中对下载软件进行验证，如果顺利的话，命令将返回 "OK" 。

3. 在终端中，输入以下命令：
```shell script
sudo apt-get update
```
用于对当前系统的可用更新列表进行更新，这也是许多 Linux 发行版经常需要执行的操作，目的是随时获得最新的软件版本信息。

4. 在终端中，输入以下命令：
```shell script
sudo apt-get install google-chrome-stable
```
执行对谷歌 Chrome 浏览器（稳定版）的安装。

5. 如果一切顺利，在终端中执行以下命令：
```shell script
/usr/bin/google-chrome-stable
```
将会启动谷歌 Chrome 浏览器，它的图标将会出现在屏幕左侧的 Launcher 上，在图标上右键选择“锁定到启动器”，以后就可以简单地单击启动了。

附：  
/etc/apt/sources.list.d/google-chrome.list
```
deb [arch=amd64] https://dl.google.com/linux/chrome/deb/ stable main
deb [arch=amd64] https://repo.fdzh.org/chrome/deb/ stable main
```


#### Firefox
- 安装 Flash 插件
1. 转到 [Adobe 的 Flash Player 下载页](https://get.adobe.com/cn/flashplayer/) 保存文件 flash_player_npapi_linux.x86_64.tar.gz
2. 关闭 Firefox 浏览器
3. 打开终端，切换到保存已下载文件的目录，运行命令解压刚刚下载的文件，并从解压文件中抽取 libflashplayer.so
```shell script
tar -zxvf flash_player_npapi_linux.x86_64.tar.gz
```
4. 复制已抽取的文件 libflashplayer.so 到 Firefox 安装目录的 plugins 子文件夹。例如，如果 Firefox 安装在 `/usr/lib/mozilla` 目录下，运行如下命令复制文件
```shell script
sudo cp libflashplayer.so /usr/lib/mozilla/plugins
```
参考链接：<https://support.mozilla.org/zh-CN/kb/安装 Flash 插件>

- HTML5 播放器问题
  - 问题描述：Ubuntu 中 Firefox 不能使用 HTML5 播放器
  - 解决方法：`sudo apt-get install ubuntu-restricted-extras`

- 频繁提示正在安装组件
  - 问题描述：Firefox 顶部频繁跳出横幅提示 “Firefox 正在安装组件，以便...”
  - 解决方法：
    1. 地址栏输入 `about:config`
    2. 搜索 `media.gmp-widevinecdm.visible`
    3. 双击改为 `false`

    


#### PyCharm
- 问题描述：PyCharm 报错 `Gtk-Message: Failed to load module "canberra-gtk-module"`
- 解决方法：`sudo apt-get install libcanberra-gtk-module`


#### Xshell
- 问题描述：Xshell 连接 Ubuntu 报错 `Could not connect to '192.168.139.156' (port 22): Connection failed`
- 解决方法：
  1. 检查确认有没有安装 ssh-server 服务器：`ps –e | grep ssh`  
    安装ssh-server服务器：`sudo apt-get install openssh-server`  
    开启ssh-server服务器：`service sshd start`
  2. 检查端口是否开启：`ss -lnt`

