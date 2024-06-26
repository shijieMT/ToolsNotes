# etcd了解及安装
## 了解etcd
> etcd是一个分布式的键值存储系统,常用于服务发现、配置共享、分布式锁等场景。
### 1. 服务发现
> 1. 服务发现(Service Discovery) 在分布式系统中,服务通常部署在多台机器上。  
> 2. 服务发现就是一种自动发现和连接这些服务实例的机制。它允许服务消费者通过服务名称而不是具体的网络地址来访问服务。
> 3. 服务发现的关键组件是服务注册中心。服务提供者在启动时将自己的网络地址注册到服务注册中心,服务消费者从服务注册中心查询服务地址,然后直接请求服务。
> 4. 常见的服务发现系统有Zookeeper、Consul、Eureka等。
> 5. 服务发现简化了分布式系统的部署和运维。即使服务的网络地址发生变化,只要及时更新注册中心,消费者就可以动态感知并连接到新的服务实例。
### 2. 配置共享
> 1. 配置共享(Configuration Sharing) 配置共享指的是将应用的配置信息存储在一个集中的配置中心,所有节点都从配置中心获取配置。这样可以在运行时动态修改配置,所有节点都能及时生效,无需重启服务。
> 2. 常见的配置共享系统有Spring Cloud Config、Apollo、Nacos等。它们通常提供了修改配置的管理界面或API,以及配置变更的实时推送机制。
> 3. 使用配置共享可以避免在多个节点上重复配置,方便集中管理。同时将配置和代码解耦,提高了系统的灵活性。
### 3. 分布式锁
> 1. 分布式锁(Distributed Lock) 在分布式环境下,多个节点同时访问共享资源时,需要通过锁来协调以避免并发问题。与单机环境下的锁不同,分布式锁需要保证在整个系统范围内是唯一的。
> 2. 分布式锁的实现方式有多种,比如基于数据库、缓存(如Redis)、Zookeeper等。它们通过一些机制(如数据库唯一约束、setnx命令、临时顺序节点等)来保证在分布式环境下同一时刻只有一个客户端能获取到锁。
> 3. 使用分布式锁可以防止多个节点同时修改共享数据导致的不一致问题,是实现分布式事务、Leader选举等场景的重要手段。
## etcd的优点
1. 强一致性:etcd基于Raft共识算法实现,保证了数据的强一致性。在etcd集群中,所有节点都会保存完整的数据,写操作需要经过多数节点的同意才能完成,避免了数据不一致的问题。
2. 高可用:etcd采用多节点部署,即使部分节点失效,整个集群仍能正常工作。etcd支持动态添加和删除节点,方便进行集群扩容和缩容。
3. 高性能:etcd采用Go语言开发,性能优异。etcd支持并发读写,并针对读多写少的场景进行了优化。etcd还提供了Watch机制,可以监听键值的变化,实现高效的事件通知。
4. 安全性:etcd支持SSL/TLS加密通信,保证数据传输的安全性。etcd还提供了基于角色的访问控制(RBAC),可以对不同用户和角色进行权限管理。
5. 易用性:etcd提供了RESTful API和gRPC API,方便不同语言的客户端进行调用。etcd还提供了命令行工具etcdctl,可以方便地进行数据操作和集群管理。
6. 丰富的特性:除了基本的键值存储功能,etcd还支持TTL(Time To Live)、事务、多版本控制等特性,满足了不同场景下的需求。
7. 活跃的社区:etcd由CoreOS(已被Red Hat收购)开源,拥有活跃的社区和广泛的用户基础。许多知名项目如Kubernetes、Rook、Vitess等都依赖etcd进行服务发现和配置管理。
## windows安装etcd
### 1. 下载
[github发布界面](https://github.com/etcd-io/etcd/releases)
> 安装后添加环境变量，例如：C:\etcd
> 验证是否安装成功
> ```shell
> etcd --version
> ```
### 2. 启动etcd
> [!WARNING]  
> 不管哪种方式启动etcd，要把启动窗口保留下来，不然后续输入命令无法执行
1. shell启动
```shell
etcd.exe --listen-client-urls http://0.0.0.0:2379 --advertise-client-urls http://0.0.0.0:2379
```
2. 点击etcd.exe启动
### 3. 查看etcd是否启动成功（确定成功可以忽略）
使用endpoint health命令来检查etcd服务的健康状态，健康则可以使用
```shell
etcdctl --write-out=table endpoint health
```
## Linux todo
[源码安装](https://blog.csdn.net/Mr_XiMu/article/details/127923827)
yum安装,版本一般比较老
## Docker todo
```shell
docker run --name etcd -d -p 2379:2379 -p 2380:2380 -e ALLOW_NONE_AUTHENTICATION=yes bitnami/etcd:3.3.11 etcd 
```

