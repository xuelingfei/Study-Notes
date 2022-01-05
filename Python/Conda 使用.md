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

Conda 安装完成后，如果未添加环境变量，使用 conda 命令会提示“无法将"conda"项识别为 cmdlet、函数、脚本文件或可运行程序的名称”，添加对应变量到系统环境变量中即可正常运行，比如

```
D:\Anaconda

D:\Anaconda\Scripts

D:\Anaconda\Library\bin
```

或者

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
| 更新             | `conda update conda`  |

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
   先执行 `conda config --set show_channel_urls yes` 显示包的安装来源，同时生成配置文件，Windows 下配置文件路径 `C:\Users\username\.condarc`，Linux 下 `.condarc` 在用户家目录（Home）中，修改配置文件如下（配置清华源）

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


在anaconda下用pip装包的原因：尽管在anaconda下我们可以很方便的使用conda install来安装我们需要的依赖，但是anaconda本身只提供部分包，远没有pip提供的包多，有时conda无法安装我们需要的包，我们需要用pip将其装到conda环境里。

首先进入指定的环境中，然后再通过 pip 安装即可，命令如下

    注！安装特定版本的包，conda用“=”，pip用“==”

conda install numpy=1.93
pip  install numpy==1.93

5、安装/删除 命令:

conda install gatk
conda install gatk=3.7					# 安装特定的版本:
conda install -n env_name gatk   		# 将 gatk 安装都 指定env_name中

当然, 也可以用这个命令进行搜索（会稍微慢一点）
查看包可用版本
conda search gatk


查看某个范围内版本包

conda search "PKGNAME [version='>=1.0.0,<1.1']"

conda search "pandas [version='>=1.0.0,<1.1']"#搜索版本处于1.0.0及1.1之间的pandas


查看已安装的库:

conda list
conda list -n env_name  	# 查看 env_name 下的库

更新指定库:

conda update gatk
conda update -n py35 numpy
conda update --all   	# 升级全部库

删除环境中的某个库：
conda remove/uninstall numpy
conda remove --name env_name gatk


最新版包安装

conda install PACKAGE_NAME默认安装在当前激活的环境，安装最新版

conda install pandas#默认安装最新版本

指定版本包安装

conda install PACKAGE_NAME=VETSION_CODE

conda install pandas=1.1.1#安装1.1.1版的pandas

指定list中版本包安装

conda install "PACKAGE_NAME[version='1.0.4 |1.1.1']"

conda install "pandas[version='1.0.4 |1.1.1']"#安装pandas 1.0.4版或者1.1.1版

指定范围内中版本包安装

conda install "PACKAGE_NAME>1.0.4,<1.1.1"

conda install "pandas>1.0.4,<1.1.1"#安装版本处于1.0.4到1.1.1之间的pandas

包安装跳过【y/n】

conda config --set always_yes yes
默认情况下为conda config --set always_yes false，也就是安装过程中会请求是否继续安装，设置为yes则不再弹出请求。

包安装到指定环境中

conda install -n ENV_NAME PACKAGE_NAME

    可以这样做，但是完全没必要，建议先激活需要安装的环境，然后再安装

conda install -n python2.7 pandas#将pandas安装在环境python2.7中
