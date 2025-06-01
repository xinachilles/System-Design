# Facebook Haystack Service -2 



---

instead of a single photograph in a file, it's going to store multiple photographs in a file.



So what that does is that, now a single inode data structure, the metadata data structure is not a description for the single photograph, but it is a repository for perhaps 1,000 photographs in one single file.





So that's the way we can reduce the metadata bottleneck.



So metadata bottleneck can be mitigated by storing multiple photos in a single file.



This structure here consisting of the haystack directory, the haystack cache, and the haystack store, it replaces the photo server and the NAS that I showed you in the original NFS-based design of the photo repository of Facebook.



So the upshot of this particular design is that the metadata for [long-tail photographs are going to be in the haystack cache.]{.mark}



Because what's happening, as I told you earlier, if it is a photograph that has been recently accessed, most likely it is a CDN, but on the other hand, if it is not in the CDN, you have to go to the folder store, but remember now the photo store is implemented by this combination of haystack directory, haystack cache, and the haystack store.



This haystack cache is going to contain the metadata for all the long-tail photographs. Because of the fact that we are using a single inode as a representation for multiple photographs contained in a single file, we are reducing the metadata overload that we had in the NFS-based design,



the photo itself that you're looking for may be in the haystack store.



[So given this haystack based design, let's look at what happens when you try to upload a photograph.]{.mark}



First of all, I should mention that the haystack store is implemented as a log-structured file system.



what log-structured file system does is, instead of storing the file as is and making updates in place, changes to the file are stored as log data that is associated with that file.



So for instance, if I have a file f and I want to write to it and create a new file, f ', what I'm really doing is, I'm having a long segment, and in the log segment I'm going to say a particular block of that file.



So let's say from block number 1 of that file, this is the new data that is associated with that.



So we don't write the file itself, but we write the log. If I do another write to another block in the same file, I'll say f_2, here's a new data.



So in order to reconstruct the current copy of the file, I'll have to take this file and then add it with the changes that I just made.





So that's the idea behind log-structured file system. This was a technology that was invented primarily to combat what is called the small-write problem in file systems, that you're making small changes to files, and the files are scattered all over the place, and so there was a lot of disk movement, and to avoid that, if you have a log-structured file system, the log segment can be a repository for changes to the multiple files.





So for instance, this particular log that I showed you may contain the changes for file f but it also may contain changes for some of the file g, all of that can be part of the same log segment, which itself is a file, but it is containing the changes for different file.



So that way, you can reduce the number of distinct small writes that might happen to the system.



So that's a digression from the main focus of this particular lecture, just to give you an update on what a log-structured file system is.



So the haystack store is implemented with a log-structured file system, for a simple reason that it is not a file that you're changing again and again. All that you're doing is, you're taking a single file and to that single file, you're having one photograph, a second photograph, and so on, and you're just appending to this file photographs that you want to add to this particular file. If I say a single file contains 1,000 photographs, that's what you're doing, and so having a log-structured file system means that I can simply write to the end of the file.



So now, coming back to what exactly happens from your browser when you request a photo upload, what you're going to do is you're going to go to the web server from your browser, and the browser will go to this haystack directory. The haystack directory is going to return a logical volume to write to.



So we're going and asking what is the volume that I should write to and the haystack directory is going to tell you a logical volume to write to.



The haystack directory knows the occupancy of the logical volumes and it's going to return a load balanced writable volume for you so that we can reduce the amount of stress that we put on the haystack store.



Now, once we get that, the logical volume will map to a set of replicated physical volumes for availability because remember that in Cloud services, failure is a given and therefore we have to make sure that anything that we do has redundancy, and so you have a replicated physical volume representing a logical volume that the haystack directory gives you.



Once you have that, then we're going to actually do the upload by writing to the end of the volume, because this is where the log-structured file characteristic comes in.



So this is how a photo upload will happen in the haystack store.



If we want to download a photograph, what we do is we request a photo download from a browser to the web server. What the haystack directory is going to give us is the physical volume to read.



So it is going to do a load balancing decision saying, a particular photograph may be replicated in multiple physical volumes because the logical volume corresponds to multiple physical machines on which it is replicated.



So the haystack directory is going to make a decision, what would be the right physical volume that you can read from?



This is what we call the needle that we get back from the haystack directly via the web browser.



Once you have that needle, then the browser is going to decide how to get the photograph that corresponds to this particular needle. The decision whether to go to the CDN or go to the haystack, is something that is decided by the directory.



The haystack directory makes this decision for you depending on the long-tail distribution. As I mentioned, depending on the photograph that you're accessing and the popularity index which is maintained by the haystack directory, it can make a guesstimate as to whether a particular needle mapping to a photograph can be found in the CDN itself or maybe you have to go directly to the haystack cache to get that.



So that decision is something that the directory gives you. Once you have that, you either go directly. If it's part of the long-tailed distribution, you go directly to the haystack cache, and as I already mentioned because of the fact that we have multiple photographs in a single file, there's a good chance that the metadata that is associated with a particular photograph is in the haystack cache, if not, it may have to go to the haystack store and fetch the metadata and then give the real data as well.



The alternative path is of course going directly to the CDN, which is a decision that the haystack directory will make. In this case, you go to the CDN and hopefully, you'll find the picture that you're looking for in the CDN because there is a guesstimate that the directory made that most likely because of the fact that this is a popular picture that you're looking for, it will be in the CDN. But on the other hand, CDN is also caching most recent stuff responsible that what you're looking for is not in the CDN, in which case it may have to take the long route of going to the haystack cache, and going to the haystack store, getting the photograph back and then passing it back to the browser.



One of the design decisions that they make in this design is that when you take this long road, going through the CDN and finding that it's not in the CDN and going into the haystack cache, is that when you bring in the photograph, [we're not going to cache it in the haystack cache.]{.mark} The reason is because, if it is not in the CDN and you had to go to the store to get it, the next time it is going to be accessed, hopefully you'll find it in the CDN. If you don't find it in the CDN, very likely you won't find it in the haystack cache as well by the time because time would have elapsed. Therefore, they make the decision that if there is a miss in the CDN and you have to go through the haystack cache in order to get the data from the store, then don't bother caching it in the haystack cache.
