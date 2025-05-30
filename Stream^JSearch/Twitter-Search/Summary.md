# Summary 



---

**what`s the fucntion we should support :**





we need search the status and the status just contains text.



- Let's assume Twitter has 800 million daily active users.
- On the average Twitter gets 400 million status updates every day.
- Average size of a status is 300 bytes.
- Let's assume there will be 500M searches every day.
- The search query will consist of multiple words combined with AND/OR.



**How Big the index id will be**



since we need build the invert index and key will be word and value will be id of the status,



where the status store and what`s the status id look like.



we have 400 M status post and each status contains 300, in the next 5 years



400 *300 * 365 *5 = 730 billion



5bytes id can represent 1 trillion id



id can be just 5 bytes





**How the index look like**



we need to store all the statues in a database, and also build an index that can keep track of which word appears in which status.





Index: What should our index look like? Since our status queries will consist of words, therefore, let's build our index that can tell us which word comes in which status.



So our index would be like a big distributed hash table, where 'key' would be the word, and 'value' will be a list of StatusIDs of all those status objects which contain that word.



**First i want to calcuate how big the index will be**



ask how to sort the result, need rank and base on what? if we need rank, we need additonal infromation for rank







let assume popular word is 500k and average length of the work is 5



500K * 5 => 2.5 MB



since we have around 730 billion status in next 5 years



and the id is 5 byts



292Billion * 5 => 1460 GB



we assume each status contains 15 words need to be index





store our index:



(1460 * 15) + 2.5MB ~= 21 TB





I am think store the index in the memeor y and





**How to shard index service?**

two option 1. sharding base on the word, problem...

2. sharding base on the object id







While querying for a particular word, we have to query all the servers (broadcast) , and each server will return a set of StatusIDs. A centralized server will aggregate these results to return them to the user.



**How to collect the data**



[How to collect the data for index service or type head](onenote:Basic.one#How%20to%20collect%20the%20data%20for%20index%20service%20or%20type%20head%20&section-id={86482390-C87C-1E49-9164-B76565805B41}&page-id={381C4E51-3A1D-CA4F-9D15-9AEADD2D0AD9}&end&base-path=https://d.docs.live.net/77339d157d673f41/Documents/9%20chapter/System%20Design%20and%20OO%20Design)





How to Process the data



[Map Reduce](onenote:Basic.one#Map%20Reduce&section-id={86482390-C87C-1E49-9164-B76565805B41}&page-id={9636D8B9-E801-AD4A-81F7-AF966EC240BE}&end&base-path=https://d.docs.live.net/77339d157d673f41/Documents/9%20chapter/System%20Design%20and%20OO%20Design)







it like a graph,



we can represent the entities such as user, page , place photo as the node and relationship as the edges such as friendship, check in, ownship as the edges





Both nodes and edges have metadata associated with them. For example, the node corresponding to me will have my name, my birthday, etc. and the node corresponding to the Page Breville will have its title and description as metadata.



each Nodes in the graph are identified by a unique number called the fbid.



Graph Search. Search others





**What`s the index look like:**



hash table



the key will be the for example:





Suppose David(10003)'s friend has fbid 1234 and lives in New York (111) and likes Soccer (222 ) . The



index corresponding to my friend will include the mappings:

friend:10003 → 1234

lives-in:111 → 1234

like:222 → 1234



we can see we have different type or differnt entity





we can keep the differnt type of the entiry in the different group of the service or vertiacal service since different type of entity has different requirement for the ranking







**How big the index will be**



if we want to build the index for friendship table

we have 1 billion user and every user has 50 friend



each index will be 51*5 = 255 bytes



and 255 * 1 billion = 255 G



**How to shard the**



it seems like store those index that is too large to fit into the memory of a single machine.

So we break up the index into multiple shards such that each shard can fit on

a single machine.





Shading base on the friend id









**first system will load user information to the browser cache first. like user frinds,**



**where user live in**



when user type as employers of my friends who live in New York as exmaple



the system will parase the query and identify such as



first it will get a friend list



find out who live in new york as a example in the live serach index service

and who work for facebook at same time



then find out is any intersection of those result












