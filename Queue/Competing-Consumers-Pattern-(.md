# Competing Consumers Pattern (no need)

Created: 2019-10-21 22:27:28 -0600

Modified: 2020-09-09 11:36:00 -0600

---

[Queue: Competing Consumers Pattern](https://www.youtube.com/watch?v=29boOn4hXUU)



![Video web content titled: Queue: Competing Consumers Pattern](../media/Queue-Competing-Consumers-Pattern-(no-need)-image1.jpg){width="5.0in" height="2.8020833333333335in"}



we do have any number of producers . can be 1 to 1000 producers. message is going on to the queue. you typical web service store the information into the queue.



on tail on to that. you consumers basic what they will be doing in is pulling off the queue.

let every second each consumer will pull one message.

the idea is there is competing is added you as a consumers

these message will go to every consumers and only once



consumer 1 will get message A and it will be only one will get message A.

second consumer get B,C and so on.

those message will only go to the signal consumer therefore each consumer is competing messages.



the reason it is good thing. it is really easy to scale. you really can have many consumers.

if you have 8 consumers, you can really dequeening 8 message per 1 second which is just that artificial time frame that i made up



each time you added a consumer you will be enabling more throughput off the queue. this is really good for cloud.

~~you can have consumer per service~~

~~this is really what we are talking about using horizontal scaling~~

~~every time you added a service, you are enabling a consumer, therefore getting more throughput.~~



