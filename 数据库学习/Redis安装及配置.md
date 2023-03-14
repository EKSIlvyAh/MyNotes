# Windows下安装Redis

## 下载Redis

[下载链接https://github.com/tporadowski/redis/releases](https://github.com/tporadowski/redis/releases)

进入网页后 选择 Redis-x64-5.0.14.1.msi 然后下载

下载之后只需要修改保存路径，和勾选添加到环境变量即可

## 修改配置文件

找到redis目录下的 **redis.windows-service.conf** 文件

然后找到以下几个选项，按需求更改

选项|含义
:-:|:-:|
bind [ip]|数据库服务器绑定的ip号（默认本机ip）
port [端口号]|运行所在端口号（默认6379）
dbfilename [文件名]|数据库文件名（默认dump.rdb）
dir [路径]|数据文件存储路径（默认./）
darabases [数量]|默认创建的数据库数量（默认16）
logfile [文件名]|日志文件名（默认server_log.txt）
requirepass [密码]|设置redis密码（默认没有）
slaveof|主从复制（类似双机备份）

同样的，使用客户端的话，找到redis目录下的 **redis.windows.conf** 文件

配置大体相同
