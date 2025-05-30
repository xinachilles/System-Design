# Requirement 



---

Author: Scott Shi

Date: June 08, 2020





[Problem Description]{.mark}

Design a 12306 ticket booking system to support the high loads. The peak qps is expecting around 1m. Potentially could grow to 10m or even higher. There are T number of train routes and each route could be further fragmented to numerous stations. A customer booking tickets from city A to city B could require booking several legs from different routes. (eg. Beijing to Hongkong could be separated to: route1 Beijing to Hangzhou, route 2, Hangzhou to Guangzhou, route3 Guangzhou to Hongkong). Customers could revoke the booking at any given time.





Additional details: Total seats per train is around 1000.

2018 total traffic: 3.37 billion:

Growth rate: 9.4%

highest record is 13m per day Spring of 2019



Solution:





Quick estimation:

System should be able to handle 5 billion passengers per year. That is ~14 million per day.

14 million per day, assuming 85% occupation rate. Number of trains per day 16.5k

Peak time throughput doubled to 33k trains with 100% occupation rate. Passengers double times to the 28 million per day.



10m qps / 10k trains = 1k peak qps per train.





- The bottleneck is with database, in particular, when many people rush to the same trains, there is a possible case of [read-modify-write conflicts]{.mark}, ie. A and B checked the empty seat of train 1 and found there was an empty seat available, when they booked it, they had a conflict and only one was able to get the seat.( two phrase locking)
- [Relational database is used to guarantee a strong transaction.]{.mark}
- Databases have to be sharded in order to handle the traffic. The normal shard key is train-travel-time (Beijing To Shanghai G1234 departs at 12:30pm)



- The separation of train seats are pre-determined and pre-allocated. Pre-allocation is required because there are still some offline agencies to sell the tickets in person, they will need to reserve some tickets. Additionally the traffic pattern is relatively predictable, few rounds of optimization of distributions of tickets should optimize the throughput to near max.






