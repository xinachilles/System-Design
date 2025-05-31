# Scenario



---

What want to be search?



Assume we want to search status,Each status consists of plain text



- Let's assume Twitter has 800 million daily active users.
- On the average Twitter gets 400 million status updates every day.
- Average size of a status is 300 bytes.
- Let's assume there will be 500M searches every day.
- The search query will consist of multiple words combined with AND/OR.





Since we have 400 million new statuses every day and each status on average is 300 bytes, therefore total storage we need, will be:



400M * 300 => 120GB/day





We need to store 120GB of new data every day. If we plan for next five years, we will need following storage:



120GB * 365days * 5 => 200 TB





want to keep an extra copy of all the statuses for fault tolerance, then our total storage requirement will be 480 TB.



If we assume a modern server can store up to 4TB of data, then we would need 120 such servers to hold all of the required data for next five years.



Let's start with a simplistic design where we store our statuses in a MySQL database. We can assume to store our statuses in a table having two columns, StatusID and StatusText. Let's assume we partition our data based on StatusID.



How can we create system wide unique StatusIDs? If we are getting 400M new statuses each day, then how many status objects we can expect in five years?



400M * 365 days * 5 years => 730 billion



This means we would need a five bytes number to identify StatusIDs uniquely. Let's assume we have a service that can generate a unique StatusID whenever we need to store an object (we will discuss this in detail later). We can feed the StatusID to our hash function to find the storage server and store our status object there.






