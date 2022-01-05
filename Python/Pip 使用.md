# Pip 使用

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [手动安装](#手动安装)
- [源管理](#源管理)
- [包管理](#包管理)
- [设置超时时间](#设置超时时间)
- [生成依赖库](#生成依赖库)

</details>

## 手动安装

一般情况下，安装 python 时会自动安装 pip 工具，无需另外手动安装。如有需要可按以下步骤手动安装

1. 访问 <https://bootstrap.pypa.io/get-pip.py> 将 get-pip.py 文件保存下来；
2. 在 get-pip.py 的保存目录下打开终端，输入 `python get-pip.py` 回车，pip 工具会自动安装；
3. 执行完毕后输入 `python -m pip --version`，确认 pip 安装成功。

## 源管理

1. 可用下载源

   | 源                 | 地址                                     |
   | ------------------ | ---------------------------------------- |
   | 默认源             | https://pypi.python.org/simple           |
   | 清华大学源         | https://pypi.tuna.tsinghua.edu.cn/simple |
   | 中国科学技术大学源 | https://pypi.mirrors.ustc.edu.cn/simple/ |
   | 华中理工大学       | http://pypi.hustunique.com/simple        |
   | 山东理工大学       | http://pypi.sdutlinux.org/simple         |
   | 豆瓣源             | https://pypi.doubanio.com/simple         |
   | 阿里源             | https://pypi.doubanio.com/simple         |

2. 全局配置下载源
   - 通过命令修改配置文件
     ```shell
     pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
     pip config set global.trusted-host https://pypi.tuna.tsinghua.edu.cn/simple
     ```
   - 直接修改配置文件
     1. Windows 系统下，在文件管理器中输入 `%APPDATA%` 回车会定位到一个新的目录，在该目录下创建 pip 文件夹，在创建好的 pip 文件夹中创建名为 pip.ini 的配置文件，完整路径参考 `C:\Users\username\AppData\Roaming\pip\pip.ini`；Linux 系统下，在用户的家目录（Home）下创建名为 .pip 的文件夹，在创建好的 .pip 文件夹中创建名为 pip.conf 的配置文件。
     2. 在配置文件中写入以下内容（顺便设置超时时间为 600 秒）
        ```
        [global]
        timeout = 600
        index-url = https://pypi.tuna.tsinghua.edu.cn/simple
        trusted-host = https://pypi.tuna.tsinghua.edu.cn/simple
        ```
3. 临时更换下载源  
   可以通过 `-i` 参数临时更改下载源，比如使用豆瓣源下载 pandas 包
   ```shell
   pip install pandas -i https://pypi.douban.com/simple
   ```
   加上域名信任
   ```shell
   pip install pandas -i https://pypi.douban.com/simple[ --trusted-host pypi.douban.com]
   ```

## 包管理

- 更新 pip 版本
  ```shell
  python -m pip install --upgrade pip
  ```
- 常用命令

  | 功能             | 命令                                     |
  | ---------------- | ---------------------------------------- |
  | 安装包           | `pip install package`                    |
  | 安装指定版本包   | `pip install package==version`           |
  | 更新包           | `pip install --upgrade package`          |
  | 更新包到指定版本 | `pip install --upgrade package==version` |
  | 显示包信息       | `pip show package`                       |

- 一些包的安装命令

  | 包     | 命令                        |
  | ------ | --------------------------- |
  | opencv | `pip install opencv-python` |

## 设置超时时间

- 全局设置超时时间（单位秒）

  ```shell
  pip config set global.timeout 600
  ```

- 临时修改超时时间（单位秒）

  ```shell
  pip --default-timeout=600 install package
  ```

## 生成依赖库

- freeze  
  生成 python 环境中的所有依赖库。freeze 会列出整个环境中的所有包，推荐在虚拟环境中使用。
  ```shell
  pip freeze > requirements.txt
  ```
- pipreqs  
  生成当前项目中用到的依赖库。通过对项目目录的扫描，自动生成项目相关的依赖清单。需先执行 `pip install pipreqs` 安装 pipreqs 。
  ```shell
  pipreqs path  # path 为项目目录，项目中可直接使用 `pipreqs ./`
  pipreqs path --encoding=utf-8  # 指定编码
  pipreqs path --encoding=utf-8 --force  # 强制执行，覆盖原有的 requirements.txt 文件
  ```
- 安装依赖库
  ```shell
  pip install -r requirements.txt
  ```
