# Queue consumer pattern.



---

Image we have application mange the credit card process. the application cannot miss any message. We going to process the message into a queue

But what if the application that consumer those message corruptions or disconnected. What happen if we stop process the message. Worst case scenario queue could actually falled up.

How to make sure something like this could not happened.

Suppose that we actually two different way depending on your application need.

We support active and stand by solution and competing consumer type solution.

The active and stand by solution ensure all the message were processed in order and ensure that all processed. We can do is we use an active and standard by solution. One consuming application received all message and any other consumer connected to the queue. They are in the backup stage. Should be first instance active one failure. whatever reason we already got someone connected take over.

~~We kind of achieve it what we cached it exclusive queue. It's fault tolerance option what you have a active consumer and one optimal backup stand by consumers.~~



Other option is load balance completing consumer solution , there is we have a queue message in the queue. We allow have. This case we are going to use excusive queen. Multiple consumer bind. Message are receive and round robin ?? to the consumers



Let look some consideration those different consumer type



With active standard by queue. Some one need consider about include consumer [scaling]{.mark} message ??, duplicate message, active floor ??



If you continue use consumer completing solution. You need think about consumer scaling, [duplicated message]{.mark}, [message delivery, message ordering,]{.mark} max delivery and active message performance.



~~Competing consumer solution with exclusive queue, all consumer receive message from the queue and they round the robin share the f??~~
