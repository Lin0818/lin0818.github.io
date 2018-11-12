---
layout: post
title: "docker容器运行kafka"
subtitle: "docker run kafka"
author: Lin0818
header-style: text
tags:
  - kafka
  - docker
  - zookeeper
---

使用了[wurstmeister](https://github.com/wurstmeister/kafka-docker)提供的docker镜像。git上提供了详细的docker-composer的启动方法，我在自己的Mac环境中只使用了简单的`docker run`的方式进行启动，可以快速的进行测试。

一、下载kafka，zookeeper镜像

`$ docker search kafka`

`$ docker pull wurstmeister/zookeeper`

`$ docker pull wurstmeister/kafka`

二、启动镜像
1. 先启动zookeeper
`$ docker run -d --name zookeeper -p 2181:2181 wurstmeister/zookeeper:latest`

2. 再启动kafka
`$ docker run -d --name kafka -p 9092:9092 -e KAFKA_BROKER_ID=0 -e KAFKA_ZOOKEEPER_CONNECT=10.12.161.207:2181 -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://10.12.161.207:9092 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -t wurstmeister/kafka`

三、测试收发消息
* 进入docker容器内
```
`$ docker exec -it ${CONTAINER ID} /bin/bash `
`$ cd /opt/kafka_2.11-0.10.1.1/`
```

* 创建topic
`$ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test`

* 查询创建的topic
`$ bin/kafka-topics.sh --list --zookeeper localhost:2181`

* 创建一个消费者
`$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning`

* 创建一个生产者
`$ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test`