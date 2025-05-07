# Memory

Created: 2017-10-20 09:07:54 -0600

Modified: 2017-10-20 09:20:53 -0600

---

we can use a write ahead log, operation add to the log, we add to the log first



we can use store it in multiple matchine and



the write the data only consider suff if



if the machine restart, we can build the memory base on the log. we need keep the log small, we use a checkpoint, that means if the log grows to the certain size, we will serialize it to the disk and the structure of the disk log is same as the memory and

To minimize startup time, we just need load the file in the disk and replay the limited number of log record after that.




