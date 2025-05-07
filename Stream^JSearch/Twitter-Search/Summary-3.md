# Summary 3

Created: 2021-01-23 02:06:04 -0600

Modified: 2021-01-26 01:31:07 -0600

---

1.  Map reduce job : map emit the key value pair, key is the word and value is doc id and word position, then partitioner suffer and group all same key together, write to file and send to reducer



2. for the real time index, when the real time index full, merge to disk with the existing index



3. index : key word -> list of pair{ document id, position} // sort by document create time

4. key word in the memory and list of pair in the desk , key word + disk offset








