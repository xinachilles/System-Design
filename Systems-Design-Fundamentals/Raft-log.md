# Raft-log

Created: 2020-11-23 13:07:05 -0600

Modified: 2020-11-23 13:16:26 -0600

---

[why use log]{.mark}



1 log can be used for the order operation because the log has the slot number



2.  log can be used to retransmit to the replica if some of replica offline
3.  log can be use to reply the operation when the service is crash and restart
4.  all the operation have been committed to the disk, the log will be deleted or log can know which part is committed to desk and which not is not committed
5.  lead's log and replicas log are not identical, there are some mechisem to fouce all log identical





[the good news is that the the way a raft works actually ends up forcing the logs]{.mark}

[to be identical after a while there may be transient differences but in the long]{.mark}

[run all the logs will sort of be modified by the leader until the leader insurers are all identical]{.mark}
















