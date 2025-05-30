# Uber - ACE



---



![功 能 性 需 求 非 功 能 性 需 求 非 需 求 1 · 2 理 解 需 求 匹 配 乘 客 和 司 机 (Matching) 预 计 等 待 时 间 (ETA) 高 扩 展 性 ， 海 量 用 户 极 高 可 用 性 ， 随 时 能 打 车 能 乘 车 高 效 性 ， 合 理 匹 配 乘 客 和 司 机 定 价 (Pricing/Surge) ](../../media/Location-Service-Uber-Uber---ACE-image1.png)

There are two type of users in our system 1) Drivers 2) Customers.



1.  Drivers need to regularly notify the service (via websocket ) about their current location and their availability to pick passengers.



2.  Passengers are able to see all the nearby available drivers and return the ETA - estimated time arrival



3.  Passengers can request a ride; system will return all nearby drivers



4.  All drivers should be notified that a customer is ready to be picked up.



5.  Once a driver and customer accept a ride, they can constantly see each other's current location, until the trip finishes.



6.  Upon reaching the destination, the driver marks the trip complete and become available for the next ride.





Non function requirement

1.  Support million of user
2.  Low latency and efficiency, the passenger should able to see nerby available driver anytime. Should able to mach the driver and passenger very quickly











![假 设 瓶 颈 1 · 3 资 源 估 算 1 M MAU ， 平 均 每 人 每 月 叫 车 5 次 平 均 每 人 每 月 读 取 预 计 等 待 时 间 20 次 平 均 乘 车 时 长 巧 分 钟 每 4 秒 客 户 端 和 服 务 器 同 步 位 置 信 息 # ofOngoing Sessions 位 置 信 息 同 步 QPS 读 取 预 计 等 待 时 间 QPS ](../../media/Location-Service-Uber-Uber---ACE-image2.png)





![服 务 资 源 1 · 3 资 源 估 算 (cont.) # ofOngoing Sessions 1 M * 5 * 巧 * / （ 30 * 24 * 36 佣 ） 、 ： 170k 位 置 信 息 同 步 QPS 1 闐 M 5 巧 （ 60 / 4 ） / （ 30 24 * 3 ） 、 = 43k 匹 配 服 务 访 问 量 QPS 1 M 20 / （ 24 36 ） 、 = 7 佣 ](../../media/Location-Service-Uber-Uber---ACE-image3.png)





![0 DriverbyGeohash (RedisSortedSet) DriverLocation 司 机 位 置 缓 存 Sort By TS Geohash DriverID DriverID (Latitude ， Longitude) ](../../media/Location-Service-Uber-Uber---ACE-image4.png)



![1.4 系 统 设 计 图 到 达 时 间 预 测 匹 配 乘 客 和 司 机 (ETA) 定 价 服 务 乘 车 服 务 ](../../media/Location-Service-Uber-Uber---ACE-image5.png)





System has 4 part

1.  Matching service(supply and demand service) --- a. update the driver's and passenger's location, b. matching the passengers and customer
2.  ETA service and map service
3.  Pricing service
4.  Trip service -- remember the trip information
5.  Send a trip event to a analysis database like surge service or trip log service



![0 @以Q队融斌，尢` 乜 c " Firewall @ 爱 思 版 权 所 有 禾 经 允 许 请 勿 录 制 或 传 播 acecodei ntervie ](../../media/Location-Service-Uber-Uber---ACE-image6.png)







Detail:

1.  How the driver update his location



[We can use google s2 cell, google s2 map the whole earth into cells. And every cell has a number , it index by "Hilbert curve", if two cell are next to each other, their index also should be close]{.mark}







![思 考 题 当 司 机 穿 过 Geohash 界 限 时 如 何 删 除 前 一 个 Geohash 中 的 司 机 坐 标 ？ 不 删 除 使 用 另 一 个 Source of Truth 来 表 达 司 机 的 真 实 位 置 ](../../media/Location-Service-Uber-Uber---ACE-image7.png)









![合 理 的 位 置 信 息 形 式 DriverbyGeohash (Redis Set) DriverLocation Geohash Driver ID DriverID ( Lati , Longitude) ](../../media/Location-Service-Uber-Uber---ACE-image8.png)









When driver update his location 1 update driver location 2.. 3..



![司 机 位 置 更 新 操 作 更 新 Driver Location 更 新 Driver by Geohash 删 除 超 过 30 秒 以 前 的 Driver by Geohash Entry ](../../media/Location-Service-Uber-Uber---ACE-image9.png)

2.  How to find the driver



1.  Get passenger's Geohash
2.  Then get all the drivers in the same cell id or same geohash

(Shard by geohash -- consistent hash and store in memory)

3.  Then check if those drivers(list of driver ID ) are still in there, calculated the ETA



![第 三 步 找 到 合 适 的 司 机 0 计 算 ETA 批 处 理 一 系 列 匹 配 要 求 ](../../media/Location-Service-Uber-Uber---ACE-image10.png)

Calculated the ETA with multiple customer and multiple driver together





![e--- 0750 ](../../media/Location-Service-Uber-Uber---ACE-image11.png)

[Each node is communication with gossip, when driver first will send his information to any node and node will forward the information to destination node]{.mark}



Each node will have a hash range, the node will just for this range of driver information



![一 致 性 ？ Best-effort 没 有 备 份 宕 机 即 丢 失 数 据 不 保 证 一 致 性 ](../../media/Location-Service-Uber-Uber---ACE-image12.png)



Consistency --- best effort, not backup --





![Trip Table BASE trip_uuidl STATUS NOTES FARE ADJS. ](../../media/Location-Service-Uber-Uber---ACE-image13.png)



Trip id, base -- driver id and time stamp,

Status -- payment status

Node --





API -- websocket





![SEND destination : /vl/locations "user uuid": 123e4567-e89b-12d3-a456-426614174eee, "lat": 23 2 "long": -123.2, "driver mode": false ](../../media/Location-Service-Uber-Uber---ACE-image14.png)





![Home Page Response . 4, "long 23.3 "drivers " : {"lat": {"lat": 23 , "long -122.2}, 't: -122.1}, ](../../media/Location-Service-Uber-Uber---ACE-image15.png)



![SEND destination : /vl/match "user uuid": 123e4567-e89b-12d3-a456-426614174ØOØ, "pickup_lat": 23 2 "pickup_long . -123.2, "dropoff_lat": 23 1 "dropoff_long ": -123.1, ](../../media/Location-Service-Uber-Uber---ACE-image16.png)







![服 务 器 存 储 1 · 8 扩 展 性 Websocket ， Stateless Service 分 布 式 缓 存 ， 分 布 式 数 据 库 ](../../media/Location-Service-Uber-Uber---ACE-image17.png)









![服 务 器 存 储 1 · 9 容 灾 设 计 使 用 客 户 端 恢 复 服 务 器 状 态 Replication ()B ， Cache) ](../../media/Location-Service-Uber-Uber---ACE-image18.png)



![Availability 1 · 10 监 控 警 报 Request Latency Cache Capacity ](../../media/Location-Service-Uber-Uber---ACE-image19.png)





Option: surge 1.2 price



![](../../media/Location-Service-Uber-Uber---ACE-image20.png)



![价 格 算 法 上 车 区 域 的 司 机 数 上 车 区 域 的 乘 客 数 上 下 车 位 置 ](../../media/Location-Service-Uber-Uber---ACE-image21.png)







Some tradeoff



![最 简 单 的 位 置 信 息 形 式 Region Driver ID Latitude Longitude ](../../media/Location-Service-Uber-Uber---ACE-image22.png)



![思 考 题 这 个 简 单 的 位 置 信 息 形 式 有 什 么 问 题 ？ 已 经 下 线 的 司 机 无 法 删 除 区 域 无 法 Shard ， 需 要 进 一 步 细 分 司 机 穿 过 两 个 区 域 时 无 法 删 除 上 一 个 区 域 里 的 信 息 ](../../media/Location-Service-Uber-Uber---ACE-image23.png)

Region 太大....



GeoHash ->{{ID,ts }, {ID,ts}}



Ts the last time driver update his location and sorted by timestamp



Delete all the Geohash - {driver id +ts } current -ts >=30 ms



























