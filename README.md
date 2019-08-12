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

关于时区,项目内默认使用了上海时区,需要使用其他时区的同学请自行更改.
