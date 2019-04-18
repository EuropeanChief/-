# Redis学习一
## 1. 优势
- 性能极高
- 数据类型：String、list、hash、set、zset
- 操作原子性：要么都成功，要么不执行
- 特性：支持publish/shubscribe，通知，key 过期等等特性。

## 2.配置
    redis的配置文件名为redis.conf，可以通过config命令查看、设置配置项。
### - timeout
    设置客户端连接时间，单位是秒。设置为0表示关闭该功能。
### - loglevel
    指定日志记录级别，log分为4个级别：debug，verbose，notice，warning
    1. debug：调试级别，一般适用于开发/测试
    2. verbose：有许多无用的信息，没有debug混乱，属于默认模式
    3. notice：适用于生产模式
    4. warning：只记录非常重要或者关键的消息
### - daemonize
    可以设置redis后台运行，默认no（关闭）设置成yes可以开启
    后台运行开启后可以通过pidfil命令设置pid文件，默认放在/var/run/redis.pid，运行多个服务时，需要指定不同到pid文件及端口(默认6379)。
### - 

- 查看
    config get key（*查看所有）
- 设置
    config set key value
