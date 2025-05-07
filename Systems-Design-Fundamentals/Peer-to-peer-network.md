# Peer to peer network

Created: 2020-11-23 11:45:44 -0600

Modified: 2020-11-23 15:48:39 -0600

---

for some big tech company where you're going to want to be able to deploy or transfer large files to thousands of service at once.





we want to design our system such that we can deploy from this one machine here we can deploy or transfer large files let's say that our files are going to be 5 gigabyte files we want to be able to transfer 5 gigabytes files to a thousand machines and we want to do it repeatly







we can assume that the system is going to live in a powerful data center owned by the big tech company and so we're going to assume that our system has a total network throughput of 40 gigabit per second





40 gigabits-per-second comes down to 5 gigabytes per second





so with just some simple math we can see that at any point when we're going to want to actually transfer a 5 gigabyte file ,if we have a network throughput for our system of 5 gigabytes per second and we want to send the 5 GB file to a thousand machines than this operation of transferring a file to a thousand machine is going to take a thousand seconds





it'll take one second to transfer the five-day GB file to the first machine ,another second to transfer the 5 GB file to the second machine ,and so on and so forth a thousand times a thousand seconds and 1000 seconds comes down to roughly 17 minutes, 17 minutes is actually quite long for this type of operation ,especially if you assume that we might do this repeatedly throughout the day



maybe we're going to do transfer 5gb files every 15 minutes ,and that's really bad, if we have this operation that takes 17 minutes ,so here we can clearly see that we have a bottleneck in our system you can imagine that when all of our machines here are getting the file from our main machine



you can see that basically is a bottleneck at our main machine deploying or transferring of the file



~~instead of having only one machine service is 5gb we're going to have 10 machine serving these 5G be here~~



~~by the store and here we have our thousand machine that are not going to be requesting the 5 GB files in a distributed manner from these 10 machines~~



~~and so assuming that are a thousand machines do so in a very balanced way, between the 10 machines here and this would basically speed up our system 10 times over~~



~~secure visually speaking you can imagine that some of our machines are going to be requesting of the files from this one some of them are going to go hear some of them are going to go here etc etc~~



~~but we also run into a problem with this approach because first of all the speed of our operation of transferring a 5 GB file still isn't great 17 minutes divided by 10 is still roughly a minute-and-a-half which isn't amazing it's not bad comparative we speaking but it's not amazing depending on our use case it might actually be pretty bad but also that means that these 5db files now need to be replicated on all of these 10 machines,~~





~~okay what about sorting maybe we could shard, 5gb files in other words we could have some of the files live in one machine like the one here the far-right other files live in another machine like the one here and so on and so forth~~





~~so friends can't imagine that we have a total of 25 gigabyte files maybe we can spread them out such that each machine has two of the 5 GB files that way we don't need to replicate the files all over the place but then we run back into the first issue of whenever we try to transfer one of the files then all the Thousand machines are going to be going to the same machine that has that particular file and will have once again a bottleneck and it won't be great we'll be back with a 17-minute operation~~







What If instead of sending this 5 GB file to each and every one of the Thousand machines What If instead we were to split up is 5 GB file ,you were to split it up into very small chunks, very small pieces of the 5GB file, then we would have send these trunks all peers and then we would have let those peers communicate with one another to grab the missing chunk as they all need to create the final file, In the basically build up their own total file



that is the general gist of a peer-to-peer Network more concretely in this example let's imagine that we were to divide our 5 GB file into 1000 ,5 megabytes files to divide 5 GB into 1000, 5 Mega file





we could send out one 5 megabyte file to each machine this would take 1 second







then you can imagine that for a single machine to get the entire 5 GB file if a single machine has 5 megabyte file, it now me the 999 other 5 megabytes files, and so a single machine needs to talk to the 999 other machines to get all the missing pieces but it doesn't have



how long would this take for a single machine to do it would take 0.999 seconds 1 second divided by 1000 * 999 ,so for a single machine it would take one second to get the rest of the 5 GB file from all the other machines



when does top left machine starts to talk to the other machines, to get the missing pieces that it needs, let's say it starts talking to this machine, while this machine is talking to this machine all the other



two other machines can simultaneously talk to each other, and have one of them transfer 5 megabytes of the file



we have a bunch of arrows are bunch of communication happening all at once ,that's how peer-to-peer network works ,at a high level
