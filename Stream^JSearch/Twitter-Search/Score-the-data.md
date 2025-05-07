# Score the data 

Created: 2017-10-14 16:45:38 -0600

Modified: 2017-10-14 16:55:32 -0600

---

Additional information about the entity (beyond what is in the inverted index) is typically

required for scoring. We added the ability to store a blob of metadata for each entity in the index. The indexing pipeline facilitates the creation of forward index data along with the inverted index.













10. Ranking



How about if we want to rank the search results by social graph distance, popularity, relevance, etc?



Let's assume we want to rank statuses on popularity, like, how many likes or comments a status is getting, etc. In such a case our ranking algorithm can calculate a 'popularity number' (based on the number of likes etc.), and store it with the index.







Each partition can sort the results based on this popularity number before returning



results to the aggregator server. The aggregator server combines all these results, sort them based on the popularity number and sends the top results to the user.






