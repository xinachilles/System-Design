# the general algorithms used called distributed hash



---

the general algorithms used called distributed hash



tables they get make use of hash algorithms the characteristics will see is that have become it's a structured peer-to-peer system what that means the node will remember some information about the index where to find the resources stored on other nodes



that implement and use dhts we're going to go through cord which is one of DHT algorithm







so we'll go through how cord works very basics of remember what we're trying to do is find which node is response for which has a particular resource



It is a the searching process that is there are a set of nodes set of node in the network





we make use of hashing hash algorithm we've mentioned before a hash algorithm takes as an input some piece of data variable sized data produces as output a practically unique small hash value



this cord will use the sha-1, it will take the hash of [an address of a node of a peer so the IP address and in practice not just the IP address but also the port number and produces some ID some 160 bit value ( sha1 will produce 160 bit value)]{.mark}



and the hash of the file here it says file name but in more general the file that file contents itself so I have a one megabyte file I want to share on the peer-to-peer system, I take the hash of that file and it produces some key





So nodes have some ID and file resources have some key u both of the same length 160 bit



[Node 1 node 2 node 3 node 4 and come back around to node 1 again so we'll visualize as some ring or some circle ordered by ID]{.mark}



[treat the hash space as a ring]{.mark}



[visually we can think of them in placed in a circle]{.mark}



[so a resource which has key k it's stored at the node with the same ID]{.mark}





so I have some file I take the hash of that file the contents of the file and the hash produces some small value





In the example that we'll go through we'll use a smaller hash value in real code we that means we need to store that file on node with ID 3

[Or we can say the node with ID 3 is managing the file with ID 3]{.mark}





with a 160-bit hash algorithm we have 2 to the power of 160 possible files and 2 to power of 160 nodes



I can have 16 possible IDs so I can only have 16 possible unique nodes in my peer-to-peer system if I only I may not have 16 I may have just 10 nodes at some point in time 10 nodes in the system



if a node with some ID which is equal to K in our example K equals three ID equals three [if there's a node if there's no node that has that idea in our network then the resources stored at the next node with it in the ring then over the next highest ID]{.mark}





For example resources with key of 8 9 10 where we don't have nodes so the rule is that the next node along with the ring will manages that resource





[What happens if new node joins a network]{.mark}





so as a new no joins the network we need to update the keys managed by the successor node



that is for 13 the successor node is 14 the next night around, so every node that joins a network we need to potentially one other node needs to have its keys updated n



[So 13 will take some portion of the ring, but rest of them will remain same]{.mark}





When a new node join, there needs a small update but very very

minor relative to the number of nodes if



[similar when we leave a network]{.mark} , [a node leaving the network we need to update the keys for the successor]{.mark}



[node if we have a million nodes in the network a larger network one node leaves we only need to do some updates at just one other node which is simple in managing the network]{.mark}



when we say update in real life it means sending some packets to to inform the other one that it's got more keys to manage







[how to research]{.mark} here's a simple approach to search here's an example where node 0 is searching for a resources with key 13



if pier 13 exists then it should have the resource with key 13 that's the the basic concept



a search is that we still need to know the IP address of some nodes because we need to send the packets



so with search we maintain a routing table and the size of









[how cord actually works is the node table maintain more than one entry and the scheme is to maintain entries for example two nodes which are 1 ,2 ,4 ,8 positions away so we double the distance from our node, if we have 2^160 node, one node will have 160 entries from 2^0 to 2^159 which across half of network]{.mark}





~~Each node need maintains a route to the nodes which are one two four and eight positions away, nodes don't exist then we maintain routes to the next one that does exist~~







we're searching, each node support manage some [key space]{.mark} some

interval of keys, the first node is assumed to maintain the key 1 the second node which will be two positions away should manage keys 2 & 3





If we assuming nodes 1 2 4 & 8 exist then if we want to search for key 1 the nearest node should be node 1, because node 1 should have key 1



if we want to search for key 3 using our scheming cord the nearest node that knows about node 3 should be node 2



so we say that the key space managed by this second node is 2 & 3



these the columns this is the next node or the node and this all right this is the IP address and this is the key space we'll use the key space when we search



so let's see how that works assuming all nodes are there then node

0 is looking for key thirteen if node zero is looking for key 13, it

should be on node 13 node zero has four routes to nodes 1 2 4 and 8



if we send to 8 then 8 we'd send to node 12 because that's the closest to 13 now from the routing table of the node 12, and 12 send a node 13





The search algorithms one way to search your searching out of a space of 16 cut it in half





we have a trade-off try to keep the number of entries low but make search fast







we're trying to trade off two different things one we want to make the search query' fast send as few messages as possible



two we want to make the routing table small so that when nodes join and leave the network updating the routing table takes little effort doesn't require much overhead the larger the routing table the larger the update cost the larger the routing table the shorter the search







if we have a 1024 nodes, which is a 10 bit hash how big is our routing table well we have 10 entries in my routing table



I'd have routes to knows which from one position away up to 512 positions away halfway across the network



which is not so bad because as nodes leave and join yes we need to

potentially update that add some overhead but to search what's the worst case time





we want to have a small routing table because when a node leaves or joins we need to update that routing table we need to refresh it that incurs some overhead the smaller it is the less overhead of updates but and we've designed the routing table in a way such that the number of queries we send is small and the design is keep the routing table small and means we can do a fast query







of this one two four eight



positions away



any further questions on this yeah



how do we know what you made with this



this is in fact we don't need to store



this this is just showing how the



algorithm works in that okay these are



my next nodes which one do I use when



I'm searching for a particular key well



I use the one which either has that or



should have that key should have key -



should have key three five and nine or



the one that's closest to the node that



will have that key but before it so the



one that is closest to node if we're



looking for key for the closest because



we don't maintain a route to node for



only two three and five the closest to



four that is before four is three so



that's how we get this value there



closest to seven are the closest to



eight but it is before it is five or in



practice seven node 7 so that's that's



how we cut this search space in half



each time send to the node which is



closest but before and then that one



should have routes which are almost



direct to the destination



how do you calculate this key space if I



ask you in a question well you just look



at these vendors even if I don't ask you



to calculate these values the ideal



nodes you can 1 2 4 8 positions away



which nodes exist and then looking at



this first column okay the key space



comes directly from 2 up until 3 but not



including 3 3 up until but not including



5 5 up until but not including 9 and



back to the start key 0 a key one we



don't need in there the key space there



it's just a way to think about how the



search would work



so we saw we just see saw how cord does



the search this was the example that we



went through at the start which is not



how cord works it was if we used a



routing table of just one entry which is



good because we just need one entry per



node have a million nodes just need one



entry but search can be very slow the



worst case search is if we just have one



entry a million knows the worst case



number of queries it was 1 million send



to my neighbor lays in to their neighbor



and it goes all the way around across to



all 1 million others so that's bad so



that's why the cord approach is a good



trade-off



let's come back just go back to the



descriptive slides and see if there's



something we've missed



so in cord every node has an ID every resource has a key and they use the same space that is we use a hash value of the address to give an ID a hash value of a



file not a file name a file to give a



key and they using the same space of



values the same set of values in court



it's 160 bit value we think of the nodes



at some circle ordered by their ID not



all nodes exist so the idea is that a



resource with key K is thought it node



with the same ID but if that node



doesn't exist the resource is stored at



the next node around in the ring to join



a network you there needs to be some



reassignment of keys that are managed



with the successor node and same when



you leave it a network if you have a



million nodes in the network one node



leaves then we need to update just one



other node the other 999,000 don't have



to worry if one node leaves so that's



easy to update I'm a node in the network



I have a file to share I don't to insert



that data into the system I calculate



the hash of the file find the key value



and then send the file to the node that



manages that key value if my key is 20 I



will send it to no 20 or the next node



the node that needs to manage key 20 of



20 doesn't exist so even though I have



the file it's stored on some other node



I transfer to some other knowledge you



can adapt that so that you don't



transfer the node but you transfer some



identity some address but the approach



is that the resources are distributed



amongst the nodes in the system



so that's inserting data to search as



we've seen we need to know the key for



example connected with BitTorrent the



dot torrent file contains the hash value



of the fire of the key once we know that



we use our approach of looking up the



routing table not using simple routing



but using the cord routing where a node



maintains routes to nodes which are 2 to



the I minus 1 positions away this 1 2 4



8 positions way depending upon the size



of the network this is just some way to



write down the routing table what I've



drawn up here and you can think of it



let's look at our table in order to find



a node that stores the resource with key



four five six or seven send to node



seven that's how we interpret our



routing team if you look in past exams



you'll see sometimes I give you an empty



table with the columns and ask you to



fill it in and then ask you to do a



search query and see who it sent by and



that's just an example of the steps of



the search query the key benefits a node



stores information about a small number



of other nodes



remember the benefit of natela the fully



distributed system was that you only



store information about your own node



it's a a unstructured system you don't



store information about other nodes



the disadvantage of Napster is that the



server stores information about all



notes the entire index here we store



information about a small number of



nodes meaning updates as things change



is easier we store routes to M nodes if



there are two to the power of M nodes in



the network reduces the maintenance



between nodes and the other benefit is



that we can locate query local locate



resources via queries quickly we've gone



through this example



the last slide summarizes those four



approaches plus another one Nutella



Napster so the two extremes of fully



distributed versus using a server the



one in the middle



super peers or fast track and then cord



and this other one is full replication



means if you want to share files just



copy the file to every other node that's



the replication approach and some



approximate comparison in terms of the



time to find a resource the latency



search query the number of messages



needed the cost of updating and the



amount of storage needed and you don't



have to be able to work out the values



but let's just explain some of them for



example for Nutella a directory server



the latency of a query is not dependent



upon the number of nodes because a query



always goes to the server so if you have



one node in your system you send a quick



one query to the server get a response



if you have a million nodes in the



system with Napster you send one query



get a response so the latency or the



time to get a response does not depend



upon the number of nodes it's fixed so



we say that that's one a one unit for



example with



natella the time to get a response



depends upon the number of modes the



more knows the longer it takes to get a



response and it's in the order of log n



so if we have a million nodes its if we



log say base ten it it's the order of



six if you go up to ten million nodes



it's slightly larger seven its we're



just using them say the Big O notation



or giving your proximal order let's look



at another one that may be a little bit



easier to understand the



the number of messages that are sent



when we send using the teller we send a



lot of messages because we broadcast



this is n up here the more nodes in the



network the more messages we send so



this is bad from the perspective of the



unstructured unstructured approach with



Napster we just sent one message no



matter how many nodes in the network we



just send one query to the server so



that's good as the number of nodes



increase the number of messages we send



increases for Nutella or an unstructured



approach with super peers it depends



upon the number of super peers remember



we send to a super peer and potentially



that needs to send to other super peers



so the if the suit number of super peers



is relatively small that's not so bad



but as that grows as see the number of



super peers grows then this number the



number of messages we send it's higher



yeah full replication is OK everyone has



some files before we start I transfer my



files to everyone I make a copy on your



computer and everyone's computer so



everyone has a copy of all my files and



now when I want to search I found the



file I have every file



therefore when I search I don't need to



send a query I have a copy the data is



replicated across all other nodes that's



no this is an extreme case with a large



network of course it will not be



feasible if we have five computers maybe



it's feasible just copy your files to



everyone else and then everyone has a



farm it's just showing search and the



number of messages we send is great but



to update something changes I need to



sender every other node if we have a



million nodes the update cost is too



large to update just a comparison so



update cost is if something changes what



it takes to update how much data we need



to store we see so the green ones are



good red ones are bad orange you're not



so bad but log end is is quite good



because logarithm as the number of nodes



grows the cost grows but only by a small



amount so log in is ok as well it's



acceptable for natella the number of



messages we send is the problem



broadcast is the problem for Napster all



good storage not so bad but we have a



centralized server and it's not captured



here if that fails the whole network



fails that's the problem with Napster



full replication fine for searching no



good for distributing the data super



pairs is better than nutella but still



if our network grows the number of super



peers grows and still our number of



messages can grow distributed hash



tables and the example is caught is not



bad for all of them with a large network



these are small values so that's why



court is considered a good alternative



to all of this and is used in practice



in a number of large peer-to-peer



systems



because even though it's larger than



here increase the number of nodes to 1



million 10 million still this is a small



value that is it increases the cost of



increases the time for queries it



increases the number of messages but



only by a very small amount so a log log



and is not bad so it's a good trade-off



in all factors that tries to to



summarize and compare you don't need to



remember these or even calculate them



but just understand the trade-offs



between those different those for



different approaches natella and fully



distributed Napster and the centralized



fast track which is a combination of the



two and chord and distributed hash



tables



and I think that summarizes what we've



talked about



we haven't talked about security and



we're not going to so we're finished



any questions on cord in the last few



minutes we have a chance diff different



peer-to-peer systems have used each of



them as we said Napster fast track and



Nutella have been used in practice



nowadays for example BitTorrent in many



cases makes use of distributed hash



tables



I think it's an option that is it can



use a centralized approach using the



trackers but it can also if it's if the



option is turned on to use distributed



hash tables so that's becoming more



common especially for large assistance



just briefly the exam from two years ago



though we don't have much time here's an



example question from two years ago is



is the core network answer some



questions about it



okay it's just grown from 16 up to 32 in



this case but just to show you the types



of questions so it gives you some set up



about which peers exist something was we



did it slightly different two years ago



but same approach there are some peers



in there or some caches of file names or



files which nodes exist so it gives you



a set of keys ask you where are they



stored you don't have this exam since



two years ago what if the maximum number



of peers allowed in the network and the



answer is 32 our chord ring has 32 nodes



we have a 5 bit hash value so the



maximum allowed is 32 in this simple



example because we have to have unique



IDs and it will only have 32 values we



can only have 32 different nodes work



out given the key work out which node



maintains the index we haven't courted



an index here but which node stores that



key so if the key was 7 and node 7



exists then the indexer would be node 7



if node 7 didn't exist then you need to



know which node stores key 7



what happens if it no joins the network



so this move moving of keys or



rearranging of keys and fill in the



tables which is this really I give you



the number of positions away too as a



reminder work out the neighbor the next



node and the key space let's call all



right it looks it takes a bit of time



and I may not give you all of them but



then you can use it for searching given



out those tables show me which nodes



it's the search query sent vine explain



why what's the advantage of using this 1



2 4 8 16 structure was there another



question look in last year's exam or one



of the other exams that I think there



was a question like ok let's what if we



don't use this 1 2 4 8 what if we use



something like 1 2 4 6 8 10 12 that is



every 2 positions away as the routing



table what's the advantage and



disadvantage



what's a disadvantage



we will have more routing table entries



though every second node I maintain a



route to then out of 32 I maintain 16



routes but with cord I'd maintain only



five routes but the query may be faster



so the more entries generally that the



more maintenance cost but the faster the



query any questions about cord there are



questions in the past exams too which



are very similar to what we've gone



through today worth practicing



it's briefly then go through last year's



exam



last



last lecture last ten minutes just have



a quick look at last year's exam a quick



last sign of the attendance sheet



I haven't I haven't written the exam yet



and I won't until sometime next week so



I cannot give many hints it should be



similar to last year it's just covering



the covering a topic since the midterm



here's a question which explains asked



you to explain the difference between



different technologies for delivering



IPTV in the service provider access Nets



of network so remember in IPTV we have



the network that connects from your home



to the core network that's usually the



bottleneck the slowest part so the



advantages of using adsl2 plus fibre to



the home and fibre to the node



what technology to use in your home



network wireless LAN or Ethernet draw a



diagram of that entire network



here you can quickly calculate this one



we have a video session requires five



megabits per second of core network



bandwidth the capacity of the network is



planned to be 10 gigabits per second so



the core network can carry a maximum of



10 gigabits per second or 10,000



megabits per second each video requires



5 megabits per second what is the



maximum number of users or channels of



standard-definition TV that can be



supported if multiple unicast is used as



a delivery mechanism multiple unicast is



just unicast we send multiple copies



what's the answer we have one video



takes up 5 5 megabits per second we have



a capacity of 10,000 if we use multiple



unicast to deliver from the TV station



to the users either so that you either



need to give me the maximum number of



users or the maximum number of channels



which one is it going to be in this



question users or channels why users



unique house so



what



a 1-1 one set of packets for each user



we cannot we're not using multicast so



we cannot for every user watching some



Channel we need to send one stream of



packets if there are two users watching



a channel the same channel at the same



time we still because we're using



multicast a unicast we still need to



send two separate video streams so it's



going to depend upon the number of users



if we have a capacity of 10,000 each



user requires five megabits per second



10,000 divided by five is 2,000 we can



support 2,000 users at once so that's



simply the capacity divided by per user



required



if each user is watching one channel



then we can support 2,000 users because



each user consumes 5 megabits per second



we have a capacity of 10,000 megabits



per second simple but then what if we



use multicast watching TV multicast what



can we support



now with multicast if we send one



channel to ten users we just use one



stream with multicast we just send a



copy once so in the best case if there



are 50 users or watching the same



channel I use up five megabits per



second because I just send one stream



using motor-cars



so now it's independent of the number of



users I can have a million users I have



a billion users in theory so what limits



us now is the number of channels if I've



got a capacity of 10,000 each channel



consumes five megabits per second each



TV channel then I can support 2,000 TV



channels 2000 channels and an unlimited



number of users per channel in theory



because of the multicast approach here



some other questions about



here's a slightly different one some



about AP TV this one's about voice the



g7 to vi codec produces a sample size of



20 bytes so remember back to voice we



sample the voice the sample we sample at



some rate produce some sample size and



send those samples as packets so this



split specific codec g7 26 you don't



need to know how it works but it tells



you every sample is 20 bytes and it has



a sample interval of 5 milliseconds how



many samples per second



how many louder how many samples per



second how many samples per second



sample interval is 5 milliseconds means



every 5 milliseconds I record a sample



so in 1 second I'd record how many



samples 205 milliseconds



with a sample every 5 milliseconds there



would be 200 per second every sample



takes up 20 bytes but note the way that



the protocol works RTP takes the bytes



of multiple samples and puts them into a



packet every 20 milliseconds so let's



say we're sampling over time these are



the samples 5 milliseconds every 20



milliseconds your computer sends a



packet so



how many bars does a packet contain



again



how many bites does our RTP packet



container so we sample every 5



milliseconds but we generate a packet to



send so our computer samples but then



just sends a packet every 20



milliseconds we don't send a packet per



sample in this case we send 4 samples



per packet per RTP packet every sample



contains 20 bytes of data so for samples



contains 80 bytes of data we're sending



80 bytes of data every 20 milliseconds



that means we're sending RTP packets



every 20 milliseconds or 50 packets per



second 50 packets per second a packet



every 20 milliseconds gives us that



means a packet 50 packets per second



every packet contains four samples each



sample of 20 bytes so a packet contains



80 bytes of data 50 packets per second



it's an RTP packet you need to add on



the header at the end of the exam shows



you the size of the headers



four samples per RGV packet means 80



bytes of payload of data plus the header



of 12 bytes of RTP and that's at the end



of the exam eight bytes of UDP and 20



bytes of IP plus five bytes of data link



layer I think that's in the question



which totals up to 125 bytes per packet



need to be sent by your computer across



the link and sent 20 every 20



milliseconds or 50 times per second



means 50 times one two five is



equivalent of 50 kilobits per second so



your computer needs sampling voice at



some rate producing samples putting them



into packets and sending those packets



and the amount that is sent across the



link works out to be 50 kilobits per



second and I think about above the



question says how many calls can you



support given some capacity what's our



capacity



you



our capacity is 10 10 gigabits per



second so 20,000 calls 20,000 times



50,000 is was that our capacity



is a million is one gigabit per second I



have to check the question but the



capacity divided by 50 kilobits per



second in that comes so the challenge



dare was working out the packet size and



over the next week you're going to go



through the remainder questions and the



questions from last year so when's our



exam the last day of exam week I think



that's it 12th ok the last exam the



period between then come and see me if



you have any questions about any of



these topics I will send this this time



an email with the hints about the number



of questions but it will be very similar



to last year's exam once I write no of



course not



last year how many questions says



question to see only a few questions but



some are maybe quite long yeah there you



go



shortest exam I've ever created six



questions not many



okay enough we'll see you during the



week answer any questions I haven't



looked at your assignments yet I will



look over them over the next week and



give you some feedback with some results



otherwise we're done well done we made



it through
