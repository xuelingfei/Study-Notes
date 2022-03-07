其他客户端连接本机运行程序进行调试

1. 查询本机ip地址
2. 项目settings文件ALLOWED_HOSTS中加上本机ip
3. runserver中的host改为本机ip后再运行程序

否则会报java.net.ConnectException: failed to connect to xxxx