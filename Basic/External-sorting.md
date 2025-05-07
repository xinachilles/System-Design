# External sorting

Created: 2017-10-15 15:34:31 -0600

Modified: 2020-12-16 01:25:01 -0600

---

One example of external sorting is the externalmerge sort algorithm, which sorts chunks that each fit in RAM, then merges the sorted chunks together. For example, for sorting 900Mof data using only 100 M of RAM:







1.  Read 100 MB of the data in main memory and sort by some conventional method, like[quicksort](https://en.wikipedia.org/wiki/Quicksort).
2.  Write the sorted data to disk.
3.  Repeat steps 1 and 2 until all of the data is in sorted 100 MB chunks (there are 900MB / 100MB = 9 chunks), which now need to be merged into one single output file.





4.  Read the first 10 MB (= 100MB / (9 chunks + 1)) of each sorted chunk into input buffers in main memory and allocate the remaining 10 MB for an output buffer. (In practice, it might provide better performance to make the output buffer larger and the input buffers slightly smaller.)

(so we have 9 input buffer and 1 output buffer)



5.  Perform a[9-way merge](https://en.wikipedia.org/wiki/K-way_merging)and store the result in the output buffer. Whenever the output buffer fills, write it to the final sorted file and empty it. Whenever any of the 9 input buffers empties, fill it with the next 10 MB of its associated 100 MB sorted chunk until no more data from the chunk is available.



- 9 way merge

<!-- -->
- Input: a list of*k*lists.
- While any of the lists is non-empty:
- Loop over the lists to find the one with the minimum first element.
- Output the minimum element and remove it from its list.













The previous example is a two-pass sort: first sort, then merge. The sort ends with a single*k*-way merge, rather than a series of two-way merge passes as in a typical in-memory merge sort. This is because each merge pass reads and writes*every value*from and to disk.



The limitation to single-pass merging is that as the number of chunks increases memory will be divided into more buffers, so each buffer is smaller. This causes many smaller reads rather than fewer larger ones. Thus, for sorting, say, 50 GB in 100 MB of RAM, using a single merge pass isn't efficient: the disk seeks required to fill the input buffers with data from each of the 500 chunks (we read 100MB / 501 ~ 200KB from each chunk at a time) take up most of the sort time. Using two merge passes solves the problem. Then the sorting process might look like this:

1.  Run the initial chunk-sorting pass as before.
2.  Run a first merge pass combining 25 chunks at a time, resulting in 20 larger sorted chunks. (25 *20 = 500 )
3.  Run a second merge pass to merge the 20 larger sorted chunks.




