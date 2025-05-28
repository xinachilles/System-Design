# Scenario

Created: 2017-10-11 22:42:22 -0600

Modified: 2017-10-20 22:33:09 -0600

---

**Functional Requirements:**As the user types in their query, our service should suggest top 10 terms starting with whatever user has typed.





![Design a Typeahead Google Suggestion 1 Scenario: prefix -> top n search keywords DAU: 500m Search: 6 * 6 * 500m = 18b QPS = 18b / 86400 200k Peak QPS = QPS * 2 400k ](../../media/Steam^JCollection-Typehead-Scenario-image1.png){width="5.0in" height="3.1875in"}





we can expect 18 Billion searches every day



we assume user search 6 times per day and each search user need type 6 characters to get the result they want











**Storage Estimation:**If on the average each query consists of 6 characters and assuming we need 2 bytes to store a character, we will need 12 bytes to store an average query. So total storage we will need:



18 Billion * 12bytes => 216GB



we can assume that only 1% of these will be unique



so the storage every day we need is 1.8GB



We can expect some growth in this data every day, but we should also be removing some terms that are not searched anymore. If we assume we have 2% new queries every day and if we are maintaining our index for last one year, total storage we should expect:



3GB + (0.02 * 3 GB * 365 days) => 25 GB







since we store the word in the tire data structure, the really storage should be less than this number





CAP:



Latency is impart for us, search typehead need compete with the typing spread

need to have low latency



Consistency



Not really important. If 2 people see different top 5 suggestions which are on the same scale of popularity, its not the end of the world. I, as a product owner, am happy as long as the results become eventually consistent.



How import the available



****Very important. If search typeahead is not available, the site would still keep working. However, it will lead to a much degraded experience.









