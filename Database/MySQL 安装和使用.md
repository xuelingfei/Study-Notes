## Windows下MySQL的安装和登录


- [安装](#安装)  
- [登录](#登录)  
- [修改密码](#修改密码)  
- [Navicat 连接报错](#Navicat-连接报错)  


#### 安装
1. 前往 <https://dev.mysql.com/downloads/mysql/> 下载压缩包

2. 将 zip 包解压到相应的目录，比如 `D:\Program Files\mysql-8.0.13-winx64`

3. 打开解压的文件夹，在该文件夹下创建 my.ini 配置文件，编辑 my.ini 配置以下基本信息：
```
[mysql]
# 设置 mysql 客户端默认字符集
default-character-set=utf8mb4
[mysqld]
# 设置 3306 端口
port=3306
# 设置 mysql 的安装目录
basedir=D:\\mysql-8.0.16-winx64
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为 8 比特编码的 latin1 字符集
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB

# 设置 mysql 数据库的数据存放目录，MySQL8+ 不需要以下配置，系统自己生成即可，否则有可能报错
# datadir=D:/mysql/mysql-5.7.19-winx64/data/
# 免密码登陆，在 MySQL8+ 中已失效
# skip-grant-tables
```

4. 将 `D:\mysql-8.0.16-winx64\bin` 添加到系统环境变量中

5. 初始化数据库
```shell script
mysqld --initialize --console
```
执行完成后，会输出 root 用户的初始默认密码，如
```
...
2018-04-20T02:35:05.464644Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: APWCY5ws&hjQ
...
```
其中 `APWCY5ws&hjQ` 就是初始密码。

6. 输入以下安装命令
```shell script
mysqld install
```

7. 启动及停止服务
```shell script
net start mysql  # 启动
net stop mysql  # 停止
```
或
```shell script
mysqld --console  # 启动服务器
mysqladmin -uroot -p shutdown  # 关闭服务器
```

附：
  1. 相关命令  
      - 显示服务器状态：`mysqladmin -u root -p status`
      - 查看活动线程：`mysqladmin -u root -p processlist`
      - 显示服务器版本信息：`mysqladmin -u root -p version`
  2. 运行 `mysqld --console` 无法启动服务器
      - 问题描述：运行 `mysqld --console` 提示 `The innodb_system data file 'ibdata1' must be writable`
      - 解决方法：
        1. 输入命令 `net stop mysql` 停止服务
        2. 删除 `MySQL_Install_Path/data` 目录下面的两个文件：ib_logfile0 和 ib_logfile1
        3. 输入命令 `net start mysql` 重启服务



#### 登录
当 MySQL 服务已经运行时, 我们可以通过客户端工具登录到 MySQL 数据库中, 首先打开命令提示符, 输入以下格式的命令:
```shell script
mysql -h 主机名 -u 用户名 -p
```
参数说明：
> - -h -- 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0.1)该参数可以省略  
> - -u -- 登录的用户名  
> - -p -- 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项  
如果我们要登录本机的 MySQL 数据库，只需要输入以下命令即可：
```shell script
mysql -u root -p
```
按回车确认, 如果安装正确且 MySQL 正在运行, 会得到以下响应:
```
Enter password:
```
若密码存在, 输入密码登录, 不存在则直接按回车登录。  
登录成功后会得到以下响应：
```
Welcome to the MySQL monitor.
...
mysql>
```
输入 `exit` 或 `quit` 退出登录。


#### 修改密码
- 用 `mysqladmin` 在 Terminal 中修改
```shell script
mysqladmin -u用户名 -p旧密码 password 新密码
```
其中新密码也可以不输入，直接回车，提示的时候再输入，这样可以防止别人通过 dos 下的 doskey/history 查看到你的新密码。例如：
```shell script
mysqladmin -uroot -p123456 password 123
```

- `alter`  
首先登录MySQL，然后输入如下命令
```shell script
mysql> alter user 'root'@'localhost' identified by '123';
```
如果出现报错:
ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement
解决方案:
先执行
mysql> flush privileges;

- 用 `update` 直接编辑 user 表  
首先登录MySQL，然后输入如下命令
```shell script
mysql> use mysql;  # 连接权限数据库
mysql> update user set password=password('123') where user='root' and host='localhost';
mysql> flush privileges;  # 刷新权限
```

- 在忘记 root 密码的时候，可以暂时跳过权限表认证（以 windows 为例）   
  1. 打开 DOS 窗口，关闭正在运行的 MySQL 服务
    ```shell script
    net stop mysql
    ```
  2. 使用如下命令启动 MySQL 服务器
    ```shell script
    mysqld --console --skip-grant-tables --shared-memory
    ```
  启动成功后会得到以下响应：
    ```
    2021-06-04T05:42:35.492174Z 0 [System] [MY-010116] [Server] D:\mysql-8.0.16-winx64\bin\mysqld.exe (mysqld 8.0.16) starting as process 10356
    2021-06-04T05:42:38.504308Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
    2021-06-04T05:42:38.544979Z 0 [System] [MY-010931] [Server] D:\mysql-8.0.16-winx64\bin\mysqld.exe: ready for connections. Version: '8.0.16'  socket: ''  port: 0  MySQL Community Server - GPL.
    2021-06-04T05:42:38.591062Z 0 [Warning] [MY-011311] [Server] Plugin mysqlx reported: 'All I/O interfaces are disabled, X Protocol won't be accessible'
    ```
  2. 再开一个 DOS 窗口，使用如下命令回车登录即可
    ```
    mysql -uroot -p
    ```


#### Navicat 连接报错
- 问题描述：当使用 Navicat 等客户端连接 MySQL 的时候出现 `Authentication plugin 'caching_sha2_password' cannot be loaded:找不到指定的模块` 的错误
- 解决方法：
    - 方法一  
        执行如下语句：
        ```shell script
        mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'abc@123' PASSWORD EXPIRE NEVER;
        mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'abc@123';
        mysql> FLUSH PRIVILEGES;
        
        mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'abc@123';
        mysql> FLUSH PRIVILEGES;
        ```
    - 方法二
        1. 看样子是验证插件问题的，输入如下命令排查下：  
            ```shell script
            mysql> SELECT `user`, `host`, `authentication_string`, `plugin` FROM mysql.user;
            ```
            输出：
            ```
            +------------------+-----------+------------------------------------------------------------------------+-----------------------+
            
            | user             | host      | authentication_string                                                  | plugin                |
            
            +------------------+-----------+------------------------------------------------------------------------+-----------------------+
            
            | archiver         | %         | $A$005$==t@l=SP'G{U[1})D8yLwA6ti2uHtmUKNuHxQSUggrBRMBR2CheCw0Oxad9 | caching_sha2_password |
            
            | mysql.infoschema | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              | mysql_native_password |
            
            | mysql.session    | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              | mysql_native_password |
            
            | mysql.sys        | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              | mysql_native_password |
            
            | root             | localhost | $A$005$==t@l=SP'G{U[1})D8yLwA6ti2uHtmUKNuHxQSUggrBRMBR2CheCw0Oxad9 | caching_sha2_password |
            
            +------------------+-----------+------------------------------------------------------------------------+-----------------------+
            
            5 rows in set (0.06 sec)
            ```
        2. 修改 arhiver 账号的密码验证插件类型，输入如下命令：  
            ```shell script
            mysql> ALTER USER 'archiver'@'%' IDENTIFIED WITH mysql_native_password BY 'archiver';
            mysql> flush privileges;
            ```
        3. 再次排查，输入命令：  
            ```shell script
            mysql> SELECT `user`, `host`, `authentication_string`, `plugin` FROM mysql.user;
            ```
            输出：
            ```
            +------------------+-----------+------------------------------------------------------------------------+-----------------------+
            
            | user             | host      | authentication_string                                                  | plugin                |
            
            +------------------+-----------+------------------------------------------------------------------------+-----------------------+
            
            | archiver         | %         | *13D295FD7B8108ABBC89FCDDD342FFBFF5DA803C                              | mysql_native_password |
            
            | mysql.infoschema | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              | mysql_native_password |
            
            | mysql.session    | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              | mysql_native_password |
            
            | mysql.sys        | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE                              | mysql_native_password |
            
            | root             | localhost | $A$005$==t@l=SP'G{U[1})D8yLwA6ti2uHtmUKNuHxQSUggrBRMBR2CheCw0Oxad9 | caching_sha2_password |
            
            +------------------+-----------+------------------------------------------------------------------------+-----------------------+
            
            5 rows in set (0.06 sec)
            ```
        4. 重新用 Navicat 连接，即可正常使用。  
        参考链接：<https://blog.51cto.com/lee90/2139698>

