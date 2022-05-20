# MySQL 安装和使用

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [安装](#安装mysql-8)
- [登录](#登录)
- [修改密码](#修改密码)
- [创建用户与授权](#创建用户与授权)
- [Navicat 连接报错](#navicat-连接报错)

</details>

## 安装(MySQL 8+)

1. 前往 <https://dev.mysql.com/downloads/mysql/> 下载压缩包

2. 将 zip 包解压到相应的目录，比如 `D:\mysql-8.0.16-winx64`

3. 打开解压的文件夹，在该文件夹下创建 my.ini 配置文件，编辑 my.ini 配置一下基本信息：

   ```ini
   [mysql]
   # 设置 mysql 客户端默认字符集
   default-character-set=utf8mb4
   [mysqld]
   # 设置端口号
   port=3306
   # 设置 mysql 的安装目录
   basedir=D:\\mysql-8.0.16-winx64
   # 允许最大连接数
   max-connections=20
   # 服务端使用的字符集默认为 8 比特编码的 latin1 字符集
   character-set-server=utf8mb4
   # 创建新表时将使用的默认存储引擎
   default-storage-engine=INNODB
   ```

4. 将 `D:\mysql-8.0.16-winx64\bin` 添加到系统环境变量中

5. 初始化数据库

   ```shell
   mysqld --initialize --console
   ```

   执行完成后，会输出 root 用户的初始默认密码，如

   ```text
   ...
   2018-04-20T02:35:05.464644Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: APWCY5ws&hjQ
   ...
   ```

   其中 `APWCY5ws&hjQ` 就是初始密码。

6. 输入以下安装命令

   ```shell
   mysqld install
   ```

7. 启动及停止服务

   ```shell
   net start mysql  # 启动
   net stop mysql  # 停止
   ```

   或

   ```shell
   mysqld --console  # 启动服务器
   mysqladmin -uroot -p shutdown  # 关闭服务器
   ```

附：

1. 相关命令

   | 功能               | 命令                                |
   | ------------------ | ----------------------------------- |
   | 显示服务器状态     | `mysqladmin -u root -p status`      |
   | 查看活动线程       | `mysqladmin -u root -p processlist` |
   | 显示服务器版本信息 | `mysqladmin -u root -p version`     |

2. 问题解决
   - 问题描述：运行 `mysqld --console` 提示 `The innodb_system data file 'ibdata1' must be writable`
   - 解决方法：
     1. 输入命令 `net stop mysql` 停止服务
     2. 删除 `MySQL_Install_Path/data` 目录下面的两个文件：ib_logfile0 和 ib_logfile1
     3. 输入命令 `net start mysql` 重启服务

## 登录

打开命令提示符，输入如下格式的命令

```shell
mysql -h 主机名 -u 用户名 -p 密码
```

参数说明：

- `-h` -- 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0.1)该参数可以省略
- `-u` -- 登录的用户名
- `-p` -- 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项

其中密码也可以不输入，直接回车，提示的时候再输入，这样可以防止别人通过 dos 下的 doskey/history 查看到你的新密码。  
比如登录本机的 MySQL 数据库，只需要输入以下命令即可：

```shell
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

## 修改密码

- 用 `mysqladmin` 在 Terminal 中修改

  ```shell
  mysqladmin -u用户名 -p旧密码 password 新密码
  ```

  其中新密码也可以不输入，直接回车，提示的时候再输入。

- `alter`

  首先登录 MySQL，然后输入如下命令

  ```shell
  mysql> flush privileges;
  mysql> alter user 'root'@'localhost' identified by '123';
  ```

- 用 `update` 直接编辑 user 表

  首先登录 MySQL，然后输入如下命令

  ```shell
  mysql> use mysql;  # 连接权限数据库
  mysql> update user set password=password('123') where user='root' and host='localhost';
  mysql> flush privileges;  # 刷新权限
  ```

注：在忘记 root 密码的时候，可以暂时跳过权限表认证（以 windows 为例）

1. 打开 DOS 窗口，关闭正在运行的 MySQL 服务

   ```shell
   net stop mysql
   ```

2. 使用如下命令启动 MySQL 服务器

   ```shell
   mysqld --console --skip-grant-tables --shared-memory
   ```

   启动成功后会得到以下响应：

   ```
   2021-06-04T05:42:35.492174Z 0 [System] [MY-010116] [Server] D:\mysql-8.0.16-winx64\bin\mysqld.exe (mysqld 8.0.16) starting as process 10356
   2021-06-04T05:42:38.504308Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
   2021-06-04T05:42:38.544979Z 0 [System] [MY-010931] [Server] D:\mysql-8.0.16-winx64\bin\mysqld.exe: ready for connections. Version: '8.0.16'  socket: ''  port: 0  MySQL Community Server - GPL.
   2021-06-04T05:42:38.591062Z 0 [Warning] [MY-011311] [Server] Plugin mysqlx reported: 'All I/O interfaces are disabled, X Protocol won't be accessible'
   ```

3. 再开一个 DOS 窗口，使用如下命令回车登录即可

   ```shell
   mysql -uroot -p
   ```

## 创建用户与授权

1. 创建用户

   先用 root 用户登录 MySQL，然后输入如下命令：

   ```sql
   mysql> create user 'username'@'host' identified by 'password';
   ```

   参数说明：

   - username -- 将创建的用户名
   - host -- 指定该用户在哪个主机上可以登陆，如果是本地可用 localhost，如果想让该用户可从任意远程主机登陆，则可使用通配符 `%`
   - password -- 该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

   示例：

   ```sql
   mysql> create user 'kylin'@'localhost' identified by '123456';
   mysql> create user 'kylin'@'%' identified by '123456';
   mysql> create user 'kylin'@'%' identified by '';
   mysql> create user 'kylin'@'%';
   ```

2. 授权

   命令：

   ```sql
   mysql> GRANT privileges ON databasename.tablename TO 'username'@'host'
   ```

   参数说明：

   - privileges -- 用户的操作权限，如 `SELECT`，`INSERT`，`UPDATE` 等，如果要授予所的权限则使用 `ALL`
   - databasename -- 数据库名
   - tablename -- 表名，如果要授予该用户对所有数据库和表的相应操作权限则可用 `*` 表示，如 `*.*`

   示例：

   ```sql
   mysql> GRANT SELECT, INSERT ON test.user TO 'kylin'@'%';
   mysql> GRANT ALL ON _._ TO 'kylin'@'%';
   ```

   注意：用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:

   ```sql
   mysql> GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
   ```

3. 设置与更改用户密码

   命令：

   ```sql
   mysql> SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
   ```

   如果是当前登陆用户用：

   ```sql
   mysql> SET PASSWORD = PASSWORD("newpassword");
   ```

4. 撤销用户权限

   命令：

   ```sql
   mysql> REVOKE privilege ON databasename.tablename FROM 'username'@'host';
   ```

   注意：  
   假如在给用户授权的时候使用的是 `GRANT SELECT ON test.user TO 'kylin'@'%'`，则使用 `REVOKE SELECT ON _._ FROM 'kylin'@'%';` 命令并不能撤销该用户对 test 数据库中 user 表的 SELECT 操作；  
   相反，如果授权使用的是 `GRANT SELECT ON _._ TO 'kylin'@'%';`，则使用 `REVOKE SELECT ON test.user FROM 'kylin'@'%';` 命令也不能撤销该用户对 test 数据库中 user 表的 SELECT 权限。  
    具体信息可以用命令 `SHOW GRANTS FOR 'kylin'@'%';` 查看。

5. 删除用户

   ```sql
   mysql> DROP USER 'username'@'host';
   ```

参考链接：<https://www.jianshu.com/p/d7b9c468f20d>

## Navicat 连接报错

- 问题描述：当使用 Navicat 等客户端连接 MySQL 的时候出现如下错误

  ```text
  Authentication plugin 'caching_sha2_password' cannot be loaded:找不到指定的模块
  ```

- 解决方法：

  - 方法一

    执行如下语句：

    ```shell
    mysql> alter user 'root'@'localhost' identified by 'abc@123' password expire never;
    mysql> alter user 'root'@'localhost' identified with mysql_native_password BY 'abc@123';
    mysql> flush privileges;

    mysql> alter user 'root'@'localhost' identified by 'abc@123';
    mysql> flush privileges;
    ```

  - 方法二

    1. 可能是验证插件问题的，输入如下命令排查下：

       ```shell
       mysql> select `user`, `host`, `authentication_string`, `plugin` from mysql.user;
       ```

       输出：

       ```text
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

       ```shell
       mysql> alter user 'archiver'@'%' identified with mysql_native_password by 'archiver';
       mysql> flush privileges;
       ```

    3. 再次排查，输入命令：

       ```shell
       mysql> select `user`, `host`, `authentication_string`, `plugin` from mysql.user;
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
