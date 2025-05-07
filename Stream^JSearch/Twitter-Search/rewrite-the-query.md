# rewrite the query

Created: 2017-10-14 16:47:41 -0600

Modified: 2021-01-23 01:29:48 -0600

---

we use two primary techniques: query rewriting and dynamic result scoring. Query rewriting happens before the execution of the query, and involves tacking on

optional clauses to search queries that bias the posts we retrieve towards results that we

think will be more valuable to the user.





Query rewriting



The queries that are issued by our end users are (obviously) not in the format required for retrieval. So we need to rewrite the query to translate the user intent and their social context into a structured Unicorn retrieval query. This is where we try to understand the query intent,

bias it socially, correct spelling, use language models for synonyms, segmentation, etc.




