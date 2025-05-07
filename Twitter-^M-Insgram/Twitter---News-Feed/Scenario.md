# Scenario

Created: 2017-05-05 22:46:11 -0600

Modified: 2017-10-19 09:42:08 -0600

---

What functions should be supported:



1.  Users should be able to post new tweets.
2.  A user should be able to follow other users.
3.  Users should be able to mark tweets favorite.
4.  The service should be able to create and display user's timeline consisting of top tweets from all the people the user follows.
5.  Tweets can contain photos and videos. (option)





Let we assume they have 100M DAU and each user around 60 times read request

such as visited follower`s time line, tweet review



100 M * 60 / 86400 = 60 K



peak *4 = 240 k



and 0.1 write request.



6k



Storage: Let's say each tweet has 140 characters and we need two bytes to store a character. Let's assume we need 30 bytes to store metadata with each tweet (like ID, timestamp, user ID, etc.). Total storage we would need:

10M * (280 + 30) bytes => 3GB/day



[CAP](onenote:Basic.one#CAP&section-id={86482390-C87C-1E49-9164-B76565805B41}&page-id={3CD0AF12-C97D-9849-8533-4DF4613701B1}&end&base-path=https://d.docs.live.net/77339d157d673f41/Documents/9%20chapter/System%20Design%20and%20OO%20Design)



I think the system should need to be highly available and the consistency is also very import but if user cannot see the tweet for a while, it should be fine.





API?






