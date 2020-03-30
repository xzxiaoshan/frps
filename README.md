# Frp 服务端

**1、编译生成镜像**

```
cd Dockerfile文件所在的目录
docker build -t xzxiaoshan/frps .
```

**2、创建配置文件**

在目录 /opt/test/frp/server (目录自便) 中创建配置文件 frps.ini
下面是示例，更多详见[官网](https://github.com/fatedier/frp/blob/master/README_zh.md)  

```
[common]
#服务器IP，0.0.0.0为服务器全局所有IP可用，假如你的服务器有多个IP则可以这样做，或者填写为指定其中的一个服务器IP,支持IPV6
bind_addr = 0.0.0.0
#通讯端口，用于和客户端内网穿透传输数据的端口，可自定义
bind_port = 7000
#UDP通讯端口，用于点对点内网穿透
bind_udp_port = 7001
#用于KCP协议UDP通讯端口，在弱网环境下传输效率提升明显，但是会有一些额外的流量消耗。设置后frpc客户端须设置protocol = kcp
kcp_bind_port = 7000
#http监听端口，注意可能和服务器上其他服务用的80冲突，比如centos有些默认有Apache，可自定义
vhost_http_port = 8080
#https监听端口，可自定义
vhost_https_port = 8443
#通过浏览器查看 frp 的状态以及代理统计信息展示端口，可自定义
dashboard_port = 7500
#信息展示面板用户名
dashboard_user = admin
#信息展示面板密码
dashboard_pwd = admin123456
#最多保存多少天日志
log_max_days = 7
#认证密钥
token = admin123456789
#端口白名单，为了防止端口被滥用，可以手动指定允许哪些端口被使用
allow_ports = 1-65535
#每个内网穿透服务限制最大连接池上限，避免大量资源占用，可自定义
max_pool_count = 100
#frpc 所在机器和 frps 所在机器的时间相差不能超过 15 分钟，因为时间戳会被用于加密验证中，防止报文被劫持后被其他人利用,单位为秒，默认值为 900，即 15 分钟。如果
修改为 0，则 frps 将不对身份验证报文的时间戳进行超时校验。国外服务器由于时区的不同，时间会相差非常大，这里需要注意同步时间或者设置此值为0
authentication_timeout = 0
#日志输出路径(容器内路径)
log_file = /frp/frps.log
#日志输出级别(info|debug|error|warn)
log_level = info
```

**3、创建启动容器**

```
docker run -itd -p 17000:7000 -p 17500:7500 -v /opt/test/frp/server:/frp xzxiaoshan/frps /bin/sh
```

**4、访问**

浏览器打开 http://192.168.xx.xx:17500 输入账号密码(admin/admin123456)，成功打开表示OK。
