# Queue -ACE



---

![O Incognito (3) Message Queuing Producer Producer Producer Consumer Consumer @ acecodeinterview.com ](../media/Queue-Queue--ACE-image1.png)



Shared one queue



![u00 u00 qns/qnd 84 - 一 dna J00f1POJd JOOnPOJd ](../media/Queue-Queue--ACE-image2.png)







![RabbitMQ O Message Broker Apache Kafka o o O Streaming Platform ](../media/Queue-Queue--ACE-image3.png)









Rabbit MQ, queue will push the message to consumer side

Rabbit will delete the message

Kakfa more like a log will store the message in the disk or file system and do other analysis or process



![•10011PO•1d 」 8 OJd 」 8 Old ](../media/Queue-Queue--ACE-image4.png)![Kafka Consumer Topic 2 Consumer Group 1 Consumer Consumer Consumer Group 2 Consumer ](../media/Queue-Queue--ACE-image5.png)

Same kafka consumer will share the same topic point



![Anatomy of a Topic 11 0123456789 01 Partition O Partition 1 Partition 2 Old Writes New ](../media/Queue-Queue--ACE-image6.png)

Topic has too many message need to be shared to different partition





![思 考 题 一 Kafka 支 持 什 么 量 级 的 pi ？ Partition 数 量 更 关 键 不 超 过 百 万 级 如 果 你 有 这 个 疑 问 ， 你 可 能 已 经 错 误 地 使 用 了 Kafka ](../media/Queue-Queue--ACE-image7.png)![0 思 考 题 二 假 设 服 务 有 亿 级 用 户 ， 用 户 每 数 秒 发 布 位 置 信 息 ， 我 们 要 通 过 Kafka 收 集 处 理 这 些 位 置 信 息 。 如 何 配 置 Kafka Topic & Partition? Topic: user 」 ocation Partition by: User ID ](../media/Queue-Queue--ACE-image8.png)








