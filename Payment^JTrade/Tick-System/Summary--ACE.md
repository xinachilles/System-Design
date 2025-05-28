# Summary -ACE

Created: 2021-03-24 23:23:21 -0600

Modified: 2021-05-28 10:14:21 -0600

---

**[1.Requirement]{.mark}**



Should able to buy the tick online



[it is for one location or different location?]{.mark}

it for different city and different cinema

when user select the city, user should be see the movies release this city



user can select the movie, user should cinema running this movie



[each cinema has different hall ? cinema also different hall, user should able to select the cinema hall]{.mark}



1.  after user select the cinema hall, user should select one seat or multiple available seat
2.  user can hold the sheet around 5 min until he finish the payment

if no seat are available, do we think we need a waiting list?



1.  if no seat are available, user should able to await until the seat available, the police should be first come first service.





![1 · 3 资 源 估 算 每 天 访 问 1M 次 ， 售 出 1 佣 k 张 票 假 设 有 抢 票 情 况 （ 1 闐 x 平 均 流 量 ） 瓶 颈 Read QPS 一 1M * 1 佣 / （ 36 佣 * 24 ） 、 ： 12 佣 服 务 资 源 Write QPS 一 1 闐 K 1 闐 / （ 3 24 ） ： 120 TicketDB · 1 闐 k 1 闐 B 5 365 = 18GB 存 储 资 源 TransactionDB- 1 闐 k 1 闐 B * 5 ％ 5 = 18GB ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image1.png){width="5.0in" height="2.5729166666666665in"}



each ticket entity is 100B

each transaction entity is 100B



![ヤ [ ceorch ー こ PB ゆ S い t 七 、 ⅵ → ⅳ 久 PB ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image2.png){width="5.0in" height="3.5208333333333335in"}





2.  **[Service]{.mark}**



We should have ticket search service, ticketing service, payment service and waitlist service





![Search Seat Payment Waitlist ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image3.png){width="3.7291666666666665in" height="4.5625in"}



3.  [Data follow]{.mark}: the most Scenario



A.  User searches for a movie base on the location. -- cinema table --geohash

Google s2 or quadtree they can sign a union id to each location

[the user alsp has a location id, it present a block or location, when they search the nearby cinema, the system will first compute his neighbor 8 cell id or more base on the radius, then return all the cinema base on the radius]{.mark}

B.  User selects a movie.

C.  User is shown the available shows or event of the movie.

User selects a show.

and user can see how many seat are availed from the ticket table



Once the user selects the seat, the system will try to reserve those selected seats and go to ticket service



user have 15 min to book the seat and finish the payment

After payment, booking is marked complete. If the user is not able to pay within 15 minutes, all their reserved seats are freed to become available to other users.



[if user found no seat are available but some seat just reserve but not completed the book, we can put them to a waiting list]{.mark}







![思 考 题 怎 样 保 证 同 一 场 次 的 同 一 个 座 位 不 会 重 复 销 售 ？ 亻 吏 用 锁 (Lock) 《 亻 专 扌 acecodeinterview.com ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image4.png){width="5.0in" height="5.385416666666667in"}



If we use master and slaver model, we can just add lock on the master service



Or the write only return to client if only the master and slaver all successfully writing



Or we can add lock for both wrtiting or reading





[Database Desgin]{.mark}

![movies id name description duration id super_venues_id type description varchar varchar datetime varchar events id movie_id venue_id name start_time description super_venues geohash address description transactions id user_id ticket_id status int varchar datetime varchar event_status varchar varchar id event_id col price modify_at transaction_status transaction_type ticket_status datetime associated_transaction_id payment_id name email int varchar varchar ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image5.png){width="5.0in" height="2.8541666666666665in"}



Venus = hall

Super_venus = cinema

Events = show





Ticket need a lock

Ticket : status - > pending, available, unavailable

Ticket id +.... Pending is select but not paid

![mat Event ID Shard Key Tickets Table Row Column Price Status Available/Not Available/Pending ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image6.png){width="5.0in" height="3.0625in"}



Modified time -- lazy update, when other people visited this record, it will check if the status is pending and modified time is more than 15 minute, we can update it back to available





![ACIE Transaction Table Payment/Refund Success/FaiIed/Pending ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image7.png){width="5.0in" height="2.90625in"}

We will add record in transaction table when the payment is success or failed

If type is refund -- associated transaction id is payment id





![思 考 题 购 票 时 ， 如 果 服 务 器 在 收 款 完 成 后 更 新 票 状 态 前 宕 机 ， 怎 么 处 理 ？ Distributed Transaction 有 一 、 Transaction Manager 来 保 证 收 款 和 更 新 票 状 态 能 够 同 时 发 生 ， 如 果 只 有 其 一 发 生 ， 需 要 重 试 失 败 操 作 或 回 滚 到 初 始 状 态 勿 录 制 或 传 播 acecodei nterview.com ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image8.png){width="5.0in" height="4.71875in"}



![2-phase Commit Coordinator B in Participant Prepare (vote request) Vote Decision Ack ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image9.png){width="5.0in" height="4.197916666666667in"}



![2-phase Commit 的 缺 点 Blocking 一 旦 有 任 何 节 点 长 期 宕 机 ， Transaction 就 会 停 滞 。 勿 录 制 或 传 播 acecodei nterview com ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image10.png){width="5.0in" height="4.583333333333333in"}





two participant here is 1 .payment 2 ticket



2 phase commit

1.  To all participant, are you ready to write the change to database
2.  All the participant will prepare a [endo log and undo logo]{.mark}( in Kafka)

3. then send "YES" message to coordinator

4. if coordinator get YES from all participant, it will send the "commit" message to participant

5. participant commit the change and send "YES" to coordinate





We can use census protocol, there are multiple coordinator, they will do the next action only more than half coordinator agree



![排 队 等 票 2 ． 3 ． 4 ， 提 供 付 费 方 式 ， 加 入 Waitlist 如 果 前 一 个 用 户 未 能 付 款 ， 系 统 自 动 实 施 买 票 如 果 购 买 失 败 ， 买 票 机 会 留 给 下 一 位 如 果 购 买 成 功 ， 通 过 Websocket 以 及 email 通 知 用 户 ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image11.png){width="5.0in" height="3.875in"}

[Waitlist DB]{.mark}

[Event id, user id, timestamp,]{.mark}











![思 考 题 四 等 票 情 况 下 ， 如 何 检 查 付 款 超 时 并 通 知 下 一 位 ？ 一 旦 确 认 付 款 失 败 ， 将 这 笔 交 易 状 态 设 置 为 失 败 ， 同 时 读 取 Waiting List 数 据 库 中 仍 在 等 待 这 张 票 的 最 早 的 用 户 ， 自 动 为 其 购 票 直 到 成 功 为 止 。 acecodei nterview.com ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image12.png){width="5.0in" height="5.583333333333333in"}











for the reservation, we updated the ticket table and the status is pending, the timestamp is current time



If the reservation gets expired, system will scan the waitinglist table and find out if any waiting customer can be served.



Also, since we are serving in a first-come-first-serve manner



Clients can usewebsocketfor keeping themselves updated for their reservation status. Whenever seats become available, the server can use this request to notify the user.



[Rate limit]{.mark}

stop the request as much as possible. ~~Not let all request hit the database, distribute the request to different layer. If we have any 200 ticket, we don`t need 200 million request to read or write the database.~~



in the [client level]{.mark}, we need limitation the user in the client level, allow user send search request xxx per second



in the [web server level]{.mark}, we need a rate limiter to limit the number of request for each customer or each IP address



we also cache some web pages in web service, if some peoplejust ask for some static information or just query the same information in certain time, we can just return those page directly



API



![// Request GET // Response "result" : "venue" : "venue "venue "movie" "movie id": 123 name": "arc sf" "Wonder Woman 1988" , name : "events": [ "event id": 123, "start time": 1609e37øøe, "event_id": 456, "start time": 1609047eøe, e acecodeinterview.com ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image13.png){width="5.0in" height="4.614583333333333in"}



![POST /vl/transaction "ticket id": 123, "amount": 3eoe, "currency": "usd" , "payment method details" : "card": { "brand" : "visa", "account number "4242424242424242" , " security_code": "123 "type": "card" ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image14.png){width="5.0in" height="4.625in"}



![1 · 10 监 控 警 报 Payment Success Rate 0 Data Consistency DBLoad 0 Cache Hit Rate ](../../media/Payment^JTrade-Tick-System-Summary--ACE-image15.png){width="5.0in" height="2.5625in"}





























