# How to build the index , update the index



---

Indexing (or the process of building the index) is performed as a combination of map-reduce scripts that collect data from Hive tables, process them, and convert them into the inverted index data structures.



We separate the data into two parts - the document data and the inverted index. The

document data for each post contains information that will later be used for ranking results.



![Input O: Deer Bear River 1: Car River 2: Deer Car Bear split O: Deer Bear River 1: Car River 2: Deer Car Bear //key: the id of a doc //value: the content of the line Map( string key, string value) for each word in value: Output( word, key); Map Deer: O Bear, O River, O Car: 1 River: 1 Deer: 2 Car: 2 Bear: 2 Bear: O Bear: 2 Car: 1 Car: 2 Deer: O Deer: 2 River: O River: 1 //key: the name of a word Reduce Output Bear: Car: ear: car: 1,2 Deer: River: Deer: 0 2 River: 0 1 //valueList: the appearances of this word in documents Reduce( string key, list valueList ) List sumList; for value in valueList: sumList.append(value); OutputFinal( key, sumList ); Copyright O wwvv.jiuzhang.com ](../../media/Stream^JSearch-Twitter-Search-How-to-build-the-index-,-update-the-index-image1.png)











Updating the Index



Whenever a new post is created, an existing post is modified, a post is

deleted, or some connection to a post is edited, we schedule an update on the corresponding

post.



because the data is in the cache, we can just update the data in cache

