# 19 Bit coin 

Created: 2020-10-22 16:15:00 -0600

Modified: 2020-10-22 17:28:15 -0600

---

there to be a global published record all the transactions ,



we want a public log this is often also called a public ledger, okay so how

are we going to build ourselves a public ledger so that everybody agrees on all

the transactions that have already happened and further when they agree on

the order of the transactions because if



~~Y tries to send this coin to both Z and~~

~~Q at the same time you know we want the first one to win and the second one to~~

~~be ignored whatever Yellin which one want which~~



~~transaction was came first and which~~



~~came second and should be ignored okay~~



so how to make a ledger so here's a bad idea well a good idea actually have the most

simplest idea is to just pick somebody who everybody trusts and have that somebody maintain the ledger for you, every time you want to do transaction, you tell the trusted entity what the transaction is it just accumulates a log, and it's willing to give a copy of that

log to anyone so anyone can inspect it and see if coins already been spent and that actually is a good idea and if you could possibly do it you should



for various reasons the Bitcoin designers rejected this very obvious straightforward idea and the fundamental reason is that in a big system in a worldwide system, there's unlikely to be any single entity that everyone trusts and that is indeed trustworthy



and has no corrupt employees for example and in a global system you know

that means that we can't have the United States you know



government



I'm the trusted entity because there's



plenty of governments in the world to



don't necessarily trust the United



States similarly for any other



individual government so really for a



global system there's no it's it's easy



to argue against the idea of having a



single central trusted entity so that



leaves us way as well we want to run the



system make a system that's built out of



mutually untrusting participants where



we can survive malice by you know by



their participants okay so here's a



here's a bad possibility





let's just let anybody let's build a system on which anybody can join so it's gonna have

thousands maybe if computers will call them peers, they're scattered all over the Internet

and each one of them is going to be running the software to for our new cryptocurrency system, anytime somebody wants to have a new transaction, like Y wants to send a coin to Z, ~~Y faward their new transaction sends their new transaction to all the peers to send~~

~~them directly or another design which is actually~~ what Bitcoin uses is that Y sends a new transaction to a couple the peers and then the peers afford it sort of over individual TCP links to the entire rest of the system, so in every transaction ends up being sent to all

the peers and the peers what they're trying to do is each of them maintain a

complete copy of the log of all transactions and what we really want is f~~or all the peers used~~ all the honest peers for their transaction logs to be identical, they'll agree on which

transactions exist and just as important on the order of those transactions,



[so how can we arrange for all these peers to end up processing the heading transactions they're logs in the same order]{.mark}. ~~remember of course why may have~~



~~sent the transaction to Z you know to~~



~~these peers and at the same time sent~~



~~its transaction to Q to some other set~~



~~of peers and~~



[Why vote doesn't work]{.mark}

we want to make sure that despite that the peers end up with the same with identical logs even if Y is trying to treat well, I actually don't know how to design this ,one possibility

that you can imagine is that the peers would somehow talk to each other about each new transaction, and for each new slot in the log would vote on what transaction should fill that slot and have the majority you know since they may disagree legitimately, if they

received different transactions, we have a majority rule that says well we're gonna all the peers are gonna look at all the votes and the transaction that gets the most votes is the one that will go in the next slot in the log and then



they'll vote again on the next slide and you know maybe you could get this to work, you'd have to know who all the other peers are in order to know what a majority is



you don't have to know how many peers there are and you really want to make sure that you never count any peer more than once, but in a completely open system like Bitcoin actually we can't do either of those things we don't know how many participants there are in Bitcoin, and furthermore the number changes all the time as people peers join and leave the system ,so we don't know how many peers are so, we don't know what a majority would be furthermore, we don't have a way to reliably count votes, such that each participant only gets one vote, even assuming that was desirable



for example so we can't use IP addresses



in order to decide it's distinct votes



we can't say well each IP address gets



one vote or at most one vote because it



turns out to be extremely easy to either



forge I P addresses or by breaking into



people's computers to accumulate tens of



thousands of real computers that you can



control and you of course would you can



get them all to vote on your



and you can get them all to vote in the



system so an attacker could probably



accumulate a majority of votes



relatively easily in a sort of



straightforward design like this and if



an attacker can can get a majority of



the votes then it can use this majority



to sit to sort of say different things



conflicting things but with the majority



each time so if Z asks the system oh you



know with which which of the two



transactions came first cuz you know



that remember the problem were worried



about is that y is gonna double spend



some coin so it's gonna spend the same



coin - Q wants you to believe that and



it's gonna send that same Q's coin to Z



and 1c to believe that so maybe when Q



asks what's the next transaction to log



the majority controlled by the attacker



can say oh you know wise transfer to Q



is the very next one in the log and that



comes before this transaction and when Z



asks maybe the attackers majority will



say well you know the transfer Z comes



first and this other transaction to Q



comes second and that would mean the



attacker can trick q + Z into X into



accepting the same coin that's a double



spend and that's a disaster so without



some very clever idea it's very hard to



build an open system



you know without a controlled



memberships very hard to build an open



system in which you have reliable voting



okay but in fact Bitcoin doesn't quite



use voting but it nevertheless manages







to solve this problem of how to get agreement on a single ledger despite malice so this is the Bitcoin blockchain ~~and at this point there's a lot of diff~~



~~chain systems out there so actually I'm~~



~~not even sure what blockchain as a term~~



~~refers to but I'm only talking about~~



~~Bitcoin right now okay so~~ the goal is we want agreement on a single transaction log, again because we want to prevent double spending and this we're going to be building Bitcoin builds this thing called the blockchain that contains all the transactions, on all the coins so

it's a single blockchain that describes all the transactions in the system, again as with the previous system



there's going to be many peers ,so we still have this kind of overlay network appears each peer is kind of building a copy of the log and a complete copy of the transaction log in its own memory, and we don't know how many peers they are or how many there are who they are, but they form a sort of in one of these overlay networks spotted with TCP

connections and anytime a peer hears about a new transaction like when Y wants to submit a payment transaction to Z or Q ,it's gonna send it to one or more peers and that peers gonna flood the transaction over the whole system



the way that blockchain is built up is that each of the peers accumulates transactions and attach many transactions thousands into a block and then it's entire new blocks of transactions that are really appended to the ledger, again by flooding new blocks over the whole system, so that at least in theory every peer sees every new blocks, ~~so the blockchain model~~



blockchains consists of blocks what each block looks like is the hash of the

previous block, it's a bit like my previous transaction, broken transaction



system so we have the hash of the



previous block like cryptographic hash



of the previous block there's a set of



transactions so these are individual



spans from you know why is trying to pay



queue or whatever it happens to be a



couple hundred of whole thousand



individual transactions and each



transaction is actually much as I



described before it has the hash of the



previous transaction for that coin which



is going to exist in a previous block



typically it has deprived a signature by



the private key of the previous owner of



that coin and it is the public key of



the new owner so this transaction would



have that transfers money from why the



queue would contain Hugh's public key



and a signature by with wise private key



plus a hash of a previous transaction in



a previous block as well as these



transactions there's a nonce



which I'll talk about in a moment it's



just a 32-bit number and then the



current time roughly the way the system



works is that the peers accumulate new



transactions and roughly every 10



minutes one of them produces a new block



that should be the successor block



containing all the transactions that



have been sort of sent into the system



in the ten minutes roughly since the



previous block was created and if you're



if somebody tells you they're paying you



Bitcoin then before you accept that



they've really done it you need to watch



the blockchain as it evolves and you



know blocks new blocks are signs



everywhere so the blockchain is quite



public if you think somebody claims to



have paid you you need to watch the



blockchain until you see a new block



that contains the transaction that



you're expecting from the person that



claimed to sent you money and with your



public key at them that looks a valid



you know correctly signed okay so



everybody everybody has to watch the



blockchain for payments to them all



right so who creates each block this



action of creating a new block is called



mining and the technique that's used to



produce a new block is often also called



proof of work in the sense that it



requires a lot of CPU time to produce a



new block and so the production of a new



block essentially proves that you



control a real live CPU and you're not



just mining new blocks on a fake



computer the new block



in order to be valid a new block when



you hash it has to have a certain number



of zeros at the beginning of the hash of



the block now of course if you just take



a bunch of arbitrary transactions and



you do a cryptographic hash on it it's



highly unlikely that the hash of some



just whatever data is gonna have more



than one or two or three zeros at the



beginning of the cryptographic hash



however the computer that's mining the



block can put any value it likes here



for the knots and so what the mining



computers do is that they try different



by different random values for the nonce



just pick one with a random number



generator that'll stick it in there copy



of the block they're trying to mine and



then they'll compute the hash on the



block



and they'll check how many zeros how



many leading zeros are in the hash with



this particular knots if it's enough



leading zeros I don't know how many it



is but it's you know sort of on the



order of dozens if there's enough



leading zeros and it's a valid mine a



valid block and the whatever peer it was



that found this nonce that caused the



block hash to have lots of leading zeros



can flood this block over the network



and you know all that's being equal



that'll be the new next block and the



chain but typically the the hash of the



block with any given nonce you know



won't have enough leading zeros and the



mining the peer will have to try another



nonce another random nonce keep doing



that until it gets uh a block that



hashes to a hash with enough leading



zeros and so this takes a lot of CPU



time it takes oh you know it's a random



process but the system is sort of tuned



and the number of zeros that are



required to exist at the beginning of



the hash of the block is set so that it



takes about ten minutes you know with



all the different peers hundreds of



thousands appears out there who are



doing Bitcoin mining the average amount



of time before the first one of them



finds a nonce for a block that has



enough feeding zeros is set up to be ten



minutes



okay so question how do peers or new



peers discover other peers to



communicate with yeah it's a great



question so this is sort of a reference



to the this network of Bitcoin peers if



I'm a new peer you know I've install a



new computer I get Bitcoin software



installed on my my computer and I want



to join the Bitcoin network how do I



know who to talk to and how do I know



well so the straightforward answer to



that is that the Bitcoin software has



built into it on the IP addresses of a



whole bunch of current peers and so your



software when you first fire it up into



the binary the source whatever the



Bitcoin software you know has a whole



bunch of IP addresses and you try to



make TCP connections to those existing



peers and if all goes well you'll be



able to connect to them and you'll say



look I'm new please give me a copy of



the blockchain as it exists now and



they'll send you the current block team



which is about a couple hundred



gigabytes right now so that's if all



goes well if all doesn't go well then



you may run into problems like for



example if your copy of the software has



been modified by somebody malicious to



have a list of IP addresses that are all



controlled by the attacker or the



attacker controls your computer network



so that regardless of who you try to



connect to you end up actually talking



to the attackers machines it may be that



the attacker is running you know their



own isolated network and that you know



you think your newly installed software



thinks it's made a bunch of connections



the Bitcoin knows but whoops they're all



attackers nodes in that case a



blockchain you'll get will be watching



controlled by the attacker



and you may well you will be in trouble



and you know there's pick one of some



defenses against this may be the main



well he



loaded correct Bitcoin software the



correct Bitcoin software has built-in



hashes of recent blocks in the



blockchain and that means that if you



connect to some attackers in your



running proper Bitcoin software at least



the Bakhtin has to start with the first



however many thousand blocks in the box



beam have to be correct if you download



it absolutely wrong software modified by



the attacker then there's just nothing



Bitcoin can do to help you then this is



a potential weakness in the system I



haven't necessarily heard of anybody



exploiting this but it's definitely



something to think about



ok back to mining ok so the if you want



to create a new block you gotta find a



nonce that causes the new block you're



trying to produce causes hash to have



lots of leading zeros for an individual



machine you know the amount of time for



an individual ordinary computer to find



a nonce that satisfies this requirement



is like at least in a months of CPU time



but there's a very large number of



Bitcoin miners out there and so even



though any one of them would take a very



long time to find a new block or we



really care about is the time for the



very first one of them to find a block



and since they're all making these sort



of random choices of nonce one of them



is gonna find a a nonce that fulfills



the requirements relatively soon and the



number a Bitcoin adjusts the number of



leading required leading zeros in the



hash in response to how fast new blocks



seemed to be appearing and so if new



blocks are appearing much faster than



once every 10 minutes Bitcoin will



automatically increase the number of



leading zeros that's required and



that'll make it correspondingly harder



and take longer for the miners to find a



a new block Nana blocks are of course



arriving slower than every 10 minutes



over a sustained period of time then



Bitcoin will adjust the required number



of leading zeroes in the hash to be



smaller and that means it'll be easier



quicker



to find you blocks to the sort of an



adaptive scheme there to us blocks to



new boxes show up run once every ten



minutes roughly okay so this is the



proof-of-work scheme and this is



essentially a solution and all in a



funny way to the that voting problem I



mentioned a few minutes ago where you



can't really have some practice I have



majority votes because we're not sure



who the participants are or how many



there are because people may sort of



create fake computers fake IP addresses



whatever it will here you have to use



CPU time which is a sort of real



resource that cannot be faked in order



to contribute a new block and that means



that um you know while it's not really a



voting scheme the what it's essentially



doing is the new block gets to come from



a reactively random choice over the



different computers that are involved in



in mining so this scheme is it's a sort



of a random sort of cryptographically



reasonably strong random selection



process for who gets to choose the next



block and so if there's a small number



of attackers they're highly unlikely to



be selected by this process in order to



contribute the next block now what that



means is that if most of the



participants or most of the CPU power in



the system is controlled by non



malicious people then most of the new



blocks will be found by non malicious



people and that's not an immediate



solution to double spending but we'll



see that it that it's the key to the



double spending defense okay



all right so let's go back to our



example we have a blockchain that maybe



looks like we currently block five



o'clock fine has been published to



everybody the mmm them all the peers are



working on mining a potential block six



and we don't know what's going to be yet



because because the miners are still



working on finding about knots but you



know we know that it has a bunch of



transactions in it well if at this point



why broadcasts you know say it's payment



to Z well the the miner is already



working on this block so wise new



transaction and even if it sends it out



so now probably not going to be



incorporated in the block has been



currently mind but all the miners will



kind of keep this new transaction and a



buffer on the side and when one of them



does find a new block for block 6 then



wise transaction will be incorporated



into the attempts to mine block seven as



soon as somebody does mind box seven



this Y arrow Z will actually be really



in the blockchain alright so so one



question is could there be two different



successors to walk six could there be



sort of a block seven and a block seven



prime right what prevents this structure



from arising and of course the reason



why we're interested in this is that if



the structure could arise then



then these two maybe seven prime B seven



double prime then these two different b7



s two different successive successors to



b6 might have different transfers from Y



and so if you were aware of just b7



prime you'd think why paid it's Bitcoin



to see if you were we're only a b7 prime



prime you this would click a totally



legitimate payment from wide acute my



question is can there be two different



successors to a block it turns out the



answer is yes and it's actually does



happen reasonably frequently and the



reason is that there's you know



thousands of peers out there Mining away



trying to find the successors to block



six and they're likely mining it's you



know trying to produce somewhat



different blocks with different sets of



transactions in them so it's easy to



imagine a situation in which some of the



peers you know they happen to see just



because of the way the transactions move



through the network they happen to see



why it's transferred to Z first and



incorporated in to block their mining



and other peers you know for sort of the



same successors but the their version of



the block their mining as a successor to



six just happen to have seen this



transaction first and included that in



the block so we can easily get different



miners trying to work in a way trying to



produce different successors to be six



if two of them happen to find a nonce



that satisfies the leading zeros in the



hash rule at the same time that means we



have two different blocks to tumor



totally valid blocks produced at the



same time and they're both going to send



those blocks out into the network and



they'll be seeing that you know on



roughly the same time by all the other



peers so it could easily be the case



that two different two quite different



successors to block six may arise and



it's called the fork



and so we're very interested in what



happens to Forks because this can and



does arise well the most immediate rule



is that as soon as any peer sees a



successor oh you know all the peers are



trying to mine a successor to block six



as soon as than ever any of them any of



them sees a new successor block be



flooded from some peer that did



successfully mine



it'll stop working on six and



immediately switch to trying to work on



a successor from b737 so initially every



peer as soon as it sees a successor



block switches to mining a successor for



that successor block so in this



situation some of the peers will see b7



Prime first and start working on a



successor to that then other peers will



start mine will see b7 prime prime for



it's just depending on you know what



they happen to see first if the if these



two are mined at the same time and



they'll start working on a successor to



b7 prime prime so now we got some of the



peers working on sort of extending this



fork in the blockchain and the other



peers working on extending this fork in



the blockchain however another critical



rule is that if a if somebody's mining



away on this trying to make one of these



blocks and it sees a new block for a



different fork that's longer then



anybody working on extending this fork



will switch to extending this longer



fork so that's a rule in the software



that everybody is supposed to favor the



longest chain so at least initially if



we have some of the peers mining a way



on one fork and the other and there's



the same length and others mining on the



other fork it turns out the variance in



how long it takes to produce a valid



knots is pretty high



so it's even if there's equal number of



peers mining both Forks it's highly



likely that one of them will find a



successor significantly before the other



and so this successor will be flooded to



a bunch of nodes appears that were



working on this successor and they'll



all switch to the longer Fork and so



that means that there's sort of an



asymmetry here that causes us a slight



you know with if this Fork itza gets



extended by the minor slightly before



this fork then that'll contract miners



over onto this fork it'll be more and



more miners mining on this Fork and so



the new blocks will be found more and



more rapidly on the winning fork so



there's a tendency sort of reinforced



success that's the longer fork gets



longer and pretty soon once all the



miners have heard about this longer fork



nobody will be left the mining on this



fork everybody will ignore it and



everybody will only treat this longest



fork as the real as the real chain okay



so this is it's highly likely that one



of the there's a fork that one of the



two Forks will see an x-block first will



be longer everybody all the peers will



switch to mining on it and that



everybody will rapidly agree that one of



the other is the longest fork of course



the transactions on the abandoned fork



you know usually most of the trend



usually these two competing blogs have



you know pretty much pretty similar set



of transactions but there may well be



transact a few transactions here that



were not there and certainly if



somebody's trying that will spend or but



if there are transactions in the



abandoned fork that didn't happen also



to be in the winning fork then these



transactions they just go away there's



no attempt in the sort of blockchain



system itself to try to carry over these



transactions now with it or there's no



attempt to kind of directly merge the



two forks now in fact you know if you



don't see your transaction show up you



maybe issue it



and you know because the blockchain is



public it'll become apparent that your



transaction needs to be reassured



because it wasn't incorporated more in



the winning fork however it is also the



case that for a brief period of time



both of these transactions were in the



blockchain right so for a brief period



of time there was a double spending of



wise coin in the blockchain and that you



know that's like a little bit of a



dangerous situation in fact you know



it's an extremely dangerous situation



right since the whole point was to avoid



block chains right until one of these



two chains got longer now it was totally



unclear which of these two chains to



believe and then these some of the peers



may only know about one of them or the



other of them so this raises a sort of



unhappy question about how Q or Z you



know what procedure should they use to



be sure that they've actually been paid



right apparently it's not enough for Z



to say well as soon as the transaction



appears in the blockchain then I'm sure



I didn't paint because that's not true



right maybe the maybe this bar chain



would end up being a shorter one and the



wipies Q blockchain will win similarly P



you can't just look at oh you know my



transaction showed up in this block and



the blockchain therefore it's a valid



transaction because it may end up being



abandoned due to being on a shorter fork



and so this is the reason for the rule



in that people who care don't really



believe in transactions until there's a



couple of blocks after them in the



blockchain and as a as the longer chain



gets longer and longer or as what you



think is the longer chain gets longer



and longer the chances that there might



be some other chain that will become



longer in it longer than it could



vanishingly small because if you're on a



slightly longer chain that's going to



attract miners to mining on it so no



other chain can grow very rapidly and of



course the you know the rate in which a



chain a particular fork can grow is



proportional to the number of peers that



are mining on that chain



all right so this is this is the



mechanism that prevents with it makes it



so that if Y sends out two conflicting



transaction that's at the same time even



though there can be a brief double spend



if there's a fork it will rapidly be



only one or the other of the two



transactions will be in the longest



chain and so one of them will win in the



sort of official chain now and you know



indeed if if this second transaction



shows up is sent to peers later on after



the Y transfer disease in the chain then



all the peers will ignore trend newly



arriving transactions that for coins



that have already been spent in a



transaction on the chain on the fork



that they're mining for so why can't you



know send out this transaction again



after the first transactions shows up in



the chain in the blockchain okay okay so



you know there's some other attacks you



might wonder about one question is



whether you know let's suppose that this



is the this is the chain if Y is



colluding with some peers and this is



the official b7 and we have a b8 etc you



know



supposing why isn't League with some of



the peers could appear take this block



seven it's now you know in the middle of



the chain and change it to produce just



a different block that doesn't have this



transaction in it and just sort of



substitute this new block for the old



block seven and sort of pretend that



block e refers to it and now block this



new block seven doesn't have a



transaction and so that sort of very



straightforward changing of a single



block doesn't work and the reason is



that these arrows here are really really



means that there's a cryptographic hash



and blog ate that



is the hash of the block seven it refers



to and so this hashing blockade you know



for for a block seven that already



exists this hashman block eight is a



hash of the original block seven if



someone changes this content it's gonna



have a different hash and so this



blockade hash if you try to pawn off



this modified block on somebody who



knows about block eight they're gonna



say we didn't have our keys you know



hash doesn't hash to the same no block



this block seven prime doesn't hash to



the same value that's embedded in Block



E so you can't trick anybody who knows



about subsequent blocks into accepting a



modified intermediate block alright



question



I see why is a Q store by his coffee it



shows up in one of the blocks



oh I see ok so this is a let me just



back up a little bit so I think the



scenario we have is that you know there



was a blockchain and then a brief fork



and in that brief fork why pay the same



coin to two different two different



parties and let's say this is block 7



prime prime and it's block 7 that wins



and is on the main chain and block 7



prime prime is it's just forgotten and



ignored and the question is jeez you



know for briefly at least Q saw this



transaction show up in the chain and



gave the cup of coffee to why and then



why I left the store but then you know



this part of the chain is discarded and



Q is left with no money they've given



away some coffee but they did not get



paid and that just is what happens in



this scenario all right if Q was willing



to handle with a cup of coffee after



seeing the transaction in just the last



block in the blockchain then they'd risk



this scenario and there's nothing they



can do about it and they can't get the



money back I mean unless you run down



the street catch up with the person and



take the cup of coffee away and that is



the reason why for high-value



transactions your Starbucks trolley



doesn't care very much right that cup of



coffee really only cost like you know 50



cents to make like if they occasionally



you know these Forks don't happen that



often they occasionally lose a cup of



coffee well they can probably willing to



deal with that but if if why was buying



a car from Q for you know $20,000



Bitcoin thank you probably would rather



not let Y walk off with this level of



assurance that being paid and that's the



reason why if you care you'll wait until



multiple blocks show up after the block



in which in which your transaction was



in so Z won't actually him if it's a



high-value transactions he won't hand



over the goods until there's at least



some number five six blocks showing up



after the block



Action Shooting open and it's very



unlikely that a a fork could be extended



at five or six times like over a period



of an hour now ken block's show up only



every ten minutes and then turn out to



be the shortest not the longest chain



because that means that there was some



other fork that was extending faster and



the only way some other before could



extend faster is that if a majority of



the cpu power was working on it and



we're assuming that a majority the cpu



power is islam malicious and is



therefore switching to the current



longest chain all right so this is you



have to be if you're doing large value



transactions you have to be careful and



wait till a chain goes long and after



your transaction shows up okay so okay



so I explained why you can't simply



modify a block in the middle of the



chain there's a related question which



is supposing there's an existing



blockchain you know that's something



that long and your transaction Y arrow



transaction why does he shows up here in



the blockchain and you want to get rid



you want to hide that transaction now



somehow make it so it doesn't exist well



gosh why don't you produce a new sort of



alternate chain that you know is mostly



identical to the main the real chain but



it's longer and just happens to omit Y



is transferred to Z and instead includes



Y is transferred to Q and if you do the



mining correctly for this and the hashes



work out your chains longer and it just



will be accepted under the rules of



Bitcoin which which everybody supposed



to switch to the longest chain so how



come you can't do this



and this you know this would also allow



you to double spend by essentially



unspent in a previous spent quantity and



my earlier comment about oh you're



supposed to wait you know zis was to



wait until the blockchain gets extended



you know this is now a way to defeat Z



waiting for the blockchain to extend it



so we're really because serious trouble



if you could make this work okay so and



somewhat well the answer is yes this can



be made to work and here's how to do it



you know the main blockchain is being



extended by the non malicious



participants at some rate right they



have enough CPU power to you know



produce a new block every 10 minutes if



you're the attacker and you have more



CPU power than the entire non-malicious



set appears then you're going to be able



to generate walks faster than the real



chain so your block makes your you know



chain may start out shorter and I may



take you a while to generate each block



but you know maybe you can generate two



blocks every ten minutes whereas the



main chain is only capable of generating



one block every ten minutes so that



means that for a while you'll have



caught up and exceeded the length of the



main chain the main fork and by the



rules of Bitcoin everyone you



non-malicious totally correct Bitcoin



peers they'll all switch to your longer



chain and that means they'll all



effectively forget this transaction and



except this other transaction this



second spend of the same coin so if



you're an attacker and you have more CPU



power than the entire rest of the



network



you can produce this chain and it means



you can double spend and you know that's



certainly you know something to think



about but the reason why you might hope



more be sort of somewhat confident that



it couldn't arise is that in a big



system with lots of participants it



might be very hard to assemble more CPU



power than the entire rest of the system



so once the buns Bitcoin grew big you



know people were somewhat confident that



the main sort of non-malicious system



had enough cpu power that it would be



expensive for an attacker to assemble



more CPU power than the rest of the



system of course for new



cryptocurrencies that don't yet have



very large mining operations going



they're actually easy to shoot down it's



easy for a new cryptocurrency like



Bitcoin it's easy for an attacker you



know for whatever reason to put it out



of business by getting more CPU power



but for a big system like Bitcoin it's



somewhat difficult now that said the



people who've looked into tried to



figure out who controls mining CPU power



and Bitcoin suspect that the biggest



players have fractions of the total that



are not that far from 50% and that



certainly if you know two or three of



the largest mining operations combined



forces that they would have a majority



of the mining power in Bitcoin and could



produce alternate Forks like this so



that's a somewhat troubling development



you know whether there'd be motivated to



do something bad especially since sort



of everything that's done in Bitcoin is



public that's what people would really



notice that oh gosh that was a long



chain and then we switch to a chain that



started way far back boy would people



ever notice that and that would you know



destroy confidence in Bitcoin and may



undermine anything that the malicious



parties were hoping to achieve



so since you know it is very expensive



actually you know the big players in



mining in Bitcoin I spent a huge amount



of money to buy the mining hardware that



they owned and so they probably wouldn't



want to undermine people's trust in



Bitcoin because that would destroy the



value of their vast collections partner



all right any questions about the



machinery here all right so I



questions that I can ask an answer one



question is that the ten minutes between



blocks is actually a serious annoyance



it means that if I want to buy something



it takes up to 10 minutes before the



transaction shows up in the blockchain



at all



even even in the first block and you



know either I have to wait around for 10



minutes to get my cup of coffee or the



store owner has to give me my cup of



coffee before the trans before the



transactions in the blockchain at all



thus having to trust me so why can't we



make the 10 minutes much shorter and you



know actually the 10 minutes probably



could be made shorter the practical



reasons why it isn't shorter is that it



actually it takes a while for new box to



be flooded over the system right after a



miner finds the next block it has to be



sent thousands of peers Bitcoin over



possibly slow network connections and it



may take quite a while before that block



is known to all the other peers and that



means that there's some period of time



in which other peers are mining on



blocks or wasting their time mining



blocks that are I've been superseded by



a block that hasn't yet beached them and



basically the fraction of time you spend



mining wasting of trying mining blocks



that have been superseded is related to



how long it takes to mine each block



compared to how long it takes to flood



the block and so if you make the inter



block interval shorter and shorter then



it starts to it gets small enough that



it approaches the amount of time it



takes the flood new blocks and that



would cause most piers to waste most of



their mining effort and since the miners



are actually making money making Bitcoin



by mining because there's a little



reward to the successful miner of each



block the miners are very uninterested



in wasting resources mining for blocks



that are have been superseded and so



they're very uninterested in



this 10-minutes be much shorter than it



it's now and you know that's a



significant constraint so there's a



question what prevents why from double



spending much in a much later block when



piers might have forgotten about the



first transaction and so you know the



question is oh you know in a very early



block why transferred according to Z and



then there are thousand blocks later Y



tries to transfer the same coin to Q you



know like a year later or something and



the answer to how this plays out is that



all the peers remember this forever



they absolutely remember every unspent



transaction forever and that means that



actually that can't be the first



I think nominally to tell you the truth



I don't understand all the ins and outs



of this but one way the most



straightforward way to solve this



problem is for all peers to remember



every transaction forever and every



incoming transaction they check to make



sure that the coin hasn't been spent yet



they just the course create a database



or index or something but allows them to



essentially check every record to see if



this coin has already been spent and I



think you can although I don't fully



understand this I think peers can



discard a lot of though a lot of this



information by only remembering



information about unspent transactions



so they keep a database of unspent



transactions but it doesn't include



spent coins and if a new transactions



coin isn't in the database of unspent



transactions then it's just ignored but



this you know this database has to be



every pair has to keep this database



forever so you know just of course this



is a in a way a very expensive system



because what we're talking about is you



know maintaining a record of every



transaction essentially forever and you



know if you think about how many



transactions there are per second or per



year on earth



it's a vast number and so people really



were serious about using Bitcoin they



used it for everything in the way they



use cash now it would you know it would



be an enormous system and there would be



enormous performance streams on the



system and indeed Bitcoin is not really



capable of supporting every transaction



you couldn't run the entire financial



system of the world on Bitcoin as it



exists today and there's a bunch of



there's a bunch of limits you know one



limit is that the full Bitcoin database



already consumes a couple hundred



gigabytes yeah that's actually not so



bad because you can fit it on a disk but



if it was a thousand times larger it



would start to be a serious problem to



even store it let alone search for stuff



in it the most immediate problem



and we it turns out that processing that



transactions it's not terribly expensive



because for the peers it's mostly about



hashing and these cryptographic hashes



are pretty quick but the sort of most



current you know ugly restriction is



that these block cuz there's a limit to



how big these blocks can be these blocks



can only be a couple megabytes in size



and new blocks appear only every ten



minutes and so that means you only get



you know less than a megabyte of new



transactions per minute each transaction



the sort of Val you know is very way



various ways of abbreviating them but



you know each transaction is at least



dozens or a hundred bytes and that means



that the system can really only because



of this block size limit and the ten



minute limit the system can only process



sort of thousands or tens of thousands



of transactions well I'm not sure I can



divide properly but it's not nearly



enough to run the current way that



Bitcoin set up is not nearly



high-capacity enough to run the world



all the world's financial transactions



are and so you know people change it it



evolves but it's not really fast enough



for everything of course nobody's really



using it for commerce it's mostly used



for speculation as far as anyone can



tell so it's not yet a problem but from



a design point that there needs to be



some things fixed okay so I mentioned



before that the Bitcoin software adjusts



the hardness of finding nonces that is



the number of required leading zeros in



the block hash adjusts that dynamically



to cause there to be ten minutes for



block one thing that has to be the case



though is that all the participants have



to agree on the required number of



leading zeros they actually all have to



agree on the hardness of finding a knots



and so if one peer sort of looks at the



rate at which blocks have been produced



and



besides that it's too slow and it should



make the require fewer leading zeros but



the other peers haven't made the same



decision then that first piers will be



generating blocks that are rejected by



the other peers because all the peers



demand what they think is the correct



number of leading zeros in the hash so



there has to be agreement on on the



hardness of finding a nots



that the the peers have to agree exactly



and what the current hardness is



otherwise they'll reject each other's



blocks so how do they reach that



agreement it turns out actually to be



totally straightforward they all are



looking at the same blockchain after all



that was the whole point is that you



know except for temporary Forks there's



just one blockchain everybody hasn't a



copy of the exact same bits in the



blockchain and so the Bitcoin just



defines a deterministic function that



takes the current blockchain as its



argument and uses that to



deterministically produce the current



hardness of finding a nonce and the way



it does that is basically it looks at



the timestamps in the blocks to decide



how fast the recent blocks have been



produced but since everybody's looking



at the same blocks in the same time



stamps and is running the same function



to adjust the hardness they all come to



exactly the same conclusion about what



the hardness ought to be for for each



successive block in the blockchain so



there's a kind of interesting agreement



that's being enforced there because they



all see the identical same laws all



right



another interesting question is that one



of the motivations that some people have



for being interested in new



cryptocurrencies is that they might be



more anonymous than credit cards and



indeed credit cards are deeply



non-anonymous since the credit card



company knows exactly what you're up to



I keeps a record of it whereas Bitcoin



at least on the face of it you know



Bitcoin there was nothing about a



Bitcoin transaction that say had my name



on it



now you might think well each Bitcoin



transaction has my public key in it



man that's true if I don't change my



public key and I always use the same



public key then once somebody figures



out my public key is just relatively



easy since whenever I pay somebody they



they get to know my public key then



people can track my activities by



looking for my public key or my



signature in the Bitcoin lock and it's a



public log so anybody can look now



people everybody who cares and I think



most Bitcoin wallets offer actually



generates fresh public keys for each



transaction I'm said anytime if somebody



wants to pay me money my wallet will



generate a new never used before public



private key pair



remember the private key then give the



public key to the person who wants to



send me money and that makes the



tracking harder but it turns out that if



you're up against determined sleuth



there's you know there's enough clues if



you make transactions often enough since



the transactions are often tied to your



real identity like if you buy something



from Amazon with Bitcoin yeah maybe the



Bitcoin transaction it's not clear it's



you but it probably needs to be shipped



to you by FedEx to your home address and



that's a little piece of identifying



information there and that will allow



somebody to figure out it was you who



spent that money and they'll be able to



straight track backwards to see where



that money came from to get another clue



about who you are and what you're up to



so in fact against amateurs Bitcoin is



reasonably anonymous against serious



adversaries a Bitcoin has turned out not



to be particularly anonymous okay



a little bit disappointing from a little



bit disappointing for people who are



interested in privacy or doing drug



deals or financing illegal activity



alright



sum up the sort of key idea here is the



blockchain like a public ledger that



everybody agrees on and that has every



transaction on it and all that has a lot



of problems like with scalability if you



can make it work it's a great idea



another sort of key technical problem is



how to do this without any



centralization now whether the



centralization or decentralization of



valuable property is kind of not really



a technical question but if you value it



then it's just really cool and amazing



but it's possible to have agreement on a



single log with no central trust and I'm



using participants many of whom are



actively malicious and the final key



idea is that is the idea of mining a



proof-of-work where it too has problems



but it's very surprising that a



technique existed at all that allowed



agreement in a way that can't be fooled



by these sort of fake IP address attacks



that doesn't suffer the same problems



that voting suffers that was a very



surprising and interesting development



all right that's all I have to say the



actually sent kind of continuing some of



this line of thought and the next



lecture which is a sort of different



kind of decentralized system partially



built on top of Bitcoin
