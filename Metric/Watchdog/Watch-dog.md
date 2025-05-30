# Watch dog



---

![Event: user_api_response Region: dc_l Response code: 200 Count: 1 ](../../media/Metric-Watchdog-Watch-dog-image1.png)

















![功 能 性 需 求 非 功 能 性 需 求 非 需 求 1 · 2 理 解 需 求 记 录 事 件 发 生 的 数 量 查 询 事 件 数 量 的 TimeSeries 查 询 经 筛 选 后 事 件 数 量 的 TimeSeries 根 据 以 上 Time 设 置 警 报 阈 值 高 扩 展 性 ， 支 持 监 控 海 量 事 件 较 低 延 迟 ， 监 控 数 据 允 许 分 钟 级 延 迟 高 可 用 性 ， 监 控 系 统 随 时 访 问 可 靠 性 ， 生 成 相 对 正 确 的 监 控 数 据 可 靠 性 ， 老 数 据 精 度 可 以 酌 情 降 低 服 务 器 日 志 CallStackTracing ](../../media/Metric-Watchdog-Watch-dog-image2.png)







1.  Recorded the event number
2.  Time series --- how many event in each minutes, each hours ...
3.  Filter function or search function
4.  Set up a alarm base on the filter



Non function requirement:

1.  Suppoert milliion events
2.  Low lantency
3.  High available
4.  High accuracy for new data but reduce the accuracy for the old data







![假 设 瓶 颈 服 务 资 源 存 储 资 源 1 · 3 资 源 估 算 监 控 10 ， 0 种 事 件 每 个 事 件 每 秒 触 发 1 次 QPS ， 存 储 QPSIO ， * 1 ： 10M 一 周 原 始 数 据 10M 1 佣 B * 24 * 36 * 7 、 =600TB 五 年 小 时 级 数 据 10M 闐 B 24 * ％ 5 * 5 、 一 40TB ](../../media/Metric-Watchdog-Watch-dog-image3.png)

5 year data - save event per each hour

Raw data - save event per second





![数 据 流 事 件 采 集 事 件 整 合 传 递 事 件 存 储 事 件 读 取 ](../../media/Metric-Watchdog-Watch-dog-image4.png)









![事 件 整 合 传 递 客 户 端 整 合 服 务 器 整 合 ](../../media/Metric-Watchdog-Watch-dog-image5.png)







Save in kafa then combine together









![思 考 题 如 何 决 定 事 件 整 合 应 该 在 哪 个 阶 段 进 行 精 度 要 求 源 服 务 器 资 源 读 取 灵 活 性 ](../../media/Metric-Watchdog-Watch-dog-image6.png)





How to decide use client side combination or service side combination

1.  Accuracy requirements
2.  Can we combine the event on the source service
3.  Depend on the requirement -- need one minute data, one hour data ....



If we just need a minute event -- how many event in one minute, we can aggregative the data in client side and sent to downside



The machine level aggregate, we only can aggregate the event for this machine

If the service has multiple machine, we cannot aggregate them together on the client side

Event name and other meta data

2 type of event: 1. event count, 2 event length



![%tqofåvø-ri -معاي لدها-اMi٩ صمجه7م @ لمدهاه4eiمععه . جم ب مادلمهين -p;pøtiae€JL% كهمم؟ ممع؟ ط، ÅÄTYI- محط مم «لصو سمهD Collect-int ٨معم مممكه" علهى.ى 4).وء) حن ٣ صن ٣ فح ٣ --u.JÄ±-e ههنلمP عر:ءمP هيا معر عس ه rd?kimq لإمصاايحه ا eط S5te.A eمطوة امره ](../../media/Metric-Watchdog-Watch-dog-image7.png)



事件存储：

Elasticsearch

Or event DB (网站的solution

![5.1 Event Cache & DB Event ID Event Type I Value Time Window Timestamp ](../../media/Metric-Watchdog-Watch-dog-image8.png)



![local Ran ー ク no 汐 に 5 ム ・ 1. e け ー pe 。 ル な ( 応 も い 尾 e 伽 ( ド 「 ん も ル よ ル 1 名 6 立 . 31 " ( 辺 お t レ ー 0V2- よ れ / ん の 「 / re-oust 収 の そ 谷 , " れ / ィ / ム ツ ](../../media/Metric-Watchdog-Watch-dog-image9.png)![思 考 题 如 何 随 着 时 间 ， 降 低 存 储 数 据 的 精 度 ？ Database 存 储 定 期 合 并 Data Warehouse 长 期 存 储 原 始 数 据 ](../../media/Metric-Watchdog-Watch-dog-image10.png)

![思 考 题 能 不 能 不 使 用 Stream Processing 而 换 做 使 用 MapReduce? 因 为 要 求 低 延 迟 所 以 不 适 合 MapReduce ](../../media/Metric-Watchdog-Watch-dog-image11.png)



![思 考 题 如 何 实 现 长 度 类 的 事 件 监 控 和 归 并 ？ ](../../media/Metric-Watchdog-Watch-dog-image12.png)

(watch dog -2)





![Event: api-response-time Region: dc-I Response code: 200 Length in ms: 20 ](../../media/Metric-Watchdog-Watch-dog-image13.png)









![事 件 读 取 模 式 秒 / 分 钟 / 小 时 5 长 度 秒 / 分 钟 / 小 时 平 均 长 度 ](../../media/Metric-Watchdog-Watch-dog-image14.png)



![gov " 房 メ 半 霧 フ 卩 屮 + " 2 第 霧 ) 4 + " フ 笋 ル 尸 ノ ~ ソ " 。 " 房 メ 半 霧 フ み } + ょ つ 男 」 / 。 と d /oSd 4 物 4 で ユ 第 ル 尸 ノ の ~ ィ / v ノ 。 男 」 /0bd /0Sd 物 房 / つ ァ 叫 0 ](../../media/Metric-Watchdog-Watch-dog-image15.png)



3 option

First option is we just write the raw event to DB and client (metric consumer )will read the data base on the requirement

( on the database side, they have min level/ hour level/...)



Machine level -- client side

Data pipeline--- map-reduce/ service --stream process ---- for data aggregation: one pipeline for minute aggregation, one pipeline for hour aggregation









histogram -Fixed bucket: -- 0- 5 min or ms ,6-10 min/ ms , 11-15 min....

Or (first 5% , 5%-10% , 10%-15%) ---event count





Average : events count and total count

Map reduce need a couple of minutes to aggregate the date

We can use kafka stream --



Event name



![Time Series (RPC) get_event_time_series( event _ key, start time, end time, aggregation method, # sum granularity, # per-minute ](../../media/Metric-Watchdog-Watch-dog-image16.png)![Monitoring Library statsd.incr(event_key, value, metadata) statsd . timer (event _ key) . start( ) statsd . timer (event _ key) . end( ) ](../../media/Metric-Watchdog-Watch-dog-image17.png)![服 务 器 存 储 1.7 扩 展 性 源 服 务 器 部 署 轻 量 后 台 进 程 或 Lib Kafka 实 现 收 集 所 有 源 服 务 的 事 件 Kafka Consumer 进 行 事 件 处 理 事 件 服 务 器 数 据 定 期 压 缩 Elasticsearch 读 取 机 制 灵 活 ](../../media/Metric-Watchdog-Watch-dog-image18.png)![服 务 器 存 储 1 ． 8 容 灾 设 计 Kafka 容 灾 机 制 服 务 器 数 据 备 份 ](../../media/Metric-Watchdog-Watch-dog-image19.png)![Kafka Producer 负 载 1 ． 9 监 控 警 报 Kafka Consumer 服 务 器 读 写 QPS 负 载 ](../../media/Metric-Watchdog-Watch-dog-image20.png)




















