# Laravel Broadcast

基于 Go 实现的 `laravel-echo-server` 。在我司内部实现 `notification` 的时候，发现 `laravel-echo-server` 服务在大用户量下占用太多
的系统资源，于是就用 Go 重新造个轮子。

## Features

+ [x] Websocket服务
+ [x] Redis订阅
+ [x] 定时输出数据统计
+ [x] 心跳检测
+ [x] HTTPS
+ [x] 数据报表接口

## Usage

首先，您需要先将项目克隆岛本地：

```
git clone https://github.com/Qsnh/go-laravel-broadcast.git
```

然后您需要编译生成可执行程序：

```
# 安装程序所需要的依赖
go get -v ./...

# 编译生成可执行程序
go build -o client
```

接下来，您需要做一些配置：

```
cp .env.example .env
vi .env
```

env 文件配置如下：

```
# Laravel认证
AUTH_HOST=http://localhost
AUTH_PATH=/broadcasting/auth

# HTTPS
TLS_ENABLED=false
TLS_CERT_FILE=
TLS_KEY_FILE=

# WebSocket服务
WEBSOCKET_HOST=0.0.0.0
WEBSOCKET_PORT=8890
WEBSOCKET_PATH=/ws
WEBSOCKET_CHECK_ORIGIN=

# Redis配置
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
REDIS_PASSWORD=
SUBSCRIBE_CHANNELS=
```

其中的 `laravel` 认证不需要修改，默认即可，除非您修改了 `laravel broadcast` 的认证路由。如果您不需要用到 `https` 的话也就无需配置 `https` 相关的信息。
`websocket` 服务参数需要配置下，可能您需要配置下端口和ws的路径，另外为了安全，跨域一定要配置哦，多个域名请用逗号分隔。接下里是 `redis` 的环境配置，
redis 的环境配置上是必须得。因为 `laravel broadast` 的 redis 驱动是结合 `redis` 的 `sub/pub` 实现的，本程序需要订阅相关的 redis 频道来达到推送的目的。
其中，subscribe_channels 是您在 `laravel` 中的 `channel.php` 中定义的频道，注意在频道前面加上 `private` 等修饰符，支持整个表达式。

## More

+ 你可以通过请求 `/metrics` 来获取当前服务运行至今的统计数据，它返回了总计有多少个 client 链接，总结转发了多少个 `redis` 订阅消息，下面是它的返回内容：

```json
{"message":0,"client":0}
```

## Author

[小滕](https://github.com/Qsnh) | [我的博客](https://58hualong.cn) | [小滕全站教学视频](https://58hualong.com)

