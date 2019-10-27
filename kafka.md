---
description: configuration
---

# Kafka

## 简介

* broker：服务器，装topic的地方，一个kafka集群由若干个broker组成
* topic：理解成一个MQ
* partition：topic被分成多个partition，kafka保证一个partition内的消息有序，但不保证多个partition之间的消息有序
* offset：每条消息都会有对应的partition以及offset
* producer：生产者
* consumer：消费者
* consumer group（CG）：kafka通过组的概念实现广播（每个人都能收到，consumer在不同的CG内），单播（一个人听到即可，consumer在同一个CG内）

参考资料：

* [kafka工作原理](http://xstarcd.github.io/wiki/Cloud/kafka_Working_Principles.html)

## Producer

* batch\_size：凑够这么多字节的信息才会发送当前数据包（含多个message）
* liinger\_ms：等待这么多秒才会发送当前数据包（含多个message）

以上两个参数满足其一就会发送数据。

2019-09-18 虫洞故障，平台侧kafka接收大量的小包信息，每台机器接收不足100M的数据，CPU使用率高达40%，计算集中在拆包上。

## Consumer

auto.offset.reset

* earliest：当无提交的offset时，从最早的offset开始消费
* latest：当无提交的offset时，从最新的offset开始消费

注意是无提交的offset时，如果之前提交过，可以无视这个参数

## kafka-python

开启kafka的logger

```python
import logging
import sys
logger = logging.getLogger('kafka')
logger.addHandler(logging.StreamHandler(sys.stdout))
logger.setLevel(logging.DEBUG)
```

默认情况下，kafka将log输入到NullHandler中了





