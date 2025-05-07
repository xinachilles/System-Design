# Shading Trade off

Created: 2017-10-19 14:22:22 -0600

Modified: 2017-10-20 23:47:51 -0600

---

most can shading by **user id**



pro:



it easy query the photo or tweet posted by the user





cons or issue :



lead to unbalanced servers and hotspots



if the user become hot and there could be a lot of query on the server which holding on that user and this will affect the performance of that service



overtime, some user will post a lot of tweet or photo compared to others. so maintain a uniform distribution will very difficult







**shading by photo id or tweet id**



pros is easy to maintain uniform distribution



issue is : if we want to find a tweet or photo for particle user, we need query all service



a aggregate service will compare those result and return to user





Shading by the creation time



pros : easy to find the current or latest tweet or photo easily. we just need query small set of service .



the problem traffic load will be huge compare with the other service hold the old item





if we shading base on the creation time and id, we will get the benefit of both of them
