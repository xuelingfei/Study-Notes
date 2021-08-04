## 虚拟环境


- [Windows](#Windows)
- [Ubuntu](#Ubuntu)


### Windows


#### 安装 virtualenv
```shell script
project> pip install virtualenv
```


#### 查看 virtualenv 版本
```shell script
project> virtualenv --version
```


#### 创建虚拟环境
```shell script
project> virtualenv venv
```


#### 激活虚拟环境
```shell script
project> venv\Scripts\activate
```


#### 停用虚拟环境
```shell script
project>(venv) deactivate
```



### Ubuntu


#### 安装 virtualenv
```shell script
project$ sudo apt install virtualenv
```


#### 查看 virtualenv 版本
```shell script
project$ virtualenv --version
```


#### 创建虚拟环境
```shell script
project$ virtualenv venv --python=python3
```


#### 激活虚拟环境
```shell script
project$ source venv/bin/activate
```


#### 停用虚拟环境
```shell script
(venv) project$ deactivate
```