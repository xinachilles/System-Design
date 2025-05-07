# Cache Consistency: Frangipani

Created: 2020-06-19 11:29:15 -0600

Modified: 2020-06-19 16:47:37 -0600

---

overall design a Frangipani



picture is that you have a bunch of users each user in the papers world is sitting in front of a workstation one is sitting front of a computer workstation I'm gonna call the workstations you know workstation one more station to each workstation runs an instance of the frangipani server



they might be running ordinary programs like a text editor that's reading and writing files

and maybe when they finished editing a source file when these ordinary programs make file system calls inside the kerne, there's a frangipani module that implements the

file system inside of all of these workstations



Then the real storage of the file system data structures things like certainly file contents but also I nodes and directories and alias file and each directory and the information about what I knows and what blocks are free all that's stored in a shared virtual disk surface called petal. it's on a separate set of machines that are you know probably server machines and a machine room rather than workstations



on people's desks pedal among many other things replicates data so you can sort of think of pedal servers is coming in pairs and one crashes we can still get at our data and so



when frangipani needs to read or write a you know read a directory or something it sends a remote procedure call off to the correct pedal server to say well here's the block that I need you know please read it for me please return that block



for the most part petal is acting like a disk drive you can think of it as a kind of shared as a shared disk drive that all



the reasonably important driver in the design what they wanted was to support their own activities and what they were members of a research lab of maybe say 50 people in

this research lab and they were used to shared infrastructure things like time sharing machines or workstations using previous network file systems to share files among cooperating groups of researchers





they're really interested in a shared file system for human users in a relatively small organization small enough that everybody was trusted all the people all the computers so really the design has essentially nothing to say about security





the real copies of files are stored in this shared disk it's fantastic if we can have some kind of caching so that after I log in and I use my files for a while they're locally cached here so they can be gotten gotten that and you know microseconds instead of milliseconds if we have to fetch them from the file servers ok so French Pyrenees supported this this kind of caching





furthermore it supported write-back caching not only caching in each in each workstation and each frangipani server we also have write back caching which means that if I

want to modify something if I modify a file or even create a file in a directory or delete a file or basically do any other operation in own workstation and that means that my writes stay only local in the cache if I create a file at least initially the information about the newly created file said a newly allocated inode with initialized contents and you know a new entry added to a new name attitudes to my home directory, all those modifications initially are just done in the cache





they're not written back in general to peddle until later so at least initially we can do all kinds of modifications to the filesystem at least to my own directories ,my own files completely locally





that's enormous ly helpful for performance it's like a you know factor of a thousand difference being able to modify something in local memory versus having

to send a remote procedure calls to send server







the first challenge is that suppose workstation one creates a file in you know maybe a file say a, new file a and initially it just creates this in its local cache so that you know



first it may need to fetch the current contents of the slash directory from petal nom but then when it creates a file just modifies, its cached copy and doesn't immediately send it back to peddle then there's an immediate problem here suppose the user on workstation 2 tries to get a directory listing of the directory slash right we'd really like to be able to this user see the newly created file right and that's what users are gonna expect and users







which means that the file system has to do something to ensure that readers see even the most recent writes so we've been talking about this as we've been calling this you know strong consistency and linearize ability





before



and that's basically what we want in the



context of caches though like the issue



here is not really about the storage



server necessarily it's about the fact



that there was a modification here that



needs to be seen somewhere else and now



for historical reasons that's usually



called cache coherence that is the



property of a caching system that even



if I have an old version of something



cached if someone else modifies it in



their cache then my cache will



automatically reflect their



modifications so we want this cache



coherence property another issue you



have is that the you know everything all



the files and directories are shared we



could easily have a situation where two



different workstations are modifying the



same directory at the same time so



suppose again maybe the user one on



their workstation wants to create a file



/a which is a new file in the directory



slash in the new in the root directory



and at the same time user two wants to



create a new file called slash B so at



some level you know they're creating



different files alright a and B but they



both need to modify the root directory



to add a new name to the root directory



and so the question is even if they do



this simultaneously you know to file



creations of differently named files but



in the same directory from different



workstations will the system be able to



sort out these concurrent modifications



to the same directory and arrive at some



sensible result and of course the



sensible result we want is that both a



and B end up existing we don't want to



end up with some you know situation in



which only one of them ends up existing



because the second modification



overwrote and sort of superseded the



first modification



and so this is again it goes by a lot of



different names but we'll call it a de



Missa T we want operations such as



create a file to lead a file to act as



if they just are instantaneous



instantaneous and time and don't ever



therefore don't ever interfere with



operations that occur at similar times



by other workstations



well things to happen just at a point in



time and not be spread over even if



they're complex operations and involve



touching a lot of state we want them to



appear as if they occur instantaneously



at a final problem we have is suppose



you know my workstation is modified a



lot of stuff and maybe it's



modifications are or many of its



modifications are done only in the local



cache because of this right back caching



if my were station crashes after having



modified some stuff in its local cache



and maybe reflected some but not all



those modifications back to storage



pedal other workstations are still



executing and they still need to be able



to make sense of the file system so the



fact that my workstation crashed while I



was in the middle of something had



better not wreck the entire file system



for everybody else or even any part of



it so that means what we need is crash



recovery of individual servers we won't



be able to have my workstation crash



without disturbing the activity of



anybody else using the same shared



system even if they look at my directory



in my files they should see something



sensible maybe it won't include the very



last things I did but they should see a



consistent file system and not a rekt



file system data structure so we want



crash recovery



as always with distributed systems



that's made more complex because we can



easily have a situation where only one



of the servers crashes but the others



are running and again for all of these



things for all three of these challenges



they're really challenged we're in this



discussion their challenges about how



frangipani works and how these



frangipani



software inside the workstations work



and so when I talk about a crash I'm



talking about a crash of a workstation



and it's frangipani you know the pedal



virtual disk has many similar questions



associated with it but there are not



really the focus today it has a



completely separate set of R'lyeh fault



tolerance machinery built into pedal and



it's actually a lot like the chain



replication kind of systems we talked



about earlier ok so I'm going to talk



about each of these challenges in turn









the first challenge is cache coherence and the game here is to get both the benefits of both linearize ability that is when I read when I look at anything in the filesystem I always see fresh data I always see the very latest data so we got both linearize ability and

caching that's good caching as we can get for performance so somehow



the frangipani servers and workstations and pedal servers there's a third kind



of server in the frangipani system



there's lock servers and so we're I'm



just gonna pretend there's one lock



server although you could shard the



locks over multiple servers so here's a



lock server it's a separate you know



it's logically at least a separate



computer although I think they ran them



on the same hardware as the pedal



servers but it basically just has a



table of named locks and locks are named



we'll consider them to be named after a



named as after file names although in



fact they're named after I numbers so we



have for every file we have a lock



potentially and each lock is possibly



owned by some owner for this discussion



I'm just gonna assume I'm gonna describe



it as if the locks were exclusive locks



although in fact frangipani has a more



complicated scheme for locks that allow



either one writer or multiple readers so



for example maybe file X has recently



been used by workstation 1 and



workstation 1 has a lock on it and maybe



file Y is recently used by workstation 2



and workstation 2 has a lock on it and



the lock server will remember off or



each file



who has the lock if anyone maybe nobody



does on that file and then in each



workstation



each workstation keeps track of which



locks it holds and this is tightly tied



to it



I'm keeping track of cache data as well



so in each workstations frangipani



module there's also a lock table and



record what file the more session to



lock for what kind of lock it has and



the contents the cached contents of that



file so that might be a whole bunch of



data blocks or maybe directory contents



for example so there's a lot of content



here so Linda frangipani server decides



oh it needs to read it needs to use the



directory slash or look at the file a or



look at an inode it first gets asked the



lock server for a lock on whatever it's



about to use and then it asks petal to



get the data for whatever that file or



directory or whatever it is and it needs



to read and then the workstation



remembers oh ho you know I have a copy



of file X its content is whatever the



content of file X is cached and it turns



out that workstations can have a lock in



at least two different modes what the



workstation can be actively reading or



writing whatever that file or directory



is right now that it's in the middle of



a file creation operation or deletion or



rename or something so in that case I'll



say that the lock is held by the



workstation and is busy it could also be



after a workstation has done some



operation like create a file or maybe



read a file



you know then release the lock as soon



as it's done with that system call



whatever system call like rename or read



or write or create as soon as the system



calls over the workstation will give up



the lock at least internally it's not



actively using that file anymore but



it'll as far as the lock server is



concerned the workstation will hold the



lock but the workstation notes for it



its own use that it's not actively using



that lock anymore as well call that the



lock is still held by the workstation



I'm just but the work station isn't



really using it and that'll be important



in a moment okay so I think these two



are set up consistently if we assume



this is workstation one the lock server



knows Oh locks for x and y exists and



they're both held by workstation one



workstation one has equivalent



information in its table it knows it's



holding these two blocks and furthermore



it has the it's remembering the content



is cached for the filers of directories



that the two locks cover there's a



number of rules here that in that



frangipani follows that caused it to use



locks in a way that provide cache



coherence then sure nobody's ever



meaning using stale data from their



cache so so these are basically rules



that are using conjunction with the



locks and cache data so one the really



overriding invariant here is that no



workstation is allowed to cache data to



hold any cached data unless it also



holds the lock associated with that data



so basically it's no cache data without



a lock without the lock that protects



that data and operationally what this



means is a workstation before it uses



data it first acquires the lock on the



data from the lock server and after the



workstation has the lock only then does



the workstation read the data from petal



and put it and put it in its cache so so



the sequence is you can acquire a lock



and then read from petal



I'll tell you at the lock of course you



know you weren't passing the data you



want to catch the data you first got to



get the lock and only strictly



afterwards read from petal and if you



ever release a lock then the rule is



that before releasing a lock you first



have to write if you modified the lock



data in your cache before you release



the lock you have to write the data back



to modify data back to petal and then



only when petals as yes I got the data



only then you'll have to release the



lock that is gives a lock back to the



lock server so the sequence is always



first you write the cache dated a petal



storage system and then release the lock



and erase the entry whoops



erase the entry and the cat and the



cache data from your from that



workstations lock table what this



results in the the protocol between the



lock server and between the workstations



and the lock server consists of four



different kinds of messages this is the



coherence protocol these are just



network you can think of them as



essentially sort of one-way Network



messages there's a request message from



from workstations to the lock server



request message says oh hey lock server



I'd like to get this lock when the lock



server is willing to give you the lock



and of course if somebody else holds if



the lock server can't immediately give



you the lock but if when the lock



becomes free the lock server will



respond we have a grant message then the



lock server back to the workstation in



response to an earlier request well if



you request a lock for the lock server



and someone else holds the lock right



now that other workstation has to first



give up the lock we can't have two



people owning the same lock so how are



we going to get that works



the lock well what I said here is that



when a lock station is you know when



it's actually using the lock and



actively reading or writing something it



has the lock and it's marked it busy but



the workstations don't give up their



locks ordinarily when they're done using



them so if I if I create a file and then



create system call finishes I'll still



have that file that new file locked and



also own the lock for that my



workstation will still all in the lock



for that file it'll just be in state



idle instead of busy but as far as the



lock server is concerned



well my workstation still has the lock



and the reason for this the reason to be



lazy about handing locks back to the



lock server is that if I create a file



called Y on my workstation



I'm almost certainly going to be about



to use Y for other purposes like maybe



write some data to it or read from it or



something so it's extremely advantageous



for the workstation to sort of



accumulate locks for all of the recently



used files in the workstation and not



give them back unless it really has to



and so in the ordinary in the common



case in which I use a bunch of files in



my home directory and nobody else on any



other workstation ever looks at them my



workstation ends up accumulating dozens



or hundreds of locks in idle state for



my files but if somebody else does look



at one of my files they need to first



get the lock and I have to give up the



lock so the way that works is that if



the lock server receives a lock request



and it sees in the lock server table AHA



you know that lock is currently owned by



workstation 1 the lock server will send



a revoke message to whoever the



workstation that currently owns that



lock saying look you know somebody else



wants it please give up the lock when a



workstation receives a revoke request if



the lock is idle then if the cache data



is dirty the workstation will first



write the cat dirty data that modified



data from his cache back to peddle



because the rule says the rule that in



order to never cache data without a lock



says we got our right the modify dated



back to peddle before releasing so if



the locks idle would first write back



the data if it's modified back to peddle



and



then send a message back to the lock



server saying it's okay we give up this



lock so the response to revoke send to a



workstation is the worst station sends



it released of course if the worst



station gets a revoke while it's



actively using a lock while it's in the



middle of a delete or rename or



something that affects the locked file



the worst station will not give us a



lock until it's it's done using and



until it's finished that file system



operation whatever system call it was



that was using this file and then the



lock in the worst stations lock state



will transition to idle and then you'll



be able to pay attention to the revoke



request and after writing to peddle if



need be released the lock alright so



this is the is the coherence protocol



that fringe that well this is a



simplification of the coherence protocol



that frangipani uses as I mentioned



before what's missing from all this is



the fact that locks can be either



exclusive for writers or shared for



read-only access and just like petal is



a block server and doesn't understand



anything about file systems the lock



server also these IDs these are really



lock identifiers and the locks are



doesn't know anything about files or



directories or file system it just has



these it's just has this table with



opaque IDs and who owns you know that



name locks and who owns those locks and



it's frangipani that knows ah you know



the lock that I associate was he given a



file has such and such an identifier and



as it happens prin Japan uses unix-style



I numbers or the numbers associated with



files instead of names for locks so just



to make this coherence protocol concrete



and to illustrate again the relationship



between petal operations



and lock server operations let me just



run through what happens if one



workstation modifies some file system



data and then in another workstation



means to look at it so we have two



workstations the lock server so the way



the protocol plays out if workstation



one wants to read since a workstation



one wants to read and then modify files



e so before it can even read anything



about Z from peddle it must first



acquire the lock for Z so it sends an



acquire request to the lock server maybe



nobody holds the lock or lock servers



never heard anything about it



so the locks are makes a new entry for Z



and it stable returns our reply saying



yes



you own the grant for lock C and at this



point the workstation says it has the



lock on file Z isn't entitled to read



information about it from petal so at



this point we're gonna read Z from petal



and indeed workstation one can modify it



locally in their cache at some later



point maybe the human being and sitting



in front of workstation two wants to



also to read file Z while the



workstation two doesn't have the lock



for files the ISA the very first thing



it needs to do is send a message the



lock server saying oh yeah I'd like to



get the lock for file Z the lock server



knows it can't reply yes yet because



somebody else has the lock namely



workstation one my the lock server sends



in response a revoke



the workstation one workstation one not



allowed to give up the lock until it



writes any modified data back to the



pedal so it's now gonna write the model



anything modified content the actual



contents of the file with always



modified back to pedal only then is



workstation two allowed to send a



release back to the lock server the lock



server with must have kept a record in



some table saying well you know there's



somebody waiting for lock Z as soon as



its current holder releases that we need



to reply and so this receipt of this



release will cause the lock server to



update its tables and finally send the



grant back to or station two and at this



point now our station two can finally



read files even pedal this is how the



cache coherence protocol plays out to



ensure that everybody who does a read



doesn't read the data until whoever the



previous until anybody who might have



had the data modified privately in their



cache first writes the data back to



pedal all right so the locking machinery



forces reads to see the latest right so



what's going on there's a number of the



optimizations that are possible in these



kind of cache coherence protocols



I mean I've actually already described



one this idle state the fact that



workstations hold onto locks that



they're not using right now instead of



immediately releasing them that's



already an optimization to the simplest



protocol you can think of and the other



main optimization is that the frangipani



has is that it has a notion of shared



versus shared read locks versus



exclusive write locks so have lots and



lots of workstations need to be



the same file but nobody's writing it



they can all have a lock a read lock on



that file and if somebody does come



along and try to write this file that's



widely cached they first need to first



revoke everybody's read lock so that



everybody gives up their cached copy and



only then is a right or allowed to write



the file but it's okay now because



nobody has a cache copy anymore so



nobody could be reading stale data while



it's being written all right so that's a



cache coherence story driven by driven



by the locking protocol next up in our



list of yes yes that's a good question



in fact there's a risk here in the



scheme I described that if I modify a



file on my workstation and nobody else



reads it for nobody else reads it that



the only copy of the modified file maybe



have some precious information in it is



on in in the cache in RAM on my



workstation and my works they were to



crash then and you know we hadn't done



anything special then it would have



crashed with the only copy of the data



and the data would be lost so in order



to forestall this no matter what all



these workstations write back anything



that's in their cache any modified stuff



in their cache every 30 seconds so that



if my workstation crash is unexpectedly



I may lose the last 30 seconds at work



but no more there's actually just mimics



the way ordinary Linux or UNIX works



indeed all of this a lot of the story is



about in the context of a distributed



file system trying to mimic the



properties that ordinary unix-style



workstations have so that users won't be



surprised by frangipani it just sort of



works much the same way that they're



already used



all right so our next challenge is how



do you atomicity that is how to make it



so even though when I do a complex



operation like creating a file which



after all involves marking a new I



knowed as allocated initializing the



inode the I knows a little piece of data



that describes each file maybe



allocating space for the file adding a



new name in the directory for my new



file there's many steps so many things



that have to be updated we don't want



anybody to see any of the intermediate



steps we want people you know other



workstations to either see the file not



exist or completely exist but not



something in between one atomic



multi-step operations alright so in



order to implement this in order to make



multi-step operations like file create



or rename or delete atomic as far as



other workstations are concerned



frangipani has a implement the notion of



transactions that is as a complete sort



of database style transaction system



inside it again driven by the locks



furthermore it it's it's this is



actually distributed transaction system



and we'll see more we'll hear more about



distributed transaction systems later in



the course



there are like a very common requirement



and distributed systems the basic story



here is that frangipani makes it so that



other workstations can't see my



modifications until completely done by



an operation by first acquiring all the



locks on all the data that I'm going to



need to read or write during my



operation and not releasing any of those



locks until it's finished with the



complete operation and of course



following the coherence rule written all



of the modified data back to petal



so before I do an operation like



renaming like moving a file from one



directory to another which after all



modifies both directories and I don't



want anybody to see the file being in



either directory or something in the



middle of the operation in order to do



in order to do this French penny first



acquires require all the locks for the



operation then do everything like all



the updates right the frangipani so I



write to pedal and then release and of



course this is easy button and you know



since we already had the locking server



anyway in order to drive the cache



coherence protocol we buy just by you



know making sure we hold all the locks



for the entire duration of an operation



we get these indivisible atomic



transactions almost for free so an



interesting thing to know and that's



basically all there is to say about



making operations atomic and transit



Pandu's hold all the locks an



interesting thing about this use of



locks is that trends of pennies using



locks for - almost opposite purposes for



cache coherence frangipani uses the



locks to make sure that writes are



visible immediately to anybody who wants



to read them so this is all about using



locks essentially to kind of make sure



people can see writes this use the



blocks is all about making sure people



don't see the writes until I'm finished



with an operation because I hold all the



locks until all the rights have been



done so they're sort of playing an



interesting trick here by reusing the



locks they would have had to have anyway



for transactions in order to drive cache



coherence



all right so the next interesting thing



is crash recovery we need to cope with



the possibility the most interesting



possibility is that a workstation



crashes while holding locks and while in



the middle of some sort of complex set



of updates that is a reforestation



acquired a bunch of locks it's writing a



whole lot of data to maybe create or



delete files or something has possibly



written some of those modifications back



to pedal because maybe it was gonna soon



release locks or had been asked by the



lock server to release locks so it's



maybe done some of the rights back to



pedal for its complex operations but not



all of them and then crashes before



giving up the locks so that's the



interesting situation for crash recovery



so there's a number of things that that



don't work very well for workstation



crashes crashing we hope one thing that



doesn't work very well is to just



observe the workstations crashed and



just release all its locks because then



if it's done something like created a



new file and it's written the files



directory entry its name back to pedal



but it hasn't yet written the



initialized inode that describes the



file the inode may still be filled with



garbage or the previous file some



previous files information in petal and



yet we've already written the directory



entry so it's not okay to just release a



crashed file servers release of crash



were stations locks another thing that's



not okay is to not release the crashed



workstations locks you know that would



be correct because you know if it



crashed while in the middle of writing



out some of this modifications the fact



that it hadn't written out all of them



means a can't of release its locks



so simply not releasing its locks is



correct because it would hide the this



partial update from any readers and so



nobody would ever be confused by seeing



partially updated data structures in



petal on the other hand you know then



anybody you needed to use those files



would have to wait forever for the locks



if we simply didn't give them up so we



absolutely have to give up the locks in



order that other workstations can use



the system can use those same files and



directories but we have to do something



about the fact that the workstation



might have done some of the rights but



not all for its operations so frangipani



has like almost every other system that



needs to implement crashed recoverable



transactions users right ahead logging



this is something we've seen at least



one instance of the last lecture with



with aurora i was also using right ahead



logging so the idea is that if a



workstation needs to do a complex



operation that involves touching



updating many pieces of data in petal in



the file system the workstation well



first before it makes any rights to



petal append a la log entry to his log



in petal describing the full set of



operations it's about to do and only



when that log entry describing the full



set of operations is safely in petal



where now anybody else can see it only



then will the workstation start to send



the rights for the operation out to



petal I'm so we if it were station could



ever reveal even the one of its rights



for an operation the petal it must have



already put the log entry describing the



whole operation all of the updates must



already exist in petal so this is very



standard this is just a description of



right ahead logging



but there's a couple of odd aspects of



how frangipani implements right ahead



logging



the first one is that in most



transaction systems there's just one log



and all the transactions in the system



you know they're all sitting there in



one log in one place so there's a crash



and there's opera there's more than one



operation that affects the same piece of



data we have all of those operations for



that piece of data and everything else



right there in the single log sequence



and so we know for example which is the



most recent update to a given piece of



David but frangipani doesn't do that



this it has her work station logs as one



log per work station and there's



separate logs the other very interesting



thing about frangipane ease logging



system is that the LA workstation logs



are stored in petal and not on local



disk in almost every system that uses



logging the log is tightly associated



with whatever computer is running the



transactions that it's almost always



kept on a local disk but for extremely



good reasons



frangipani workstations



store their logs in petal in the shared



storage each workstation had its own



sort of semi-private log but it's stored



in petal storage where if the



workstation crashes its log can be



gotten that by other workstations so the



logs are in petal and this is this is



like separate logs for workstation



stored somewhere else in public sort of



shared storage so like a very



interesting and unusual arrangement all



right so we kind of need to know roughly



what's in the law what's in a log entry



and unfortunately the papers not super



explicit about the format of a log entry



but we can imagine that the well the



paper does say that each workstations



log sits in a known place a known range



of block numbers in petal and



furthermore that each workstation uses



its log space and petal on a kind of in



a circular way that it is all right log



entries along from the beginning and



when it hits the end the workstation



will go back and reuse its log space



back at the beginning of its log area



and of course that means that were



stations need to be able to you know



clean their logs so that sort of ensure



that a log entry isn't needed before



that space is reused and I'll talk about



that in a bit but each a log consists of



a sequence of log entries each log entry



has a log sequence number it's just an



increasing number each workstation



numbers it's log entries 1 2 3 4 5 and



the immediate reason for this may be the



only reason for this that the paper



mentions is that the the way that French



penny just detects the end of a work



stations log if the work station crashes



is by scanning for words in its log in



petal until it sees the increasing



sequence stop increasing and it knows



then that the log entry with the highest



log sequence number must be the very



last entry as it needs to be able to



detect the end of the log ok so we have



this log sequence number and then I



believe each log actually has an an



array



of descriptions of model aughh entry has



an array of the descriptions of the



modifications all the different



modifications that were involved in a



particular operation or an operation of



some a file system system call so each



entry in the array is going to have a



block number it's a block number in



petal there's a version number which



we'll get to in a bit and then there's



the data to be written and so there's a



bunch of these required to describe



operations that might touch more than



one piece of data in the file system one



thing to notice is that the log only



contains information about changes to



metadata that is to directories and



inodes and allocation bitmaps in the



file system the log doesn't actually



contain the data that is written to the



contents of files it doesn't contain the



user's data it just contains information



enough information to make the file



systems structures recoverable after a



crash so for example if I create a file



called F in a directory that's gonna



result in a new log entry that has two



little descriptions of modifications in



it one a description of how to



initialize the new files inode and in



another description of a new name to be



placed in the new files directory



alright so one thing I didn't mention so



of course the log is really a sequence



of these log entries



initially in order to be able to do



modifications as fast as possible



initially a friend Japanese workstations



log is only stored inside the



workstations own memory and won't be



written back to peddle until it has to



be and that's so that you know writing



anything including log entries to peddle



you know it takes a long time so we want



to avoid even writing log entries back



to peddle as well as writing dirty data



or modified blocks back to peddle we'd



like to avoid doing that as long as



possible so the real full story for what



happens when a workstation gets a revoke



message from the lock server seeing that



it has to give up a certain lock so on



right now this is the same you know this



is though compared sporto's



protocols revoke message if the



workstation gets a revoke message the



series of steps it must take is first



it's great that's the right any parts of



its log that are only in memory and



haven't yet been written to peddle it's



got to make sure as log is complete and



pedal as the first step so it writes



it's long and only then does it write



any updated blocks that are covered by



the lock that's being revoked so write



modified blocks



just for that provoked to lock and then



send a release message and the reason



for this sequencing and for this strict



ban is that these modifications if we



write them to peddle you know their



modifications to the data structure the



file system data structure and if we



were to crash midway through baby news



box just as usual we want to make sure



that some other workstation somebody



else there's enough information to be



able to complete the set of



modifications that the were station is



made even though the workstation has



crashed and maybe didn't finish doing



these rights and writing the log first



it's gonna be what allows us to



accomplish it these these log records



are a complete description of what these



modifications are going to be so first



we you know first we write though the



complete log to petal and then we



workstation can start writing its



modified blocks you know maybe it



crashes maybe doesn't hopefully not and



if it finishes writing as modified



blocks then it could send the release



back to the lock server so you know if



my workstation has modified a bunch of



files and then some other workstation



wants to read one of those files this is



the sequence that happens lock so ever



asked me for my locks right back my



workstation right back said log then



right back



writes the dirty modified blocks to



peddle and only then releases and then



the other workstation can acquire the



lock and read these blocks so that's



sort of the non crash you know if a



crash doesn't happen that is the



sequence of course it's only interesting



if a crash happens yes



[Music]



okay so for the log you're absolutely



right it writes the entire log and yeah



so so if if we get a revoke for a



particular file the workstation will



write its entire log and then only it's



only because it's only giving up the



lock for Z it it only needs to write



back data that's covered by Z so I have



to write the whole log just the data



that's covered by the lock that we



needed to give up and then we can



release that lock so yeah you know maybe



this writing the whole log might be



overkill like you if it turned out you



know so here's an optimization that you



might or might not care about if the



last modification for profile Z for the



lock were giving up is this one but



subsequent entries in my log didn't



modify that file then I could just write



just this prefix of my in-memory log



back to petal and you know be lazy about



writing the rest and that might see me



sometime



I might have to write the log back it's



actually not clear I would save us a lot



of time we have to write the log back at



some point anyway and yeah I think petal



just writes the whole thing okay okay so



now we can talk about what happens when



a workstation crashes while holding



locks right it's you know needs to



modify something rename a file create a



file whatever it's acquired all the



locks it needs it's modified some stuff



in its own cache to reflect these



operations maybe written some stuff back



to petal and then crashed men possibly



midway through writing so there's a



number of points at which it could crash



right because this is always the



sequence it always just always before



writing modified blocks from the cache



back



the frangipani will always have written



it's logged pedal first that means that



if a crash happens it's either while the



worst station is writing us log back to



pedal but before it's written any



modified file or directory' blocks back



or it crashes while it's writing these



modified block back but therefore



definitely after it's written in its



entire log and so that's a very



important you know but or maybe the



crash happened after it's completely



finished all of this so you know there's



only because of the sequencing there's



only a limited number of kind of



scenarios we made me worried about for



the crash okay so the workstations



crashed its crashed you know for like to



be exciting let's crash while Holdings



locks the first thing that happens the



lock server sends it a revoke request



and the lock server gets no response all



right that's what starts to trigger



anything where did nobody ever asked for



the lock



basically nobody's ever going to notice



that the workstation crashed so let's



assume somebody else wanted one of the



locks that the workstation had while it



was crashed and the lock service ended



revoke and it will never get a release



back from the workstation after a



certain amount of time has passed and it



turns out frangipani locks use leases



for a number of reasons so you know



after the least time has expired the



lock server will decide that the



workstation must have crashed and it



will initiate recovery and what that



really means is telling a different



workstation the lock server will tell



some other live workstation look



workstation one seems to have crashed



please go read it's log and replay all



of its recent operations to make sure



they're complete and tell me when you're



done and only then the lock servers



going to release the locks so okay so



and and this is the point at which it



was critical that the logs are in pedal



because some other workstation is going



to inspect the crash workstations log in



pedal



all right so what are the possibilities



one is that the worst that you can crash



before it ever wrote anything back and



so that means this other work station



doing recovery will look at the crash



workstation this log see that maybe



there's nothing in it at all and do



nothing and then release the locks the



workstation held now the worst that you



may have modified all kinds of things in



its cache but if it didn't write



anything to his log area then it



couldn't possibly have written any of



the blocks that have modified during



these operations right and so well we



will have lost the last few operations



that the workstation did the file system



is going to be consistent with the point



in time before that crashed workstation



started to modify anything because



apparently the workstation never even



got to the point where it was writing



log entries the next possibilities of



the workstation wrote some log entries



the log area and in that case the



recovering workstation will scan forward



from the beginning of log until it's



stopped seeing the log sequence numbers



increasing that's the point of where's



the log must Anton and the recovering



workstation we'll look at each of these



descriptions of a change and basically



play that change back into petal I'll



say oh you know there's certain block



number and petal needs to have some



certain data written to it which is just



the same modification that the crashed



workstation did in its own local cache



so the recovering workstation we'll just



consider each of these and replay each



of the crashed workstations log entries



back into petal and when it's done that



all the way to the end of a crashed



workstations log as it exists in petal



it'll tell the lock server and the lock



server will release the crashed



workstations locks and that will bring



the pedal up to date with some prefix of



the operations the crash workstation had



done before crashing maybe not all of



them because maybe it didn't write out



all of its log but the recovery were



season



won't replay anything in a log entry



unless it has the complete log entry in



petal and so you know implicitly that



means there's gonna be some sort of



checksum arrangement or something so the



recovery work station will know aha this



log entry is complete and not like



partially written that's quite important



because the whole point of this is to



make sure that only complete operations



are visible and petal and never never



never a partial operation so it's also



important that all the rights for a



given operation or a group together in



the log so that on recovery the recovery



workstation can do all of the rights for



an operation or none of them never half



of them ok so that's what happens if the



crash happens while the log is being



written back to petal a another



interesting possibility is that the



crash workstation crashed after writing



its log and also after writing some of



the blocks back itself and then crashed



and then skimming over some extremely



important details which I'll get to in a



moment then what will happen is again



the recovery workstation of course the



recovery where station doesn't know



really the point at which the



workstation crashed all it sees is oh



here's some log entries and again the



recovery workstation will replay the log



in the same way and more or less what's



going on is that yeah even if the



modifications were already done in petal



we're replaying the same modifications



here the recovery where students were



playing the same modifications it just



writes the same data the same place



again and presumably not really changing



the value for the writes that had



already been completed but if the crash



workstation hadn't done some of its



rights then some of these rights were



not sure which will actually change the



data to complete the operations all



right



that's not actually as it turns out the



full story and today's question sets up



a particular scenario for which a little



bit of added complexity is necessary in



particular the possibility that the



crashed workstation had actually gotten



through this entire sequence before



crashing and in fact released some of



its locks or so that it wasn't the last



person the last workstation to modify a



particular piece of data so an example



of this is what happens if we have some



workstation and it executes say a delete



file it deletes a file say a file F and



directory D and then there's some other



workstation which after this delete



creates a new file with the same name



but of course it's a different file now



so workstation 1 I'm sorry



workstation two later create create same



file same file name and then after that



workstation 1 crashes so we're going to



need you to do recovery on workstation



ones log and so at this point in time



you know maybe there's a third



workstation doing the recovery



so now workstation 3 is doing a recover



on workstation ones log so the sequence



says workstation 1 deleted a file or



station 2 created a file or station 3



does recovery well you know could be



that this delete is still in workstation



ones log so workstation two may you know



or station 1 crash just going to go or



station 3 is going to look at its log



that's going to replay all



all the updates in workstation ones log



this delete may the updates for this



delete the entry for this delete may



still be in workstation ones log so



unless we do something clever



workstation 3 is going to delete this



file you know because this this



operation erased the relevant entry from



the directory



thus actually erasing deleting this file



that's it's a different file that



workstation 2 created afterwards so



that's completely wrong alright what we



want you know the how come we want is



you know horse station one deleted a



file that file should be deleted but a



new file that if her name should not be



deleted just because it was a crash in a



restart cuz this create happen after



delete all right so we cannot just



replay workstation ones log without



further thought because it may it may



essentially a log entry in workstations



one log may be out of date by the time



it's we played during recovery some



other workstation may have modified the



same data and some other way



subsequently so we can't blindly replay



the log entries and so this is this is



this is today's question and the way



frangipani solves this is by associating



version numbers with every piece of data



in the file system as stored in pedal



and also associating the same version



number with every update that's



described in the log so every log entry



when well first I don't have any that's



you know say in pedal I'll just say in



pedal every piece of metadata every



inode every every piece of data that's



like the contents of a directory for



example every block of data metadata in



stored and pedal has a version number



when a workstation needs to modify a



piece of metadata in pedal it first



reads that metadata from pedal into its



memory and then looks at the existing



version of



and then when it's creating the log file



describing its modification it puts the



existing version number plus one into



the log entry and then when it in if it



does get a chance to write the data back



it'll write the data back with the new



increased version number so if over



station hasn't crashed and it did or if



it did manage to write some data back



before it crashed then the version



number has stored in petal for the



effected metadata it will be at least as



high or higher than the version numbers



stored in the log entry there will be



higher some other workstations



subsequently modified so what will



actually happen here is that the what



workstation 3 we'll see is that the log



entry for workstations one delete



operation will have a particular version



number stored in the log entry that



associated with the modification to the



directory let's say and the log entry



will say well the version number for the



directory and the new version number



created by this log entry is version



number three in order for workstation



two to subsequently change the directory



that is to add a file app in fact before



it crashed the workstation one must have



given up the lock in the directory and



that's probably why the log entry even



exists in pedal so workstation 1 must



have given up the lock apparently



workstation two got the lock and read



the current metadata for the directory



saw that the version number was three



now and when workstation two writes this



data it will set the version number or



the directory in peddle to be 4 ok so



the that means the log entry for this



delete operation is going to have



version number 3 in it now when the



recovery software on worst agent 3



replays workstation ones log it looks at



the version numbers first so it'll look



at the version number the log entry



it'll read the block from



look at the version number in the block



and if the version number in the block



in pedal is greater than or equal to the



version number in the log entry the



recovery software will simply ignore



that update in the log entry and not do



it because clearly the block had already



been written back by the crash



workstation and then maybe subsequently



modified by other workstations so the



replay is actually selectively based on



this version number that replay it's a



recovery only writes only



replays are right in the log if that



right is actually newer right in the log



entry is newer than the data that's



already stored in peddle so one sort of



irritating question here maybe is that



workstation three is running this



recovery software while other



workstations are still reading and



writing in the file system actively and



have locks and knows what to peddle so



the replay it's gonna go on while we're



station to which that doesn't know



anything about the recovery still active



and indeed workstation two may have the



lock for this directory



while recoveries going on so recovery



may be scanning the log and you no need



to read or write this directories data



in pedal while workstation two still has



the lock on this data the question is



how you know how do we sort this out



like one possibility which actually



turns out not to work is for the



recovery software to first acquire the



lock on anything that it needs to look



at in petal before while it's replaying



the log and the the you know one good



reason why that doesn't work is that it



could be that we're running recovery



after a system-wide power failure for



example in which all knowledge of who



had what locks is lost and therefore we



cannot write the recovery software to



sort of participate in the locking



protocol because



you know all knowledge of what's locked



my slot not locked may have been lost in



the power failure



um but luckily it turns out that the



recovery software can just go ahead and



read or write blocks in pedal without



worrying about sorry read or write data



in pedal without worrying at all about



locks and the reason is that if the



recovery software you know the recovery



software wants to replay this log entry



and possibly modify the data associated



with this directory it just goes ahead



and reads whatever's there for the



directory out of pedal right now and



there's really only two cases either the



crash workstation one had given up its



lock or it hadn't if it hadn't given up



this lock then nobody else can have a



directory locked and so there's no



problem if it had given up its lock then



before I gave it up its lock it must



have written that it's data for the



directory back to pedal and that means



that the version number stored in pedal



must be at least as high as the version



number in the crashed workstations log



entry and therefore when recovery



software compares the log entry version



number with the version number of the



data and pedal it'll see that the log



entry version number is not higher and



therefore won't we play the log entry so



yeah the recovery software will have



read the block without holding the lock



but it's not going to modify it because



if the locked was released the version



number will be high enough to show that



the log entry had already been sort of



processing to pedal before the crashed



workstation crashed no so there's no



locking issue alright this is the I've



gone over that kind of main guts of what



pedal is up to Nam it's cache coherence



it's distributed transactions and it's



distributed crash recovery the other



things to think about are the the paper



talks a bit about performance it's



actually very hard after over 20 years



to interpret performance numbers because



they brand their performance numbers on



very different Hardware in a very



different environment from



you see today roughly speaking the



performance numbers they show or that as



you add more and more friendship and



work stations the system basically



doesn't get slower that is each new



workstation even if it's actively doing



file system operations doesn't slow down



the existing workstation so in that



sense the system you know at least for



the application state look at the system



was giving them reasonable scalability



they could add more workstations without



slowing existing users down looking



backwards



although frangipani is full of like very



interesting techniques that are worth



remembering it didn't have too much



influence in on how on the evolution of



storage systems part of the reason is



that the environment for which is aimed



that is small workgroups



people sitting in front of workstations



on their desks and sharing files that



environment well it still exists in some



places isn't really where the action is



in distributed storage the action the



real action is moved into sort of big



data center or big websites big data



computations and there you know in that



world first of all the file system



interface just isn't very useful



compared to databases like people really



like transactions in the big website



world but they need them for very small



items of data the kind of data that you



would store in a database rather than



the kind of data that would you would



naturally store in a file system so you



know some of this technology might sort



of you can see echoes of it in modern



systems but it usually takes the form of



some database the other big kind of



storage this out there is storing big



files as needed for big data



computations like MapReduce and indeed



GFS is a you know to some extent looks



like a file system and is the kind of



storage system you want for MapReduce



but for GFS and for big data



computations frangipane ease you know



focus on local caching and workstations



and very close attention to



cache coherence and locking it's just



not very useful you know for both the



data read and write



typically caching is not useful at all



right if you're reading through ten



terabytes of data it's really



counterproductive almost to cache it so



a lot of the focus in frangipani is sort



of time is pass it by a little bit it's



still useful in some situations but it's



not what people are really thinking



about in designing new systems for all



right that is it
