# How to collect the data for index service or type head 

Created: 2017-10-20 09:34:56 -0600

Modified: 2018-01-30 01:30:39 -0600

---

Collecting the Data



we can use pub sub way to collect the data



publisher has a tailor and the tailor will reads updates from the database transition log and copy and push them to interested applications or subscribers that include News Feed, index servers, Graph Search and many others.



The tailor read the transaction log once is good for IO. It is IO efficiency



since the database is master slaver mode and salver in the different location. the publisher is running on a slaver and provide the data to subscriber locally



There are some application marker in the traction log that indicates the position of the last received and get acknowledged back from the application



Publishers periodically ask the application: hi application, are you done with the message number 10. Just make sure the application received the data. if publisher receiver this message and it will move the application maker to number 10



In case of error happened , the publisher needs to resent the data sending updates base on those marker.



For example, we have two maker blue and red and one recovery tailor, recovery tailor to start reading the minimum of blue and red, but blue application need wait to the green application catch up



or we can have two recovery tailor which read the data in different place ,put more IO load but low latency. It is a tradeoff between IO and latency,





When the recovery tailor and main tailor read the data at the same point. Publisher realize those factor that We back the good the state again



How to handle the failure



On the publisher side, Wormhole provides multiple-copy reliable delivery where it allows applications to configure a primary source and many secondary sources



If the primary publisher usually in the same location (which generally is the publisher in the local region) fails, Wormhole can seamlessly start sending updates from one of the secondary publishers.



if the subscriber fail, we have different marker in the transaction log and we can use one or more than one recovery tail
