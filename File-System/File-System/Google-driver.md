# Google driver

Created: 2020-11-28 16:24:51 -0600

Modified: 2020-11-28 23:50:23 -0600

---

Welcome to systems expert let's dive into the question today I want you to design Google Drive Google Drive



okay are we designing just the storage aspect of Google drive or are we also designing some of the products like Google Docs Google Sheets slides drawings, all that stuff



we're just designing the core product of Google Drive which is the storage aspect in other words users can upload and create files which are stored somewhere in the cloud and they can access those files and folders moving around replace them etcetera



okay but even just on Google Drive dust on the storage product is there are a lot of other auxiliary features things ,like a personal Google Drive versus a shared company Google drive or a shared files and folders like two people can share a personal driver files in a personal Drive there's also stuff like what files were recently accessed activity on files are we designing all of

these things



or so let's keep things narrow and imagine that you're just designing your personal DR



So forget about like share Company drives and in your personal Google Drive you can have folders and files and that's kind of all I want to take care of and for short I'm going to be referring to these files and folders is as entities, but any other features like activity on entities or you know the recently accessed entities or start entities you can just forget that for now and you can definitely ignore the sharing aspect so she's being able to share access to folders and files ,not I'll take care of this in today design





okay so then I guess I'll write down that we are really just handling folders and files and files is it fair to say you mentioned this earlier, but is it fair to say that we are basically implementing all of the basic CRUD operations on folders and files ,like creating folders and files or I suppose uploading files ,moving them around ,renaming them deleting them is that what we're doing and downloading files



so I'll write in (here crud operations and I'll remember that we have downloading or we also designing a desktop client for Google Drive or are we just dealing with the web application



no desktop client I just want you to design the functionality of the at the web application of Google Drive that you could see if you go on the other Google Drive website



on the web I know that you said that we're not dealing with Access Control lists like sharing your files are giving people permissions to files, but do we still want to handle what would happen if for instance like I'm a single user, and I've got two tabs open of my own personal drive on some same folder, and I create a new file in that folder on one tab do I want the other tab to immediately see that, what is there conflicts is this something that we should think about in our design



as I said before we're not signing the sharing a suspect but if it's being edited by multiple clients we'd want the route changes to be reflected, let's say within 10 seconds to all clients so if one client create a file within 10 seconds the other clients should see it



but for the purpose of this question is not worry about clients attending the same entity at the same time ,don't worry I like the other conflicts that they make calls or anything like





that okay okay so I'll put just maybe like 10 second updates time, if we have multiple clients like two tabs in a browser accessing the same folder but no conflicts this will simplify a little bit of the design



I guess as far as scale like how many users are we serving? this is a global product I'm assuming but how many users are we going to be serving and how much storage do we expect these users to actually be using up in our application or in our system ?



so let's assume the system should be working for a billion people on and handle 15 GB per person on average per user, that's kind of what Google Drive gives you as a free tier so we can just assume the average is right at that limit us 15 GB





okay I'll put 1 billion users and like I said these are Global users right? 1 billion users and 15 GB per user



okay so that's going to be a lot of storage and is this a highly-available system that were looking for? so first and foremost I want to talk about data loss which is not tolerated at all we need to make sure that once or write or addition or an upload goes through and a client receives a confirmation ,it will not disappear or get lost until the user removes it or delete it and then in terms of availability what you understand we do need this to be highly available loss and highly available



that makes sense cuz when you upload something to drive you you basically assume that it is that it is safe for good, I'll put ha for highly-availablility



hese are these are two slightly separate things I think that the data loss is going to mean that we're going to want a lot of redundancy or a lot of duplication rather of our of our data



so I think that I have most of what I need to start thinking about a design I might have other questions that come a bit later on, but for now I think I should be good, what I'm what I'm thinking here is that maybe it makes sense to write out all of the operations ,all the credit operations that we mentioned it before, it got a clear picture of what exactly were supporting what exactly we're designing and then to jump into your the storage, like the backbone of our system and and everything else does that make sense as a sort of tentative plan



okay so then I'll write down here all of the operations, so the first operation is going to be what would be the simplest operation, uploading a file or actually creating a folder is probably be even simpler is ,a folder really is just ate like an arbitrary structure ,doesn't have any actual a data that you're that you're uploading, it's not like uploading an image, so create folder as the main one was the first one, then I will put upload file ,and maybe download files ,that come with it ,so upload download file



what next after I guess we get into really all of our crud operations, so things like deleting both a file in a folder, moving both file in a folder ,rename renaming both file in a folder, and getting both of them,so I'll just read this here



I'll probably right rename is effectively like edit and then get and then these are both file and folder



I guess you can't really download or upload a folder rights, you just upload or download a file , so I think that's every single operation that we want to support is that correct, let stick to those



perfect so now I can see things a bit more clearly we're going to have to figure out where we store both folders and files, I suppose we have to ask ourselves like what does a folder or a file really look like what is the data behind a folder behind a file ,and I feel like there are two different types of data, there is there's information about a file or information about a folder , this is going to be stuff like when the file or folder was created, you know what the name of the file or folder is who owns the file or folde,r that's kind of like almost like a metadata and then there's the actual contents of a folder or file



and I suppose a folder like I hinted at before a folder doesn't actually have contents, if anything's the files or other folders that are contained within a folder, they are part of the metadata, that I'm talking about but files definitely have actual content ,and that's a very different data , more of an opaque blob of data versus the more structured metadata that I just mentioned



so I think that our system is going to have to be divided into two separate pieces of storage or or storage components, one of them is going to be purely for file contents ,and I'm really thinking that we're going to be using a blobstore they are like GCS or Amazon S3 because Google Drive would not be using Amazon S3, we would you be using GCS Google Cloud Storage that lends itself perfectly to dissipate pieces of data



but then for entity information this metadata piece that I'm just driving that you like we could probably use something like a key-value store cuz that's almost like configuration parameters about a particular entity about a particular folder files,



I think I take kV-score makes more sense what do you think yeah that makes sense are you thinking about how scalable that Key value store, Are you just gonna have one? what kind of strategy for storage you're going to be using and what also a kind of key value stores are you going to be using? cuz different ones give you different guarantees ?



May be we can just start the key value store section of the system and so this would be for the for the information that were storing about entities? maybe I'll all star with create folder actually and I'll get into exactly the Key value store look like in a second so answer your question



the second but if you create a folder all you're really doing is storing information about that fooled or the representation of that folder in storage ,so this is but This lends itself perfectly good to what we want to do right now which is to describe how these KV stores work when you create a folder you're going to store stuff in the KV store, and I suppose to your question we would want to have highly available KV stores because we need that high availability that we talked about before



we are going to want to probably shared our KV stores because we're dealing with so many users know billion users from trying to think like would we want to start on a regional level, region Regional shading is offen, pretty useful, but here I'm thinking that I know that once again we're not dealing with sharing we're not dealing with permissions ,but you could still have we still have multiple, like we still eventually have to support having one person looks and the United States accessing a file or folder ,and another person in India accessing the exact same file or folder and if we do eventually have to support that, I don't think that we can start on regions or if we do is going to be a lot more complicated because of replicating data across regions and all that so I'm actually thinking that we may want to shared our key value stores either on the on the file name or the entity name,like if we're talking about a folder on the folder name or maybe the name of the CID



we would be using the the ID some kind of unique ID that every entity is given them wondering if that might be that might actually be a little bit more cumbersome, cuz then like it begs the question where is that I degenerated ,like it's going to be generated client-side, but I actually think that he might be just easier to shard kv stores based on the owner ID, so instance, the person who owns the drive in which a folder or file is created, they would likely have a unique ID as a Google user, and we would use that ID as the sharding key for kv store starting strategy does that make sense



~~to go with this then and I'll maybe give myself some space here and write rkv stores here on the right ,~~so you'd have multiple KV stores , I'll drive couple circles these would be kv stores you said what types of KV stores, we said we want them to be highly available, I think we could probably go with something like at CD or zookeeper, those would give us some good guarantee is there and idea is again we would have multiple or a lot of these KV stores and we would be storing stuff in them based on the owner idea of the entity that were storing



okay that is that is the storage solution, I suppose if we're if we go down to the client level like the client issuing the create folder request ,what's the write create folder right here if it's drizzling a crate full to request we definitely need some kind of load balancing,





cuz we're not going to be hitting these KV store for his with a billion users ,so we'll have some kind of load balance here since year all right LD and this would be a based on the owner ID when I'm going to call the owner ID ,on the reason I'm not calling you this user ID, is because if a user is creating it in somebody else's driver, know if they have granted that permission even though I guess we're not handling that but if we have we want the owner of the drive or the owner of the of the parents of the folder to be to be the sharding key.



owner ID and we would hatch on this owner ID, and I'm trying to think would we want we would probably here have some proxies that live in front of the KV stores, at least proxies could handle and maybe some cashing if we do have ACLS eventually, your permissioning they could do the checking of if a user has permission to create a folder here or whatever does that make sense to have a couple of proxy server or a bunch of proxies here



I guess I'll just write you know a bunch of circles here and these are going to be our ,so the load balancer is going to like this create folder requests are going to the load balancer ,the load balancer then goes to all of the proxies ,based on the owner ID hash, and then the proxies go to all the corresponding KV stores, do use some extra logic whatever we want to be there



now as far as what the actual entity information looks like in the KV stores, maybe I should write that down I'll write it down to the side here, so we would have probably has a lot of properties to store for entity, and all sorts of metadata about the entity ,but the ones that I'm thinking of right now would be probably get you no name of the entity, and maybe I'll put a bracket here we would have maybe we would store the owner ID, so we would probably store I'm thinking that a folder is going to have to store pointers to its children meaning all of the things but it's stores inside of it probably has no a list of folders and files and these would be a folder and file IDs



so here all right children, I'll just make it an empty list for now, this would be the default thing if you're rude also have I guess a unique ID and that's about it for now I suppose if we are going to have we could use maybe the same object for the same schema for both folders and files, and just have some sort of Boolean that says he is folder, I'll do is this could also have his different types of entities ,but I'll just go with a Boolean for now, so no is folder and this would be just true



okay so that's kind of what I'm thinking about for this for now, and that's it I think for creating a folder right, that's all they are doing creating a folder comes down to creating this object and storing it in one of the key value stores ,so I think we can move on to storing actual data which means file data and to store files that are we would probably have to upload a file, so it would be the upload operation, that would trigger this alright upload operation here



okay upload file I suppose after uploading will get into downloading is probably



so if we are uploading a file and say we're uploading an image or a big document, maybe a good a good way to start this will be to estimate how much storage we're actually going to need to store your over users data



so we had said we're going to have a billion ,users 15 GB per user so let's quickly estimated that a billion is 1000 ^ 3 so that's 15 * 1000 that's 16 GB * 1000 again 15 petabytes



and multiply that by a thousand again where we are at 15000 petabyte storage



that's a lot of data and we also have the no data loss demand and no data loss means that we need to have like probably duplication or even more of our data, I think that we would probably want to has like our data stored in multiple different availability zones ,such that if one or even two availability stones are hit by natural disasters or huge outages ,there's always one that still has a users data



so we need 3 min sources of Truth for a piece of data, that means we need 45000 petabytes a 50,000 petabytes, I'll write this down the scale here 50000 petabytes petabyte ,



I'm trying to see if there's a way to optimize this cuz I think at a Google scale we can afford to have 50,000 petabytes it's just a lot of machines ,but there's there's certainly one optimization that comes to mind which is I don't know if they would if they don't necessarily impact this value this certainly still is our upper Bound for how much storage we need, but one way that we could optimize this store less data, basically is not show me how designers system in a way that duplicate data doesn't get scored twice, and I'm not talking about the no data loss duplication, that is voluntary duplication, but I imagine I upload the same picture twice to my Google Drive or imagine the two users upload the same picture or the same document or whatever right ,completely separate users, who live across the world from each other ,if they upload the same exact file, you don't want to spoil that twice or if you do for a twice, it's not an optimal use of your resources



so I'm trying to think if there's a way that we can basically have like in the same way that here we said we would have these Global key value store, we could have Global blob stories hear that that somehow avoid any duplicate data ,from being uploaded sore to users or uploading the same data then they just they don't actually like one of them doesn't actually upload that data it just accesses the previously uploaded version of that data, does that make sense as a as a kind of like direction to go in



or yeah so by access you just me and when it tries to upload a certain part of that data and just got the confirmation that it's already there,



yeah and I guess the fact that you said part of data makes me realize that we could actually split up any file that gets uploaded into smaller blobs of data ,because there might be duplication in in smaller pieces of data ,in other words to user is my upload to images that share some data in a images, maybe like half of that is identical to the other half is where the advantages differ, and so you would split the images up ,and smaller blobs that constitute the full image, we would check if the smaller blobs are already stored in the blob storage ,and if they are ,then we just wouldn't upload them, wouldn't re-upload then we would just say hey is that thing that we already have their ,that's going to be your where this part of the file lists



I think we're going to do that so the idea would be if a user upload the file, we have to go to some API servers, so you know we're going to have some API server here I ,as suppose as usual we would have to do some load balancing ,so I'll actually move these little bit up



we would have some load balancing here, load balancing hear that splits up our request or or rather distributes are upload file requests across the API servers, and these API servers their duty would be to split up a file, that's being uploaded into blobs, into smaller Blobs of data and two and we would have some kind of you know algorithm or logic that determines you how many blobs what the size of these blobs is and so on and so forth



but they would split up the blob and then they would store them in our final blobs solution ,and we would store them I think we would have to synchronously store then in multiple blob stores, cuz like we said we want that no data loss thing and I think I said three like middle-aged go with a minimum of three blob stores ,so we would synchronously store all these pieces of data in or three minimum free blob stores, and maybe we can have some asynchronous replication have even more a blob storage container all the information, and we would do that fact that there is like check here, is this blob already store in the blobs



before finish this drawing out how would we check though if a blob is already contained in the blob store we could we could have asked our content we could take a blob, hash the contents of The Blob and compare that to all of the block compared to all the blob that complicate or







every blob has an address, so we actually named ,we name our blobs, we name the address of the pointer to a given blob by hash of the content contents, so if you have a piece of data ,it right here, at say this is the this is your blob, you hash the contents ,



you get a value would say x ,and if x is already in your blobstore, then that means that if x is a pointer or an address in your blob store ,and that means that this content is already stored in your Bob's store ,and you can just throw this out you don't need to store it does that seem sensible



yeah and that's actually called content addressable storage ,and it's actually really good to be using it here it makes a lot of sense



first of all that means it every single blob would be effectively immutable, as if we were to make flavored like upload a slightly different blob, it would have a different hash and so we would highly the contents would be has two differently, and so we would have a separate wild entirely stored which is not bad, cuz it's immutable ,then if we do need some kind of cashing ,somewhere in our system of cashing is going to be a little bit simplified, right you don't need to do any we don't need to like overly optimize to have our cash be in sync with our source of Truth or cache is always going to be pointing to a mutable blobs of that makes sense



so I kind of like this this this approach ,so I guess I'll complete my drawing out now, erase that and here's a load balancer ,would it be doing owner ,ID, hashing I don't think we need to, cuz we are storing these blobs not based on owner ID just like round-robin a load balancing here ,alright round-robin so here we're getting a bunch of request to going to this load balancer, this load balancer then goes to all of our API servers, and I guess I'll call these API servers, like blob Splitters, their job is to split the blob, of being uploaded with a piece of data is a blob Splitters



and then we go to our blob stores ,and I suppose if we do want some kind of cache or some other processing are blobs in a maybe we're going to be like compressing or files or something if you want to do any kind of processing, we might have just like we have proxies here, instead of proxy here, I'm so I'll put potential proxies here, proxies and we would have probably another load balancer here, and this load balancer would it weird if we're going with cashing it would go with like we would low down is based on blob hash, ~~help I'll put this year this goes here is goes here~~ we could do some some blob hash balancing here and I guess I'm a b I'm complicating the system a little bit here by adding this additional functionality but I do think that it could be helpful



in theory we don't need this entire section I think we could just go from the log splitters directly to the blobs for solution does that make sense that are you following me



yeah but I think in reality ,the fact that you have in your immutable blockages is very useful as you said for cashing and so I think being able to have like two separate layer proxies that might do something a little bit different, like you said compressor , cache I would be very useful especially since the blob stores are going to be kind of on the global plan, so latency to hit them ideal for inside the casting would end up being pretty useful especially if you got users were using drive very frequently



okay so I'll keep this in for the proxies and even like in red that we can do some cashing here and again the cache would be done is being based on the blob hash, and so this would be load balancing here and then we go to our final blob storage solution up here so here we would have three blob stores that I went with three different duplication or triplication I suppose at minimum



but I think we might even want more so like I mentioned before I think we could have a bunch of other blob store living behind the scenes, that we would asynchronously replicate too, but fear we would synchronously write the blobs to each of these blob store is, so basically like a single proxy here ,~~that gets a single blob would go here and hear a this one would go here here and here and I won't skip the third one but you got the idea~~ and so these are our blob stores ,~~all right there snow blob stores t~~he idea is that these three blob store ,would have to be picked a very intentionally you would want them to be in different availability zone is a hurricane you don't want all three of these to get hit, these my to live in the same data center or at least like two of them live in the same Data Center ,and maybe on a different part of the network or something that way they're isolated from each other and then you probably want one in a completely different region



and then even if one goes down ,you still have the other two, so maybe like this isn't too hard requirements, that they're write here succeed across all three maybe you just need them to succeed across two of them and you know you trust that that's enough and then they immediately has a sync replication with the others ,and hear another service so I'll actually erase this you you would probably have some kind of a async manager here or controller async rep controllers ,and you know this would kind of go to all the blob scores and a synchronously replicates to the others does that make sense



this satisfied data loss or no data loss policy also satisfies our high availability policy, so I'm pretty happy with our or blob storage here, and we still get decent latency even the fact that this is a global these are Global blob stores, I think we get decently and see cuz we can make use of a cashing like you said here, like overall we might have cash, in here and in general this is Google Drive so we we are okay with way used to see your folder completely load like from my experience with Google Drive latency is not the most critical thing it is like you said the no data loss in the high of a liability



~~so overall I'm I'm pretty happy with this maybe we can talk about the other operations now so download file with C with download file be difficult it ain't download file would be oh and~~ I suppose wait I did forget one thing about uploading a file when you upload a file, you first you first stored in Blob storage and then you need to do what we did for create folder right and stored in the KV store



and so here maybe maybe cuz I guess I guess in this object we would probably want to have like blobs and this would be a list for folder is it would probably be empty but for files it would be a list of blob addresses for pointers, that way we we can just go from here ,we can just point to all of our blobs in here the blob hashes, but we do have to store this and we can only store this once we stored the blobs, I supposed to the blobs the really the critical thing that we have to store so, maybe uploading file,



you to keep this simple to so as to add another service here I think we might be able to naively just store in blob for it get a response from our servers, get back to the client wants the client gets service ,what's that the blob has been stored ,and has all the hashes we just like ghosts here, and you make a request here to store the object with the blobs that make sense



that's the good thing about this is that even if they say like request fail or something and you maybe we could do a retry, with our solution of checking to see if Blobs of data already live in our blobstore, we can just re upload the file ,and it just will not get uploaded will just be effectively making a check to see if that file is already uploaded ,





and it's for downloading the file I think we're actually dealing with something very simple where we first go to the KV store and get the object final all blobs, go to the blob store, get all the blob hashes, put them together, like create the puzzle, and get the file then download it.





if we're renaming a file or folder this seems like it's very easy all that we do is we go to the KV store and go to the load balancing and the proxies and all that and we just added a field in this ,so I think that this should be fine



then if we're removing rather moving a file or folder that actually gets a little bit tricky because if we're moving a file, from one folder to another folder, we have to update the parents folders both of them ,we have to update to remove the file or the folder from its original Parent folder, and we have to add it to its new parent folder ,so I think we might want to structure our entity infos here, these objects we might want to make them and like trees data structures except that they have a pointer to their parents



for their doubly linked is that makes sense, so here we would erase this, I'll grab this this little bracket and move it down, and we would have his parents, and this is this points to the ID of the parents ,or the parent ID ,



is that could be useful when trying to put in a folder and you're trying to go up a folder that sits you actually need this otherwise you'd have to scan the whole the whole store



so so then that's what we'll do and I guess moving a file or folder would be that you update the new update the parents of the thing you're moving, and then you update the children of the old parents, and then you parent that's pretty simple



deleting would be the same thing deleting you would just find the thing delete the object will remove it from the KV store and then update the parents children ,I supposed to do we want to handle things like the concept of a trash folder, where an item doesn't actually get deleted immediately it just gets placed in the trash



for a good without that so I think we don't really need to talk about this too much at the end of the day, you can think of the trash is like a special folder, maybe with a special indexing so that some may be other process can go through and clean things up



afterwards but we have to go into it and I think that's sort of how I would I would have thought to do it if we did have a trash folder but this simplifies things



and I guess then the last thing that we would need would be to remove the blob ,that I guess handling blogs maybe a little cranky ,cuz we don't need to remove the blobs from any object, here we just we just delete the object of the entity that were deleting ,because this is a file, but do we want to delete the blobs from the Blob storage ,we actually can't do that because of the fact that our design rely so heavily on blob deduplication, or blob reuse



we might have other entities that are pointing to the blobs that that are being used here and there are effectively being kind of deleted, so let me think we don't want to delete blobs at the same time I don't know if it's a good idea to just to leave I guess I'll actually has been deleted in the sense that it's no longer being used at all



really say that I've got an image that is completely unique and I'm deleting it we would want to get rid of those blobs and blob storage ,so I'm wondering if we could have some kind of like an auxiliary service here, that lives in between the KV stores and our blob stores and that either watches the KV stores for updates to blobs and see like how would we do this we would have to know if a particular blob



maybe like simplify things and deal only with one blob ,but if we have a single blob we would have to know if it is being still used by an entity, and we would know that from the KV stores ,cuz the KV Sports have all the information that we need right, like in other words we can have a service that aggregate all of the references, to a particular blob ,or maybe that like ,maybe we have multiple services that watch all of our KV stores for like blob hashes, and keep track of it at any point in time of blob hash which is like a blob hatches reference 0 times,





so I guess we probably would need another storage solution like another database where we we hold like the number of times a single blob has been referenced, I going completely off Mark hear



you're designing garbage collection system which I think is roughly how that would work here do reference counting so counting how many times did you have this blob reference and then once it drops down to zero, you could probably market for deletion and but the blob stores would have to implement some kind of internal operations that allow you do this in a concurrency safe manner





right I feel like a little bit tricky do you want me to get into that or no, no I don't think you need to get us that it could be it could be a systems design question



just clarify it's on the same page as you the ideas you would have a garbage collecti here so I'll put garbage collector DC and this thing with live kind of sort of in between the key value stores and the blob storage right



and it would also talk to a database to keep that the reference count for each be going to



that then the idea is there would be some entirely different system here that handles all that and not make her blob spores hold useless unneeded data



but I think this is everything then maybe the last thing that we forgot is the updates as we did say I think we said no conflict, but we want 10 seconds update if we're like multiple clients talking to the same folder or something you know like a renamed to here I think we can do this very easily with just some pulling



it may be here we talked about deleting renaming and moving before getting is saying it's literally just go to the store in and boom you have the object with all the description so we can have we can pull or get like our get operation get file or folder, we can pull that whole every 10 seconds where every 8 seconds or something from the client



and this would be a very simple baby naive way to to implement that part of the system



I think it could work potentially you could imagine that like whenever an entity info changes in your key value storage, sets kind of the last modified time, and so your proxies and red there could easily kind of I guess figure out if something is changed recently or not



these proxy so you're saying like instead of pulling we could have these like push to the client's



no if we are worried about the amount of data that's get file or folder are returns, with a lot of different users we could use that to just return the last modified time, and if that has changed then you can get the rest of the info,



oh yeah yeah oh yeah I think that covers the the rest of the of the things that you wanted me to cover what are the features of the system is there anything that I'm missing here or no I think you've covered all the operations we wanted and you went through all the other requirements so I think this looks good thank you would that I hope that you found this video informative and we'll see you in the next one
