# MIT-4-Primary-Backup Replication

Created: 2020-06-05 10:07:14 -0600

Modified: 2020-11-28 17:07:24 -0600

---

if the computer goes wrong, stop executing if anything goes wrong, if someone kick the power cable out of you service, unplug your service network connection or cut off the service network or hardware or software problem like cpu over heating and stop executing

or the software running in the service has bug and need reboot the computer



if those failure is independent



really these failures we can deal with replication





for us for your service will may well be





if there's an earthquake and the city where our datacenter is probably gonna take out, the whole data center you know we can have all the replication we like inside that data center it's not going to help us because the failure caused by an earthquake or a citywide power failure

or something the building burning down is like it's correlated failure between



our replicas if they're on that building so if we care about dealing with earthquakes then we need to put our replicas in maybe in just different cities at least physically separate enough that they have separate power unlikely to be affected by the same natural disaster



there's a couple of different approaches to replication 1[. one calls state transfer and another calls replicated state machine]{.mark}



the idea behind state transferor's that if we have replicas of a server the way you cause them to be to stay in sync ~~that is to be actual replicas~~ so that the backup can has everything it needs to take over if the primary fails.



In a state transfer scheme the way that works is that the primary [sends a copy of its entire state]{.mark}, that is for example the contents of its RAM to the backup and the backup just sort of stores the latest state ,so it's all there the primary fails in the backup can start executing with this last state it got. if the primary fails , the replica stored all the contents of the memory of the primary, so ~~maybe every once while the~~ primary would just you know make a big copy of its memory and send it across the network to the backup you can imagine if you wanted to be efficient. [you know maybe you would only send the parts of the memory that it's changed since the last time you sent in memory]{.mark}

[to the backup]{.mark}



replicated state machine schemes don't send the state between the replicas [instead they just send those external events and external input (]{.mark}operation from client ) ,~~they just send maybe from a primary to a backup again~~ just send things like arriving input from the outside world that the backup needs to know.



the observation is that you know if you have to two computers and they start from the same state and they see the same inputs that that in the same order

or at the same time , the two computers will continue to be replicas of each other, and sort of execute identically as long as they both see the same inputs at the same time







why people tend to favor a replicated state machine is that usually operations are

smaller than the state, but this you know the state of a server if it's a database

server might be the entire database, might be you know gigabytes whereas the

operations are just some clients sending and you know please read or write key, 27

operations are usually small, the states usually large ,so replicate a state



[machine usually looks attractive and slight downside is that the schemes tend to be quite a bit more complicated and rely on sort of more assumptions about how the computers operate]{.mark}





the state transfer is a really heavy-handed I'm just gonna send you my whole state sort of a nothing to worry about.







__________________________________________________________________________

detail :



the VMware scheme does that okay



interestingly enough though today's



paper is all about a replicated state



machine you may have noticed that



today's paper only deals with you know



processors and it's not that clear how



it could be extended to a multi-core and



a multi-core machine where the



interleavings of the instructions from



the two cores organ are



non-deterministic all right so we no



longer have this situation on a



multi-core machine where if we just let



the primary and backup execute they're



you know all else being equal they're



going to be the same because they won't



execute on multiple cores VMware has



since come out with a new possibly



completely different replication system



that does work on multi-core and the new



system appears to me to be using state



transfer instead of replicated state



machine because state transferred is



more robust in the face



multi-core and parallelism if you use



the machine and send the memory over you



know that the memory image is just that



just is the state of the machine and



sort of it doesn't matter that there was



parallelism whereas the replicated state



machine scheme really has a problem with



the parallelism you know on the other



hand I'm guessing that this new



multi-core scheme is more expensive okay



all right so if we want to build a



replicated state machine scheme we got a



number of questions to answer so we need



to decide at what level we're gonna



replicate state right so what state what



do we mean by state we have to worry



about how how closely synchronized the



primary and backup have to be right



because it's likely the primary will



execute a little bit ahead of the backup



after all it it's the primary that sees



the inputs so the backup almost



necessarily must lag over that gives



that means there's an opportunity if the



primary fails for the prime for the



backup not to be fully caught up having



the backup actually executes really in



lockstep with the primaries for



expensive because it requires a lot of



chitchat so a lot of designs a lot of



what people sweat about is how close the



synchronization is if the primary fails



or you know actually if the backup fails



to but it's more exciting if the primary



fails there has to be some scheme for



switching over and the clients have to



know oh gosh I instead of talking to the



old primary on server one I should now



be talking to the



the backup on server to all the clients



have to somehow figure this out the



switch over almost certainly it's almost



impossible maybe impossible to design a



cut over system in which no anomalies



are every are ever visible you know in



this sort of ideal world if the primary



feels we'd like nobody to ever notice



none of the clients to notice turns out



that's basically unattainable so there's



going to be a mama leaves during the cut



over and we've gotta figure out a way to



cope with them and finally if the one of



the two if one of our replicas fails we



really need to have a new replica right



if we have a two replicas and one fails



we're just living on borrowed time right



because the second replica may fail at



some point so we absolutely need to get



a new replica back online as fast as



possible so and that can be very



expensive the state is big you know you



know but the reason we like to replicate



a state machine was because we thought



state transfer would be expensive but



the two replicas in a replicated state



machine still need to have full state



right we just had a cheap way of keeping



them both in sync if we need to create a



new replica we actually have no choice



but state transfer to create the new



replicas the new replica needs to have a



complete copy of the state so it's going



to be expensive to create new replicas



and this is often people spending well



actually people spend a lot of time



worrying about all these questions and



you know we'll see them again as we look



at other replicated state machine



schemes so on the topic of what state to



replicate the today's paper has a very



interesting answer to this question it



replicates the full state of the machine



that is all of memory and all the



Machine registers it's like a very very



detailed replication scheme just no



difference at the even of the lowest



levels between the primary in the backup



that's quite rare for replication



schemes



almost always you see something that's



more like GFS where GFS absolutely did



not replicate you know they had



replication but it wasn't replicating



every single you know bit of memory



between the primaries and the backups



it was replicating much more application



level table of chunks



I had this abstraction of you know



chunks and chunk identifiers and that's



what it was replicating it wasn't



replicating sort of everything else



wasn't going to the expense of



replicating every single other thing



that machines we're doing okay as long



as they had the same sort of application



visible set of of chunks so most



replication schemes out there go the GFS



route in fact almost everything except



pretty much this paper and a few handful



of similar systems almost everything



uses application at some level



application level of replication because



it can be much more efficient because we



don't have to go to the we don't have to



go to the trouble of for example making



sure that interrupts occur at exactly



the same point in the execution of the



primary and backup GFS does not sweat



that at all but this paper has to do



because it replicates at such a low



level so most people build efficient



systems with applications specific



replication the consequence of that



though is that the replication has to be



built into the right into the



application right if you're getting a



feed of application level operations for



example you really need to have the



application participate in that because



some generic replication thing like



today's paper



doesn't really can't understand the



semantics of what needs to be replicated



so anyways so most teams are application



specific like GFS and every other paper



we're going to read on this topic



today's paper is unique in that it



replicates at the level of the machine



and therefore does not care what



software you run on it right it



replicates the low-level memory and



machine registers you can run any



software you like on it as long as it



runs on that kind of microprocessor



that's being represented this



replication scheme applies to the



software can be anything



and you know the downside is that it's



not that efficient necessarily the



upside is that you can take any existing



piece of software maybe you don't even



have source code for it or understand



how it works and you know do within some



limits you can just run it under this



under VMware this replication scheme and



it'll just work which is sort of magic



fault-tolerance wand for arbitrary



software all right now let me talk about



how this is VMware or Ft first of all



VMware is a virtual machine company



they're what their business is a lot of



their business is selling virtual



machine technology and what virtual



machines refer to is the idea of you



know you buy a single computer and



instead of booting an operating system



like Linux on the hardware you boot



we'll call a virtual machine monitor or



hypervisor on the hardware and the



hypervisor is job is actually to



simulate multiple multiple computers



multiple virtual computers on this piece



of hardware so the virtual machine



monitor may boot up you know one



instance of Linux may be multiple



instances of Linux may be a Windows



machine you can the virtual machine



monitor on this one computer can run a



bunch of different operating systems you



know each of these as is itself some



sort of operating system kernel and then



applications so this is the technology



they're starting with and you know the



reason for this is that if you know you



need to it just turns out there's many



many reasons why it's very convenient to



kind of interpose this level of



indirection between the hardware and the



operating systems and means that we can



buy one computer and run lots of



different operating systems on it we can



have each if we run lots and lots of



little services instead of having to



have lots and lots of computers one per



service you can just buy one computer



and run each service in the operate



system that it needs I'm using his



personal machines so this was their



starting point they already had this



stuff and a lot of sophisticated things



built around it at the start of



designing vmware ft so this is just



virtual machines um what the papers



doing is that it's gonna set up one



machine or they did requires two



physical machines because there's no



point in running the primary and backup



software in different virtual machines



on the same physical machine because



we're trying to guard against hardware



failures so you're gonna to at least you



know you have two machines running their



virtual machine monitors and the primary



it's going to run on one the backups and



the other so on one of these machines we



have a guest you know we only it might



be running a lot of virtual machines we



only care about one of them it's gonna



be running some guest operating system



and some sort of server application



maybe a database server MapReduce master



or something so I'll call this the



primary and there'll be a second machine



that you know runs the same virtual



machine monitor and an identical virtual



machine holding the backup so we have



the same whatever the operating system



is exactly the same and the virtual



machine is you know giving these guest



operating systems the primary and backup



a each range of memory and this memory



images will be identical or the goal is



to make them identical in the primary in



the backup we have two physical machines



each one of them running a virtual



machine guest with a its own copy of the



service we care about we're assuming



that there's a network connecting these



two machines and in addition on this



local area network in addition on this



network there's some set of clients



really they don't have to be clients



they're just maybe other computers that



our replicated service needs to talk



with some of them our clients



sending requests it turns out in this



paper there the replicated service



actually doesn't use a local disk and



instead assumes that there's some sort



of disk server that it talks to him



although it's a little bit hard to



realize this from the paper the scheme



actually does not really treat the de



server particularly especially it's just



another external source of packets and



place that the replicated state machine



may send packets do not very much



different from clients okay so the basic



scheme is that the we assume that these



two replicas the two virtual machines



primary and backup are our exact



replicas some client you know database



client who knows who has some client of



our replicated server sends a request to



the primary and that really takes the



form of a network packet that's what



we're talking about that generates an



interrupt



and this interrupts actually goes to the



virtual machine monitor at least in the



first instance the virtual machine



monitor sees a hot here's the input for



this replicated service and so the



virtual machine monitor does two things



one is it sort of simulates a network



packet arrival interrupt into the



primary guest operating system to



deliver it to the primary copy of the



application and in addition the virtual



machine monitor you know knows that this



is an input to a replicated virtual



machine and it's so it sends back out on



the network a copy of that packet to the



backup virtual machine monitor it's also



guessing and backup virtual machine



monitor knows a hot is a packet for this



particular replicated state machine and



it also fakes a sort of network packet



arrival interrupt at the backup and



delivers the packet so now both the



primary and the back have a copy this



packet they looks at the same input you



know with a lot of details are gonna



process it in the same way and stay



synchronized



course the service is probably going to



reply to the client on the primary the



service will generate a reply packet and



send it on the NIC that the virtual



machine monitor is emulating and then



the virtual machine monitor or will



we'll see that output packet on the



primary they'll actually send the reply



back out on the network to the client



because the backup is running exactly



the same sequence of instructions it



also generates a reply packet back to



the client and sends that reply packet



on its emulated NIC it's the virtual



machine monitor that's emulating that



network interface card and it says aha



you know the virtual machine monitor



says I know this was the backup only the



primary is allowed to generate output



and the virtual machine monitor drops



the reply packet so both of them see



inputs and only the primary generates



outputs as far as terminology goes the



paper calls this stream of input events



and other things other events we'll talk



about from the stream is called the



logging Channel it all goes over the



same network presumably but these events



the primary since the back of our called



log events on the log Channel



where the fault tolerance comes in is



that those the primary crashes what the



backup is going to see is that it stops



getting stuff on the stops getting log



entries a log entry stops getting log



entries on the logging channel and we



know it it turns out that the backup can



expect to get many per second because



one of the things that generates log



entries is periodic timer interrupts in



the in the primary each one of which



turns out every interrupt generates a



log entries into the backup these timer



interrupts are going to happen like 100



times a second so the backups can



certainly expect to see



a lot of chitchat on the logging Channel



if the primaries up if the primary



crashes then the virtual machine



monitored over here will say gosh you



know I haven't received anything on the



logging channel for like a second or



however long the primary must be dead or



or something and in that case when the



backup stop seeing log entries from the



primary the paper the way the paper



freezes it is that the back of goes live



and what that means is that it stops



waiting for these input events on the



logging Channel from the primary and



instead this virtual machine monitor



just lets this backup execute freely



without waiting for without being driven



by input events from the primary the vmm



does something to the network to cause



future client requests to go to the



backup instead of the primary and the



VMM here stops discarding the backup



personnel it's the primary not the



backup stops discarding output from this



virtual machine so now this or machine



directly gets the inputs and there's a



lot of produce output and now our backup



is taken over and similarly you know



that this is less interesting but has to



work correctly



if the backup fails a similar primary



has to use a similar process to abandon



the backup stop sending it events and



just sort of act much more like a single



non replicated server so either one of



them can go live if the other one



appears to be dead stops you know stops



generating network traffic



magic now it depends you know depends on



what the networking technology is I



think with the paper one possibility is



that this is sitting on Ethernet every



physical computer on the Internet or



really every NIC has a 48 bit unique ID



I'm making this up now the it could be



that in fact instead of each physical



computer having a unique ID each virtual



machine does and when the backup takes



over it essentially claims the primary's



Ethernet ID as its own and it starts



saying you know I'm the owner of that ID



and then other people on the ethernet



will start sending us packets that's my



interpretation the designers believed



they had identified all such sources and



for each one of them the primary does



whatever it is you know executes the



random number generator instruction or



takes an interrupt at some time the



backup does not and the back of virtual



machine monitor sort of detects any such



instruction and and intercepts that and



doesn't do it and he said the backup



wheats for an event on the logging



Channel saying this instruction number



you know the random number was whatever



it was on the primary



Edwige



yes yes



yeah the paper hints that they got Intel



to add features to the microprocessor to



support exactly this but they don't say



what it was okay



okay so on that topic the so far that



you know the story is sort of assumed



that as long as the backup to sees the



package from the clients it'll execute



in identically to the primary and that's



actually glossing over some huge and



important details so one problem is that



as a couple of people have mentioned



there are some things that are



non-deterministic now it's not the case



that every single thing that happens in



the computer is a deterministic function



of the contents of the memory of the



computer it is for a sort of straight



line code execution often but certainly



not always so worried about is things



that may happen that are not a strict



function of the current state that is



that might be different if we're not



careful on the primary and backup so



these are sort of non-deterministic



events that may happen so the designers



had to sit down and like figure out what



they all work and here are the ones



here's the kind of stuff they talked



about so one is inputs from external



sources like clients which arrive just



whenever they arrive right they're not



predictable there are no sense in which



the time at which a client request



arrives or its content is a



deterministic function of the services



state because it's not so these actually



this system is really dedicated to a



world in which services only talk over



the network and so the only really



basically the only form of input or



output in this system is supported by



this system seems to be network packets



coming and going so we didn't put



arrives at what that really means it's a



packet



arrives and what a packet really



consists of for us is the data in the



packet plus the interrupt



that's signaled that the packet had



arrived so that's quite important so



when a packet arrives



I'm ordinarily the NIC DMA is the packet



contents into memory and then raises an



interrupt which the operating system



feels and the interrupt happens at some



point in the instruction stream and so



both of those have to look identical on



the primary and backup or else we're



gonna have they're also executions gonna



diverge and so you know the real issue



is when the interrupt occurs exactly at



which instruction the interrupts happen



to occur and better be the same on the



primary in the backup otherwise their



execution is different and their states



are gonna diverge and so we care about



the content of the packet and the timing



of the interrupt and then as a couple of



people have mentioned there's a few



instructions that that behave



differently on different computers or



differently depending on something like



there's maybe a random number generator



instruction there's I get time-of-day



instructions that will yield different



answers have called at different times



and unique ID instructions another huge



source of non determinism which the



paper basically rules out is multi-core



parallelism is a unit process or only



system there's no multi-core in this



world the reason for this is that if it



allowed multi-core then then the service



would be running on multiple cores and



the instructions of the service the rest



of you know the different cores are



interleaved in some way which is not



predictable and so really if we run the



same code on the on the backup in the



server if it's parallel code running on



a multi-core the tubo interleave the



instructions in the two cores in



different ways the hardware will and



that can just cause



different results because you know



supposing the code and the two cores you



know they both asked for a lock on some



data well on the master you know



core one may get the lock before Core 2



on the slave just because of a tiny



timing difference core to may got the



lock first and the you know execution



results are totally different likely to



be totally different if different



threads get the lock



so multi-core is the grim source of



non-determinism man is just totally



outlawed in this papers world and indeed



like as far as I can tell the techniques



are not really applicable the service



can't use multi-core parallel



parallelism the hardware is almost



certainly multi-core parallel but that's



the hardware sitting underneath the



virtual machine monitor the machine that



the virtual machine monitor exposes to



one of the guest operating systems that



runs the primary backup that emulated



virtual machine is a unicorn it's a unit



processor machine in this paper and I'm



guessing there's not an easy way for



them to adapt this design to multi-core



virtual machines



okay so so these are really it's it's



it's these events that go over the



logging channel and so the format of a



log record a log log entry they don't



quite say but I'm guessing that there's



really three things in a log entry



there's the instruction number at which



the event occurred because if you're



delivering an interrupt or you know



input or whatever it better be delivered



at exactly the same place in the primary



backup so we need to know the



instruction number and by instruction



number I mean you know the number of



instructions since the Machine booted



why not the instruction address but like



oh or executing the four billion and



79th instructions since boot so log



entry is going to have instruction



number four an interrupt for input it's



going to be the instruction at which the



interrupt was delivered on the primary



and for a weird instruction like get at



time of day it's going to be the



instruction number of the instruction of



the get time of day or whatever



instruction that was executed on the



primary so that you know the backup



knows where to where to call this event



to occur okay so there's gonna be a type



you know network input whatever a weird



instruction and then there's I'm gonna



be data for a packet arrival it's gonna



be the packet data for one of these



weird instructions it's going to be the



result of the instruction when it was



executed on the primary so that the



backup virtual machine can sort of fake



the instruction and supply that same



result



okay so so as an example the both of



these operating systems guest operating



system assumes requires that the



hardware in this case emulated hardware



virtual machine has a timer that ticks



say a hundred times a second and causes



interrupts to the operating system and



that's how the operating system keeps



track of time it's by counting these



timer interrupts so the way that plays



out those timer notice why they have to



happen at exactly the same place in the



primary and backup otherwise they don't



execute the same no diverge so what



really happens is that the there's



there's a timer on the physical machine



that's running the Ft virtual machine



monitor and the timer on the physical



machine ticks and delivers an interrupt



a timer and up to the virtual machine



monitor on the primary the virtual



machine monitor at you know the



appropriate moment stops the execution



of the primary writes down the



instruction number that it was at you



know instruction since boot and then



delivers sort of fake simulates and



interrupts into the guest operating



system in the primary at that



instruction number saying oh you know



you're emulating the timer Hardware just



ticked



there's the interrupt and then the



primary virtual machine monitor sends



that instruction number which the



interrupt happened you know to the



backup the backup of course it's virtual



machine monitor is also taking timer



interrupts from its physical timer and



it's not giving them it's not giving



it's a real physical timer interrupts to



the to the backup operating system it's



just ignoring them when the law when the



log entry for the primaries timer



interrupts arrives here then the backup



virtual machine monitor will arrange



with the CPU and this requires special



CPU support to cause the physical



machine to interrupt at the same



instruction number



at the timer interrupts tapped into the



primary at that point the virtual



machine monitor gets control again from



the guest and then fakes the timer



interrupts into the backup operating



system now exact exactly the same



instruction number as it occurred on the



primary well yeah so the observation is



that this will this relies on the CPU



having some special hardware in it where



the vmm can tell the hardware CPU please



interrupt a thousand instructions from



now and then the vmm you know where so



that you know it'll interrupt at the



right instruction number the same



instruction as the primary did and then



the vmm just tells the cpu to start X



resume executing again in the backup and



exactly a thousand instructions later



the CPU will force an interrupt into the



virtual machine monitor and that that's



special hardware but it turns out it's



you know on all Intel chips so it's not



it's not that special anymore you know



15 years ago it was exotic now it's



totally normal and it turns out there's



a lot of other uses for it like um if



you want to do profiling you wanna do



CPU time profiling what you'd really



like or one way to do CPU time profiling



is to have the microprocessor interrupt



every thousand instructions right and



this is the hardware that's this



Hardware also this is the same hardware



that would cause the microprocessor to



generate an interrupt every thousand



instructions so it's a very natural sort



of gadget to want in your CPU



all right yes



what if the backup gets ahead of the



primary so you know we standing above



know that oh you know the primary is



about to take an interrupt at the



millionth instruction but the backup is



already you know executed the millionth



and first instruction so it's gonna be



if we let this happen it's gonna be too



late to deliver the interrupts if we let



the backup xu the head of the primary



it's going to be too late to deliver the



interrupts at the same point in the



primary instruction stream and the



backup of the instruction stream so we



cannot let that happen we cannot let the



backup get ahead of the primary in



execution and the way VMware aft does



that is that the the backup virtual



machine monitor it actually keeps a



buffer of waiting events that have



arrived from the primary and it will not



let to the backup execute unless there's



at least one event in that buffer and if



there's one event in that buffer then it



will know from the instruction number



the place at which it's got a force the



backup to stop executing so always



always the backup is executing with the



CPU being told exactly where the next



stopping point the next instruction



number of a stopping point is because



the backup only executes if it has a an



event here that tells it where to stop



next so that means it starts up after



the primary because the backup can't



even start executing until the primary



has generated the first event and that



event has arrived at the backup so the



backup sort of always one event



basically behind the at least one event



behind the primary and if it's slower



for some other whatever reason maybe



there's other stuff running on that



physical machine then the backup might



get you know multiple events behind at



the primary



alright there's a one little piece of



mess about arriving the specific case of



arriving packets ordinarily when a



packet arrives from a network interface



card if we weren't running a virtual



machine the network interface card would



DMA the packet content into the memory



of the computer that it's attached to



sort of as the data arrives from the



network interface card and that means



you know you should never write software



like this but it could be that the



operating system that's running on a



computer might actually see the data of



a packet as its DMA or copied from the



network interface card into memory right



you know this is and you know we don't



know what operating this system is



designed so that it can support any



operating system and cost maybe there is



an operating system that watches



arriving packets in memory as they're



copied into memory so we can't let that



happen because if the primary happens to



be playing that trick it's gonna see you



know if we allowed the network interface



card to directly DMA incoming packets



into the memory of the primary the



primary we don't have any control over



the exact timing of when the network



interface card copies data into memory



and so we're not going to know sort of



at what times the primary did or didn't



observe data from the packet arriving



and so what that means is that in fact



the NIC copies incoming packets into



private memory of the virtual machine



monitor and then the network interface



card interrupts the virtual machine



monitor and says oh a packet has arrived



at that point the virtual machine



monitor will suspend the primary and



remember what instruction number had



suspended at copy the entire packet into



the primaries memory while the primary



suspended and not looking at this copy



and then emulate a network interface



card interrupt into the primary



and then send the packet and the



instruction number to the backup the



backup will also suspend the backup rope



you know virtual machine monitor will



spend the backup at that instruction



number copy the entire packet and again



to the back-up is guaranteed not to be



watching the data arrive and then fakin



interrupts at the same instruction



numbers the primary and this is the



something the bounce buffer mechanism



explained in the paper okay yeah the the



only instructions and that result in



logging channel traffic or are weird



instructions which are rare no its



instructions that might yield a



different result if executed on the



primary and backup like instruction to



get the current time of day or current



processor number or ask how many



instructions have been executed or and



those actually turn out to be relatively



rare there's also one them to get random



tasks when some machines to ask or a



hardware generated random number for



cryptography or something and but those



are not everyday instructions most



instructions like add instructions



they're gonna get the same result on



primary and that go



yeah so the way those get replicated on



the back up is just by forwarding that's



exactly right each network packet just



it's packaged up and forwarded as it is



as a network packet and is interpreted



by the tcp/ip stack on both you know so



I'm expecting 99.99% of the logging



channel traffic to be incoming packets



and only a tiny fraction to be results



from special non-deterministic



instructions and so we can kind of guess



what the traffic load is likely to be



for for a server that serves clients



basically it's a copy of every client



packet and then we'll sort of know what



the logging channel how fast the logging



channel has to be all right so um so



it's worth talking a little bit about



how output works and in this system



really the only what output basically



means only is sending packets that



client send requests in as network



packets the response goes back out as



network packets and there's really no



other form of output as I mentioned the



you know both primary and backup compute



the output packet they want to send and



that sort of asks that simulated mix to



send the packet it's really sent on the



primary and simply discard it the output



packet discarded on the backup okay but



it turns out is a little more



complicated than that so supposing we're



what we're running is a some sort of



simple database server and the operation



the client operation that our database



server supports is increment and ideas



the client sends an increment requests



the database server increments the value



and sends back the new value so maybe on



the primary well let's say everything's



fine so far and the primary backup both



have value 10 in memory and that's the



current value at the counter and some



client on the local area network sends a



you know an increment request to



the primary that packet is you know



delivered to the primary it's you know



it's executed the primary server



software and the primary says oh you



know current values 10 I'm gonna change



to 11 and send a you know response



packet back to the client saying saying



11 as their reply the same request as I



mentioned gonna supposed to be sent to



the backup will also be processed here



it's going to change this 10 to 11 also



generate a reply and we'll throw it away



that's what's supposed to happen the



output however you also need to ask



yourself what happens if there's a



failure at an awkward time if you should



always in this class should always ask



yourself what's the most awkward time to



have a failure and what would happen you



to failure occurred then so suppose the



primary does indeed generate the reply



here back to the client but the client



the primary crashes just after sending



the report its reply to the client and



furthermore and much worse it turns out



that you know this is just a network it



doesn't guarantee to deliver packets



let's suppose this log entry on the



logging channel got dropped also when



the when the primary died so now the



state of play is the client received a



reply saying 11 but the backup did not



get the client request so its state is



still 10 no now the backup takes over



because it's seized the primary is dead



and this client or maybe some other



client sends an increment request a new



backup and now it's really processing



these requests and so the new backup



when it gets the next increment requests



you know it's now going to change its



state to 11 and generate a second 11



response maybe the same client maybe to



a different client which if the clients



compare notes or if it's the same client



it's just obviously cannot have happened



I didn't so you know because we have to



support unmodified software that does



not



damn that there's any funny business of



replication going on that means we do



not have the opportunity to you know you



can imagine the client could go you know



we could change the client to realize



something funny it happened with the



fault tolerance and do I don't know what



but we don't have that option here



because this whole system really only



makes sense if we're running unmodified



software so so this was a big this is a



disaster we can't have let this happen



does anybody remember from the paper how



they prevent this from happening the



output rule yeah so you want to do you



know yeah so the output rules is the



their solution to this problem and the



idea is that the client he's not allowed



to generate you know and generate any



output the primary's not allowed to



generate any output and what we're



talking about now is this output here



until the backup acknowledges that it



has received all log records up to this



point so the real sequence at the



primary then let's now undone crash the



primary go back to them starting at 10



the real sequence now when the output



rule is that the input arrives at the



time the input arrives that's when the



virtual machine monitor sends a copy of



the input to the backup so the the sort



of time at which this log message with



the input is sent is before strictly



before the primary generates the output



sort of obvious then after firing this



log entry off across a network and now



it's heading towards the backup but I'd



have been lost my not the virtual



machine monitor delivers a request to



the primary server software it generates



the output so now the



replicated you know the primary has



actually generated change the state 211



and generated an output packet that says



eleven but the virtual machine monitor



says oh wait a minute we're not allowed



to generate that output until all



previous log records have been



acknowledged by the backup so you know



this is the most recent previous log



message so this output is held by the



virtual machine monitor until the this



log entry containing the input packet



from the client is delivered to the



virtual machine monitor and buffered by



the virtual machine monitor but do not



necessarily execute it it may be just



waiting for the backup to get to that



point in the instruction stream and then



the virtual machine monitor here will



send an active packet back saying yes I



did get that input and when the



acknowledgment comes back only then will



the virtual machine monitor here release



the packet out onto the network and so



the idea is that if the client could



have seen the reply then necessarily the



backup must have seen the request and at



least buffered it and so we no longer



get this weird situation in which a



client can see a reply but then there's



a failure and a cut over and the replica



didn't know anything about that reply if



the you know there's also a situation



maybe this message was lost and if this



log entry was lost and then the primary



crashes well since it hadn't been



delivered so the backup hadn't sent the



act that means if the primary crashed



you know this log entry was brought in



the primary crashed it must have crashed



before the virtual machine monitor or at



least the output packet and prayer for



this client couldn't have gotten the



reply and so it's not in a position to



spot any irregularities they're already



happy with the output rule



brennon see I don't know they don't



paper doesn't mention how the virtual



machine monitor is implemented I mean



it's pretty low level stuff because you



know it's sitting there allocating



memory and figuring page tables and



talking to device drivers and



intercepting instructions and



understanding what instructions the



guest was executing so we're talking



about low-level stuff what language is



written in you know traditionally C or



C++ but I don't actually know okay this



of the primary has to delay at this



point waiting for the backup to say that



it's up to date this is a real



performance thorn in the side of just



about every replication scheme this sort



of synchronous wait where the we can't



let the primary get too far ahead of the



backup because if the primary failed



while it was ahead that would be the



backup lagging lagging behind clients



right so just about every replication



system has this problem that at some



point the primary has to stall waiting



for the backup and it's a real limit on



performance even if the machines are



like side-by-side and adjacent racks



it's still you know we're talking about



a half a millisecond or something to



send messages back and forth with a



primary stalled and if we wanna like



withstand earthquakes or citywide power



failures you know the primary in the



backup have to be in different cities



that's probably five milliseconds apart



every time we produce output if we



replicate in the two replicas in



different city every packet that it



produces this output has to first wait



the five milliseconds or whatever to



have the last log entry get to the



backup and how they need Osment come



back and then we can release a path



packet and you know for sort of low



intensity services that's not a problem



but if we're building a you know



database server that we would like to



you know that if it weren't for this



could process millions of requests per



second then



that's just unbelievably damaging for



performance and this is a big reason why



people you know you know if they



possibly can use a replication scheme



that's operating at a higher level and



kind of understands the semantics of



operations and so it doesn't have to



stall on every packet you know it could



stall on every high level operation or



even notice that well you know read-only



operations don't have to stall at all



it's only right so that just all or



something but you have to there has to



be an application level replication



scheme to to realize that you're



absolutely right so the observation is



that you don't have to stall the



execution of the primary you only have



to hold the output and so maybe that's



not as bad as it could be but



nevertheless it means that every you



know in a service that could otherwise



have responded in a couple of



microseconds to the client you know if



we have to first update the replicas in



the next city we turn to you know 10



micro second interaction into it 10



millisecond interactions possibly if you



have vast numbers of clients submitting



concurrent requests then you may may be



able to maintain high throughput even



with high latency but you have to be



lucky to or very clever designer to get



that



that's a great idea but if you log in



the memory of the primary that log will



disappear when the primary crashes or



that's usual semantics of a server



failing is that you lose everything



inside the box like the contents of



memory or you know if even if you didn't



if the failure is that somebody



unplugged the power cable accidentally



from the primary even if the primary



just has battery backed up RAM or I



don't know what you can't get at it



all right the backup can't get at it so



in fact this system does log the output



and the place it logs it is in the



memory of the backup and in order to



reliably log it there you have to



observe the output rule and wait for the



acknowledgment so it's entirely correct



idea just can't use the primary's memory



for it yes



say it again that's a clever idea I'd



and so the question is maybe input



should go to the primary but output



should come from the backup



I completely haven't thought this



through that might work that



I don't know that's interesting



yeah maybe I will



okay one possibility this does expose



though is that the situation you know



maybe the a primary crashes after its



output is released so the client does



receive the reply then the primary



crashes the backups input is still in



this event buffer in the virtual machine



monitor of the backup it hasn't been



delivered to the actual replicated



service when the backup goes live after



the crash of the primary the backup



first has to consume all of the sort of



log records that are lying around that



it hasn't consumed X it has to catch up



to the primary otherwise it won't take



over with the same state so before the



backup can go live it actually has to



consume all these entries the last entry



is presumably is the request from the



client so the backup will be live after



after it after the interrupt that



delivers the request from the client and



that means that the backup well you know



increment its counter to eleven and then



generate an output packet and since it's



live at this point it will generate the



output packet and the client will get to



eleven replies which is also if it if



that really happened would be anomalous



like possibly not something that could



happen if there was only one server the



good news is that almost certainly or



the almost certainly the client is



talking to this service using TCP and



that this is the request and the



response go back and forth on a TCP



Channel the when the backup takes over



the backup since the state is identical



to the primaries it knows all about that



TCP connection and whether all the



sequence numbers are and whatnot and



when it generates this packet it will



generate it with the same TCP sequence



number as an original packet and the TCP



stack on the client will say oh wait a



minute that's a duplicate packet



we'll discard the duplicate packet at



the TCP level and the user level



software will just never see this



duplicate and so this system really you



know you can view this as a kind of



accidental or clever trick but the fact



is for any replication system where



cutover can happen which is to say



pretty much any replication system it's



essentially impossible to design them in



a way that they are guaranteed not to



generate duplicate output basically you



know you well you can err on either side



I'm not even either not generate the



output at all which would be bad which



would be terrible or you can generate



the output twice on a cutover that's



basically no way to generate it



guaranteed generated only once everybody



errors on the side of possibly



generating duplicate output and that



means that at some level you know the



client side of all replication schemes



need some sort of duplicate detection



scheme here we get to use TCP s that we



didn't have TCP that would have to be



something else maybe application level



sequence numbers or I don't know what



and you'll see all of this and actually



you'll see versions of essentially



everything I've talked about like the



output rule for example in labs 2 &amp; 3



you'll design your own replicated state



machine yes



yes to the first part so the scenario is



the primary sends the reply and then



either the primary send the clothes



packet or the client closes the connect



the TCP connection after it receives the



primary's reply so now there's like no



connection on the client side but there



is a connection on the backup side and



so now the backup so the backup consumes



the very last log entry that as the



input is now live so we're not



responsible for replicating anything at



this point right because the backup now



live there's no other replica as the



primary died so there's no like if if we



don't if the backup fails to execute in



lockstep with the primary that's fine



actually because the primary is is dead



and we do not want to execute in



lockstep with it okay so the primer is



now not it's live it generates an output



on this TCP connection that isn't closed



yet from the backup point of view this



packet arrives with the client on a CCP



connection that doesn't exist anymore



from the clients point of view like no



big whoopee on the client right he's



just going to throw away the packet as



if nothing happened the application



won't no the client may send a reset



something like a TCP error or whatever



packet back to the backup and the backup



does something or other with it but it



doesn't matter because we're not



diverging from anything because there's



no primary to diverge from you can just



handle a stray we said however it likes



and what it'll in fact do is basically



ignore but there's no now the backup has



gone live there's just no we don't owe



anybody anything as far as replication



yeah



well you can bet since the backups



memory image is identical to the



primaries image that they're sending



packets with the very same source TCP



number and they're very same everything



they're sending bit for bit identical



packets you know at this level the



server's don't have IP addresses or for



our purposes the virtual machines you



know the primary in the back up virtual



machines have IP addresses but the the



physical computer and the vmm are



transparent to the network it's not



entirely true but it's basically the



case that the virtual machine monitor in



the physical machine don't really have



identity of their own on the network



because you can configure that then that



way instead these they're not you know



the virtual machine with a sewing



operating system in its own TCP stack it



doesn't IP address underneath there an



address and all this other stuff which



is identical between the primary in the



backup and when it sends a packet it



sends it with the virtual machines IP



address and Ethernet address and those



bits least in my mental model are just



simply passed through on to the local



area network it's exactly what we want



and so I think he doesn't generate



exactly the same packets that the



primary would have generated there's



maybe a little bit of trickery



you know what the we if this is these



are actually plugged into an Ethernet



switch into the physical machines maybe



it wasn't in two different ports of an



Ethernet switch and we'd like the



Ethernet switch to change its mind about



which of these two machines that



delivers packets with replicated



services Ethernet address and so there's



a little bit of funny business there for



the most part they're just generating



identical packets so let me just send



them out



okay so another little detail I've been



glossing over is that I've been assuming



that the primary just fails or the



backup just fails that is fail stop



right but that's not the only option



another very common situation that has



to be dealt with is if the two machines



are still up and running and executing



but there's something funny happen on



the network that causes them not to be



able to talk to each other but to still



be able to talk to some clients so if



that happened if the primary backup



couldn't talk to each other but they



could still talk to the clients they



would both think oh the other replicas



dead I better take over and go live and



so now we have two machines going live



with this service and now you know



they're no longer sending each other log



events or anything they're just



diverging maybe they're accepting



different client inputs and changes are



stayed in different ways so now we have



a split brain disaster if we let the



primary in the backup go live because it



was a network that has some kind of



failure instead of these machines and



the way that this paper solves it I mean



is by appealing to an outside authority



to make the decision about which of the



primary of the backup is allowed to be



live and so it they're you know it turns



out that their storage is actually not



on local disk this almost doesn't matter



but their storage is on some external



disk server and as well as being in this



server as a like totally separate



service there's nothing to do with disks



there this server happens to abort this



test and set test and set service over



the network where you you can send a



test and set request to it and there's



some flag it's keeping in memory and



it'll set the flag and return what the



old value was so both primary and backup



have to sort of acquire this test and



set flag it's a little bit like a lock



in order to go live they both may be



send test and set requests at the same



time to this test and set server the



first one gets back a reply that says oh



the flag used to be zero now it's one



this



second request to arrive the response



from the testing set server is Oh



actually the flag was already one when



your request arrived so so basically



you're not allowed to be primary and so



this this test and set server and we can



think of it as a single machine is the



arbitrator that decides which of the two



should go live if they both think the



other ones dead due to a network



partition any questions about this



mechanism you're busted yeah if the test



and set server should be dead at the



critical moment when and so actually



even if there's not a network partition



under all circumstances in which one or



the other of these wants to go live



because it thinks the others dead even



when the other one really is dead the



one that wants to collide still has to



acquire the test and set lock because



one of like the deep rules of 6:18 for



game is that you cannot tell whether or



another computer is dead or not all you



know is that you stopped receiving



packets from it and you don't know



whether it's because the other computer



is dead or because something has gone



wrong with the network between you and



the other computer so all the backup



ceases well I've stuck in packets maybe



the primary is dead maybe it's live



primary probably sees the same thing so



if there's a network partition they



certainly have to ask the TAT since that



server but since they don't know if it's



a network partition they have to ask the



testing set server regardless of whether



it's a partition or not so anytime



either wants to collide the test and set



server also has to be alive because they



always have to acquire this testing set



lock so the test and set server



sounds like a single point of failure



they were trying to build a replicated



fault tolerant whatever thing but in the



end you know we can't failover unless



unless this is alive so that's a bit of



a bummer



I'm guessing though I'm making a strong



guess that the test and set server is



actually itself a replicated service and



is fault tolerant right it's almost



certainly I mean these people are being



where they're like happy to sell you a



million dollar highly available storage



system that



uses enormous amounts of replication



internally um since the testing set



thing is on there dis server I'm I'm



guessing it's replicated too and the



stuff you'll be doing in lab 2 in lab 3



is more than powerful enough for you to



build your own fault-tolerant test and



set server so this problem can easily be



eliminated
