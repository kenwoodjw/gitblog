# [Kubernetes网络](https://github.com/kenwoodjw/gitblog/issues/6)


### 常见术语

- 第2层网络: OSI模型的“数据链"层.
- 第3层网络: OSI模型的”网络" 层，IPv4,IPv6,ICMP
- VXLAN: VLAN仅限于4096个网络ID，VXLAN用于在UDP数据包中封装第2层以太网帧帮助实现大型云部署，VXLAN是一种overlay协议
- overlay网络: 建立在现有网络之上的虚拟逻辑网络，用于现有网络之上提供有用的抽象，并分离和保护不同的逻辑网络
- 封装: 是指在附加层中封装网络数据包以提供其他上下文和信息的过程。在overlay网络中，封装被用于从虚拟网络转换到底层地址空间，从而能路由到不同位置
- 网状网络: 每个节点连接到许多其他节点已协作路由，并实现更大连接的网络
- BGP: 边界网关协议，用于管理边缘路由器之间数据包的路由方式，BGP通过考虑可用路径，路由规则和特定网络策略等因素，将数据包从一个网络发送到另一个网络

### 前置知识
#### network namespace

- 概念: 网络命名空间（Network Namespace）是 Linux 提供的一种虚拟化技术，用于隔离网络环境，每个网络命名空间都有自己的网络设备、IP 地址、路由表、/proc/net 目录以及防火墙规则等。这样，不同命名空间中的网络设置不会相互影响，为创建隔离的网络环境和测试环境提供了便利
-  如何创建network namespace
```sh
 ip netns add red
 ip netns add blue
 ip netns
```
- 在network namespace中执行命令
```sh
ip netns exec red ip link = ip -n red link
# 查看arp
ip netns exec red arp
# 如果提示没有arp命令
ip netns exec red ip neighbor
```

- 创建 veth pair 的用途：将 veth pair 的一端放置在一个命名空间中，另一端放置在另一个命名空间中，从而实现这两个命名空间之间的通信。
```sh
# 创建网络命名空间
ip netns add red
ip netns add blue
# 创建 veth pair
ip link add veth-red type veth peer name veth-blue
# 将 veth-red 接口移动到 red 命名空间
ip link set veth-red netns red
# 将 veth-blue 接口移动到 blue 命名空间
ip link set veth-blue netns blue
# 在 red 命名空间中配置 veth-red 接口
ip netns exec red ip addr add 192.168.1.1/24 dev veth-red
ip netns exec red ip link set veth-red up
# 在 blue 命名空间中配置 veth-blue 接口
ip netns exec blue ip addr add 192.168.1.2/24 dev veth-blue
ip netns exec blue ip link set veth-blue up
# 验证配置
ip netns exec red ping 192.168.1.2
ip netns exec blue ping 192.168.1.1
```
![veth-pair](https://github.com/kenwoodjw/gitblog/assets/10386710/da132fa8-b9cc-43f6-85a1-3ebdcda8617b)

