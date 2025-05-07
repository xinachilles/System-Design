# Queue -ACE

Created: 2021-04-26 11:42:13 -0600

Modified: 2021-04-26 12:48:20 -0600

---

![O Incognito (3) Message Queuing Producer Producer Producer Consumer Consumer @ acecodeinterview.com ](../media/Queue-Queue--ACE-image1.png){width="10.083333333333334in" height="6.145833333333333in"}



Shared one queue



![u00 u00 qns/qnd 84 - 一 dna J00f1POJd JOOnPOJd ](../media/Queue-Queue--ACE-image2.png){width="10.083333333333334in" height="5.59375in"}







![RabbitMQ O Message Broker Apache Kafka o o O Streaming Platform ](../media/Queue-Queue--ACE-image3.png){width="10.083333333333334in" height="5.572916666666667in"}









Rabbit MQ, queue will push the message to consumer side

Rabbit will delete the message

Kakfa more like a log will store the message in the disk or file system and do other analysis or process



![•10011PO•1d 」 8 OJd 」 8 Old ](../media/Queue-Queue--ACE-image4.png){width="10.083333333333334in" height="7.84375in"}![Kafka Consumer Topic 2 Consumer Group 1 Consumer Consumer Consumer Group 2 Consumer ](../media/Queue-Queue--ACE-image5.png){width="10.083333333333334in" height="6.416666666666667in"}

Same kafka consumer will share the same topic point



![Anatomy of a Topic 11 0123456789 01 Partition O Partition 1 Partition 2 Old Writes New ](../media/Queue-Queue--ACE-image6.png){width="10.083333333333334in" height="6.822916666666667in"}

Topic has too many message need to be shared to different partition





![思 考 题 一 Kafka 支 持 什 么 量 级 的 pi ？ Partition 数 量 更 关 键 不 超 过 百 万 级 如 果 你 有 这 个 疑 问 ， 你 可 能 已 经 错 误 地 使 用 了 Kafka ](../media/Queue-Queue--ACE-image7.png){width="10.083333333333334in" height="9.9375in"}![0 思 考 题 二 假 设 服 务 有 亿 级 用 户 ， 用 户 每 数 秒 发 布 位 置 信 息 ， 我 们 要 通 过 Kafka 收 集 处 理 这 些 位 置 信 息 。 如 何 配 置 Kafka Topic & Partition? Topic: user 」 ocation Partition by: User ID ](../media/Queue-Queue--ACE-image8.png){width="10.083333333333334in" height="8.854166666666666in"}








