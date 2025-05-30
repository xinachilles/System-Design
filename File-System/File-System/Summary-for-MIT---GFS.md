# Summary for MIT - GFS



---

[MIT- 3- GFS](onenote:..MIT.one#MIT-%203-%20GFS%20&section-id={B73BEFC0-606E-044E-89A5-61AF37C842BB}&page-id={842A70A8-9291-BD49-9228-69DE5A23B94B}&end&base-path=https://d.docs.live.net/77339d157d673f41/Documents/System%20Design)



The file was divided by chunk, each chunk is 64M and stored in the chunk servers







Master node



keeps the mapping from file names to where to find the data.



in the master, it's got two main tables :



one table that map's file name to an array of chunk IDs or chunk handles.



Second table that map's each chunk handle to a bunch of data about that chunk, so one is the list of chunk servers that hold replicas of that data, each chunk is stored on more than one chunk server



Chunk id -> list of chunk service and which one is primary



Primary is only allowed to be primary for a certain least time





Master node need remeber 1. file name -> chunk id, 2 version number for each chunk



chunk servers it turns out doesn't because the master if it reboots talks to all the chunk servers and ask them



The version number need to be increased when file change or primary chunk service change





[Read]{.mark}



Pass the file name and offset

Base on the offset/64 find which chunk handler and



Get the list of chunk service and read the chunk from the service closet to client



Cache the chunk list so don't need to talk to mast next time





Write

1.  If no primary, master node need to choose a master first base on the version number ( version number in master = version number in chunk service)



Increase the version number



Tell primary and secondary the new version number and ask them ready to accept new data



Give primary a new lease



Tell client the primary and secondary location



Client send the copy of data to primary and secondary



Base on the Dropbox

Chunk can be store in the chunk service with key: hash, value: content

If the chunk on the client side has the same hash value, means duplicate



Primary need to make sure secondary write the chunk on the same offset



[chunk servers it turns out doesn't because the master if it reboots talks to all the chunk servers and ask them]{.mark}












