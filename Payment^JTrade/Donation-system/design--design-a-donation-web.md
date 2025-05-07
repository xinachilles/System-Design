# design: design a donation website for a 3 day charity event. Must support millions of users and 100 millions in donations.

Created: 2021-04-19 11:44:51 -0600

Modified: 2021-04-19 13:00:18 -0600

---

design: design a donation website for a 3 day charity event. Must support millions of users and 100 millions in donations.



讨论了大概的设计，payment的时候会发生的race condition。被问怎么保证exactly once, 我说用2pc 做transaction 好像并不太满意，但是我也答不出更好的了。其他讨论的有：

user interface: what does user see when go to donation website. what does user see when clicking on "donate"

basic workflow of a payment. You can assume we have third party payment gateway we can call into.



race conditions e.g. double charge, insufficient funds

edge cases e.g. external api is temporarily down, what can we do to continue to accept donations.

大概的时间是花了近20分钟讨论overall design, workflow, services, table schema。然后花了20分钟讨论race condition和edge cases，还有可能的解决办法。









Can I use the credit in my account



Non function requirment



1.  High consistency between our system, 3rd party payment system and charity



All the status should be same, if the status of 3rd payment service show the payment successful



Our system should get the payment status and return to user. Charity also should receive the money



2.  Support million of user and high QPS


