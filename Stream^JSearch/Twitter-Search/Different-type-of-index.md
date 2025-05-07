# Different type of index 

Created: 2017-10-13 23:33:50 -0600

Modified: 2021-01-23 01:25:19 -0600

---

The entities are the nodes and the relationships are the edges. The nodes would be the nouns and the edges would be the verbs. Every user, page, place, photo, post, etc. are nodes in this graph. Edges between nodes represent friendships, check-ins, tags, relationships, ownership, attributes, etc.



Both nodes and edges have metadata associated with them. For example, the node corresponding to me will have my name, my birthday, etc. and the node corresponding to the Page Breville will have its title and description as metadata. Nodes in the graph are identified by a unique number called the fbid.

Graph Search. Search others





**What`s the index look like:**



hash table



the key will be the for example:





Suppose David(10003)'s friend has fbid 1234 and lives in New York (111) and likes Soccer (222 ) . The



[index corresponding to my friend will include the mappings:]{.mark}

friend:10003 → 1234

lives-in:111 → 1234

like:222 → 1234



we can see we have different type or differnt entity



Keep different entity types in separate index service verticals



Given that different entity types (users, pages, photos, etc.) have very different requirements for ranking, we decided to maintain each entity type in a separate Unicorn vertical and create

a new entity call the top aggregator--that would combine the different results across the verticals.



**How big the index will be**



if we want to build the index for friendship table

we have 1 billion user and every user has 50 friend



each index will be 51*5 = 255 bytes



and 255 * 1 billion = 255 G



**How to shard the**



it seems like store those index that is too large to fit into the memory of a single machine.

So we break up the index into multiple shards such that each shard can fit on

a single machine.





These machines are called index servers. Queries to Unicorn are sent to a

Vertical Aggregator, which in turn broadcasts the queries to all index servers. Each index

server retrieves entities from its shard and returns it to the Vertical Aggregator -- which then combines all these entities and returns them to the caller.

Unicorn is fundamentally an in-memory "database" with a query language for retrieval.















If we take "employers of my friends who live in New York" as an example query, in the first step, our search system fetches the list of nodes that are connected by a specified edge-type to the input nodes. ME --[friend-edge]--> my friends (who live in NY). In the second step, it takes the set of result nodes from the first step and fetches the list of nodes that are connected to those nodes by another specified edge type. [MY FRIENDS FROM NY]--[works-at-edge]--> employers. We call this the "apply" operator, because it takes the first set of results and "applies" a query to these results.





Suppose we wish to index the following five users: Mark Zuckerberg (fbid: 4), Randi Zuckerberg (fbid: 13755), Mark David Johnson (fbid: 1001), Randi Johnson (fbid: 5542), and David Johnson (fbid: 10003). The mappings we use are:



mark → 4



zuck → 4



randi → 13755



zuck → 13755



mark → 1001



david → 1001



johnson → 1001



randi → 5542



johnson →5542



david → 10003



johnson → 10003







**How big the index will be**



The posts index is much larger than other search indexes that Facebook maintains

The solution we decided on involves storing the majority of the index on solid-state flash memory.





Shading base on the friend id



the order of entities in each posting list must be the same. For example, they can be ordered by fbid, time of creation of the entity, etc.





The Life of a Graph Search Query

The interaction with Graph Search takes place in two distinct phases -- the query suggestion phase, and the search phase.



In the query suggestion phase the searcher enters text into the

search box and obtains suggestions. The search phase starts from the selected suggestion and produces a results page.



The Query Suggestion Phase

When a searcher types a query into the search box, a Natural Language Processing (NLP) module attempts to parse it based on a grammar. It identifies parts of the query as potential entities and passes these parts down to Unicorn to search for them.



In the query suggestions phase, the Unicorn Top Aggregator fans out the search request to all the verticals and then blends together the results on the way back. The sequence of steps

that takes place are:



1. The NLP module parses the query, and identifies portions to search in Unicorn.



Steps 2 through 9 are then performed simultaneously for each of these search requests.



2. The Top Aggregator receives the request and fans it out for each vertical. Steps 3 through 8 are performed simultaneously for each vertical.



3. The Top Aggregator rewrites the query for the vertical. Each vertical has different query rewriting requirements. Rewritten queries are typically augmented with additional



searcher context.

4. The Top Aggregator sends the rewritten query to the vertical -- first to the Vertical Aggregator, which passes it on to each of the Index Servers.

5. Each Index Server retrieves a specified number of entities from the index.

6. Each of these retrieved entities is scored and the top results are returned from the Index

Server to the Vertical Aggregator.

7. The Vertical Aggregator combines the results from all Index Servers and sends them

back to the Top Aggregator.

8. The Top Aggregator performs result set scoring on the returned results separately for

each vertical.

9. The Top Aggregator runs the blending algorithm to combine the results from each

vertical and returns this combined result set to the NLP module.

10. Once the results for all the search requests are back at the NLP module, it constructs all

possible parse trees with this information, assigns a score to each parse tree and

shows the top parse trees as suggestions to the searcher.
