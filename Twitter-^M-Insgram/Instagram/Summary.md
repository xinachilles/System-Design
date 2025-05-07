# Summary

Created: 2017-10-20 00:25:06 -0600

Modified: 2017-10-20 00:25:47 -0600

---

**Functional Requirements**

Users should be able to upload/download/view photos.



Users can follow other users.



The system should be able to generate and display a user's timeline consisting of top photos from all the people the user follows.



Users can perform searches **based on** photo/video titles.(option)





CAP





Our service needs to be highly available.



The acceptable latency of the system is 200ms for timeline generation.



Consistency can take a hit (in the interest of availability), if a user doesn't see a photo for a while, it should be fine.



The system should be highly reliable, any photo/video uploaded should not be lost.





Data



we assume 1M daily active users. and every user upload 2 photo per ada



2M new photos every day and 20 new photos every second. ( qps is 20)



Average photo file size => 200KB

Total space required for 1 day of photos : 2M * 200KB => 400 GB

Total space required for 5 years: 400GB * 365 (days a year) * 5 (years) ~= 712 TB
