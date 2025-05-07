# System design II --  Design distributed message queue

Created: 2022-11-13 17:35:43 -0600

Modified: 2022-11-13 18:12:07 -0600

---

[Candidate: What's the format and average size of messages? Is it text only? Is multimedia allowed?]{.mark}



Interviewer: Text messages only. Messages are generally measured in the range of kilobytes (KBS).



[Candidate: Can messages be repeatedly consumed?]{.mark}



Interviewer: Yes, messages can be repeatedly consumed by different consumers. Note that this is an added feature. A traditional distributed message queue does not retain a message once it has been successfully delivered to a consumer. Therefore, a message cannot be repeatedly consumed in a traditional message queue.



[Candidate: Are messages consumed in the same order they were produced?]{.mark}

Interviewer: Yes, messages should be consumed in the same order they were produced. Note that this is an added feature. A traditional distributed message queue does not usually guarantee delivery orders.



[Candidate: Does data need to be persisted and what is the data retention?]{.mark}

Interviewer: Yes, let's assume data retention is two weeks. This is an added feature. A traditional distributed message queue does not retain messages.



[Candidate: How many producers and consumers are we going to support?]{.mark}



Interviewer: The more the better.



[Candidate: What's the data delivery semantic we need to support? For example, atmost-once, at-least-once, and exactly once.]{.mark}

Interviewer: We definitely want to support at-least-once. Ideally, we should support all of them and make them configurable.



[Candidate: What's the target throughput and end-to-end latency?]{.mark}

Interviewer: It should support high throughput for use cases like log aggregation. It should also support low latency delivery for more traditional message queue use cases.


