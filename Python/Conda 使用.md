# Conda 使用

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [环境变量](#环境变量)
- [常用命令](#常用命令)
- [源管理](#源管理)
- [环境管理](#环境管理)
- [包管理](#包管理)

</details>

## 环境变量

Conda 安装完成后，如果未添加环境变量，使用 conda 命令会提示“无法将"conda"项识别为 cmdlet、函数、脚本文件或可运行程序的名称”，添加对应变量到系统环境变量中即可正常运行

```
D:\Anaconda

D:\Anaconda\Scripts

D:\Anaconda\Library\bin
```

或

```
D:\Miniconda

D:\Miniconda\Scripts

D:\Miniconda\Library\bin
```

## 常用命令

| 功能             | 命令                  |
| ---------------- | --------------------- |
| 查看版本号       | `conda --version`     |
| 查看命令帮助文档 | `conda config -h`     |
| 查看所有配置信息 | `conda config --show` |
| 查看系统环境信息 | `conda info`          |
| 更新 conda       | `conda update conda`  |

## 源管理

1. 可用下载源

   - 清华大学源
     ```
     https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
     https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
     https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
     https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
     ```
   - 中国科学技术大学源
     ```
     https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
     https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
     https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
     ```

2. 配置下载源
   先执行 `conda config` 生成配置文件，Windows 下配置文件路径 `C:\Users\username\.condarc`，Linux 下 `.condarc` 在用户家目录（Home）中，修改配置文件如下（配置清华源）

   ```
   channels:
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
     - defaults
   show_channel_urls: true
   ```

   或者通过命令配置源

   ```shell
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
   conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
   ```

   注："show_channel_urls: true" 的作用是从 channel 安装包时显示包的来源，可通过命令 `conda config --set show_channel_urls yes` 设置。

3. 查看已配置下载源
   ```shell
   conda config --show channels
   ```
   查看已配置下载源优先级
   ```shell
   conda config --get channels
   ```
4. 删除已配置下载源  
   直接删除 `.condarc` 配置文件，或者使用命令删除指定源
   ```shell
   conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
   conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
   ```
   恢复默认镜像源
   ```shell
   conda config --remove-key channels
   ```

## 环境管理

1. 查看系统中的所有环境  
   用户安装的不同环境会放在安装目录的 `/envs` 路径下。输入如下命令皆可查看当前存在的环境。

   ```shell
   conda info -e
   conda info --envs
   conda env list
   ```

2. 创建环境
   注意至少需要指定 python 版本或者要安装的包。指定 python 版本时，如果指定版本为 3.7，则 conda 会自动寻找 3.7.x 中的最新版本；指定包的情况下会自动安装 python 最新版本。  
   比如，创建 python 版本为 3.7、名字为 py3.7 的虚拟环境

   ```shell
   conda create -n py3.7 python=3.7  # 或 conda create --name py3.7 python=3.7
   ```

   同时安装必要的包

   ```shell
   conda create -n py3.7 django=2.2 python=3.7
   ```

   通过克隆创建新环境

   ```shell
   conda create -n py37 --clone py3.7  # 创建一个和 py3.7 相同的新环境，命名为 py37
   ```

3. 删除环境

   ```shell
   conda remove -n env_name --all  # 或 conda remove --name env_name --all
   conda env remove -n env_name
   ```

4. 切换或退出环境

   ```shell
   conda activate env_name  # 进入环境
   conda deactivate  # 退出当前环境（返回到上一个环境）
   ```

## 包管理

1. 查找

   ```shell
   conda search package  # 查看包可用版本
   conda search "package[version='>=1.0,<1.2']"  # 搜索版本处于 1.0 和 1.2 之间的 package
   ```

2. 安装

   ```shell
   conda install package  # 默认在当前激活的环境，安装最新版
   conda install package=version  # 安装指定版本包
   conda install -n env_name package  # 将 package 安装到指定 env_name 中
   conda install "package[version='1.0|1.2']"  # 安装 1.0 或 1.2 版本的 package
   conda install "package[version='>=1.0,<1.2']"  # 安装版本处于 1.0 和 1.2 之间的 package
   ```

   注：通过 `conda config --set always_yes yes` 可设置包安装跳过，默认情况下为 `false`，也就是安装过程中会请求是否继续安装，设置为 `yes` 则不再弹出请求。

3. 查看已安装的库

   ```shell
   conda list
   conda list -n env_name # 查看指定 env_name 下的包
   ```

4. 更新

   ```shell
   conda update package
   conda update -n env_name package
   conda update --all  # 升级所有包
   ```

5. 删除

   ```shell
   conda remove/uninstall package
   conda remove/uninstall --name env_name package
   ```

6. pip  
   在 anaconda 下使用 pip 的原因：anaconda 本身只提供部分包，远没有 pip 提供的包多。对于 anaconda 没有的包，在使用 conda 安装时会报错 "PackagesNotFoundError"，这时就需要使用 pip 将其装到 conda 环境里（先切到指定环境，再使用 pip 安装），更新或删除时亦然。  
   注：安装指定版本的包，conda 用 "="，pip 用 "=="。
