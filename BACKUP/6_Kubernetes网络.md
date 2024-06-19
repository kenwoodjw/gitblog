# [Kubernetes网络](https://github.com/kenwoodjw/gitblog/issues/6)

### 常见术语

- 第2层网络: OSI模型的“数据链"层.
- 第3层网络: OSI模型的”网络" 层，IPv4,IPv6,ICMP
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