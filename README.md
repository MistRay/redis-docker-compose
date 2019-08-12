# redis-docker-compose
`使用docker-compose一键搭建Redis集群`

### 简介

大部分情况开发者不会在本地搭建集群.但有时在我们需要进行集群API测试,对集群性能及可用性验证,学习Redis集群特性等情况下
需要在本地搭建Redis集群,本项目诣在帮助没有集群部署经验的开发人员可以快速在本地搭建Redis集群

### Redis集群
Redis集群可以把数据分散存储到n个节点中,同时可以对每个节点做备份,来保障Redis数据的高可用和稳定性

![RedisCluster](https://rancher.com/img/blog/2018/deploying-redis-cluster/01-rancher-redis-cluster-architecture.png)

### 快速开始

首先需要安装[docker](https://www.docker.com/),[docker-compose](https://docs.docker.com/compose/install/)及[python3.X](https://www.python.org/downloads/)

```
docker-compose up -d 

docker exec -it node-80 redis-cli -p 6380 --cluster create 172.16.238.10:6380 172.16.238.11:6381 172.16.238.12:6382 172.16.238.13:6383 172.16.238.14:6384 172.16.238.15:6385 --cluster-replicas 1
```

### 说明

* 使用Redis版本为5.0.5-alpine,如有需要请自行更改.  
> redis官方在redis3.x和redis4.x提供了redis-trib.rb工具方便我们快速搭建集群,在redis5.x中更是可以直接使用redis-cli命令来直接完成集群的一键搭建,省去了redis-trib.rb依赖ruby环境的问题。
* docker-compose自动组网
> 使用docker-compose up启动容器后，这些容器都会被加入`{app_name}_default`网络中,但是ip不固定.所以在docker-compose内使用了指定ip地址的方式,使每个容器的ip为定值.
* client端节点互通问题  
在redis配置文件中有如下配置:
``` shell
# docker虚拟网卡的ip
cluster-announce-ip 10.1.1.5
# 节点映射端口
cluster-announce-port 6379
# 总线映射端口,通常为节点映射端口前加1
cluster-announce-bus-port 16379
```
> docker虚拟网卡地址为docker与外界互通所使用的虚拟ip,如果没有上述配置,在client端操作时,会有取docker内网ip的问题.
* 关于时区,项目内默认使用了上海时区,需要使用其他时区的同学请自行更改.
```yaml
    environment:
      # 设置时区为上海，否则时间会有问题
      - TZ=Asia/Shanghai
```
