# Sub Pub

Created: 2020-11-24 17:27:24 -0600

Modified: 2020-11-25 00:32:05 -0600

---

like imagine a client basically triggers an asynchronous operation ,some operation that doesn't block them from doing other stuff and that operation goes to the servers and then has to take some time to happen, and then eventually once it completes it has to go back to the client or the response of that operation the reserve that operation eventually once it completes ,has to go back to the clients, that might not be something that you would want to store in your typical database solution



Publishers, publish data to these topics ,to these arrows here represent data being published or pushed from these servers to these topics ,you can think of these topics kind of like channels, the your subscribers in our example these subscribers are going to be the clients, the clients that were originally listening for data from the servers ,that said here, these clients are these subscribers are going to subscribe to topics ,so they're going to be listening for data or for information







pups of pattern this is different, the subscribers do not communicate with the servers, they subscribe to topics and the Publishers don't communicate with clients or with subscribers they just publish data to topics,



Publishers and the subscribers don't really know that each other. they just know that these topics and topics act as this intermediary between the Publishers and the subscriber. and then the fourth and final entity that we mentioned is what we called messages , they are represent some form of some operation that is going to be relevant to the subscribers and of the subscribers are basically going to be listening for, so the overarching flow is the publisher publishes data or rather publishes a message to a topic, the subscriber or multiple subscribers subscribe to that topic ,and as messages coming to the topic ,they are sent out to the subscribers that are subscribed to the topic



now to be clear here when we say message for this fourth entity ,we're not referring to a chat application message ,for example we're referring to the concept of a message, like a data block ,if you will this could contain a chat application message ,like a piece of text, but it could also represent an operation ,to execute a stock trade for example or anything







you're guaranteed that these messages are going to be delivered at least once to the subscribers who are listening





you can imagine it is that every message that sent or published to a topic is probably going to keep track of some sort of index or an ID representing, all of the subscribers are each of the subscribers rather that is listening to the topic, and when a message is pushed out to all the subscribers ,The subscribers are then like we going to acknowledge receipt of the message will send back and ACT to the topic, and then the messages are going to be simply flip some sort of flag, saying no I've been consumed by subscriber, one or has been consumed by subscriber









an idempotent idem potent operation or the concept of item potency an idempotent operation is an operation that has the same ultimate outcome regardless of how many times it's performed,



what's nice about a pub sub system at about the topics is that they also give you ordering of messages, ~~so as you send or publish messages to topics, they will be then pushed to your subscribers in the same order,~~ so this is kind of like a queeu, the first piece of data that goes into it is the first one that comes out of it



it is the fact that you can oftentimes replay messages in a pub sub topic if you want ,





you can then add stuff on to them for example, you can then add on content-based filtering at the subscriber level ,would that means is that you might have to subscribers here ,S1 and S2 who both subscribe to the same topic, like the stock prices but maybe S1 is going to specifically filter out any messages that don't contain tech company stock prices



Apache Kafka another one is Google Cloud pubsub Solutions,Google Cloud pubsub basically tells you that everything Auto scales is typically you might have topics are sorted into different partitions,



because this is like I said that of a solution ultimately Sochi one might actually be sorted into multiple shards



you don't worry about that your topics because going to scale automatically



and it give you the ability to encrypt your messages such that over the network here they're encrypted and then only your subscribers know how to read them and make sure that message is only reach the relevant individuals, people that you want to have receive these messages


