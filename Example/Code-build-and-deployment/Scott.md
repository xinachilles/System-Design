# Scott



---

Author: Scott Shi

Date: June 7, 2020

**Problem description**

In a multiple region multiple zone data center, there is a need to replicate an ever changing file ~10GB to a growing 10k nodes. Every node could go down. There should not be any data loss.



Solution:

High level architecture is using zookeeper and keeps track of the location of master file, the version change of master file, membership management of a file sharing group. And use bittorrent to transfer the large files peer to peer to avoid a single node traffic overloaded.

First thing first:

- The master file should not be changed too frequently, otherwise the "write amplification" is proportional to the number of nodes that need to be replicated. If write is done too frequently, the next round of replications could be started while previous rounds are still ongoing.
- File should be replicated in case a single point of failure in master file and crash the whole system
- There should be no data loss for the changes appended to the file
- Read from replicas could read stale data but not corrupted data.

1.  You have a small group of machines (> 1, 3 is a good number) to keep the up to date version of files, each write is ensured to write to all of the machines before returning success to the requester. New write is appended as changelogs to a file. eg. file_name+new_version_number. The write order is the total broadcast order, i.e no write loss and all replicas got the changes in the same order. If small data loss is acceptable, no replication needed.
2.  When a certain threshold is met, like after a certain time period or after more than M number of changes, a pump of version is issued. New append log file created as file_name+(new_version_number+1), all new requests are redirected to this new log file. [The old log is now fully merging with the old file]{.mark}.
3.  Master node of this small group uploads the merged file of the current version to amazon S3 (old file), once the file upload is completed. Register the zookeeper with the newly created version of file. If the master node is dead, promote a slave node as master.
4.  Zookeeper also tracks the group memberships for the broadcast group, since this is within the DC, membership changes should be rare.
5.  Once zookeep is updated with a new version and its amazon s3 location, a new download command is issued to all group members, group members will use bittorrent to share the files among them, first starting from the seeder. (seed group)
6.  At any given time each replica will have a list of the same file of different versions: FileA_1, FileA_2, FileA_3_crdownload, etc. the download in progress file will have an additional appendix, and should not be used to serve read requests. The stale version of files could be cleaned up later by a background process.
