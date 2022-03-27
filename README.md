# Wireproxy

将Wireguard设置为为socks5代理或隧道的Wireguard客户端

# 这个程序是什么？

Wireproxy是一个完全用户空间的应用程序，它连接到WireGuard。并在机器上公开一个socks5代理或隧道。如果你需要通过WireGuard连接到某些网站，但又不想设置一个新的网络接口。

# 为什么你可能需要这个

- 你只是想通过wireguard来代理一些流量
- 你不需要root权限来改变wireguard的设置。

目前我正在运行wireproxy，连接到另一个国家的WireGuard服务器。并将我的浏览器配置为对某些网站使用Wireproxy。这是很有用的，因为Wireproxy与我的网络接口完全隔离，而且我不需要root权限来配置任何东西。

# 使用方法

`./wireproxy [配置文件位置]`

# 配置文件示例

```
# SelfSecretKey 为WireGuard的私钥
SelfSecretKey = uCTIK+56CPyCvwJxmU5dBfuyJvPuSXAq1FzHdnIxe1Q=
# SelfEndpoint 是你的WireGuard的IP
SelfEndpoint = 172.16.31.2
# PeerPublicKey 是你想连接的wireguard服务器的公钥
PeerPublicKey = QP+A67Z2UBrMgvNIdHv8gPel5URWNLS4B3ZQ2hQIZlg=
# PeerEndpoint 是你想连接的wireguard服务器的EndPoint IP
PeerEndpoint = 172.16.0.1:53
# DNS is the nameservers that will be used by wireproxy.
DNS = 1.1.1.1
# KeepAlive 是Wireuard设备的守护时间间隔，一般不需要设置
# KeepAlive = 25
# PreSharedKey是你的Wireguard设备的预共享密钥，如果你不知道这是什么，你就不需要它了
# PreSharedKey = UItQuvLsyh50ucXHfjF0bbR4IIpVBd74lwKc8uIPXXs=

# TCPClientTunnel 是一个在你的机器上监听的隧道，并将收到的任何TCP流量通过wireguard转发给指定的目标。
# 你的局域网上的一些应用程序 -> 127.0.0.1:25565 --> WireGuard --> play.cubecraft.net:25565
[TCPClientTunnel]
BindAddress = 127.0.0.1:25565
Target = play.cubecraft.net:25565

# TCPServerTunnel是一个在wireguard上监听的隧道，并通过本地网络将收到的任何TCP流量转发到指定的目标。
# 你的wireguard网络上的一些应用 --> WireGuard --> 172.16.31.2:3422 -> localhost:25545
[TCPServerTunnel]
ListenPort = 3422
Target = localhost:25545

# 在你的局域网上创建一个socks5代理，通过Socks5的流量都将通过wireguard进行路由。
[Socks5]
BindAddress = 127.0.0.1:25344
```

## 感谢

原作：https://github.com/octeep/wireproxy
