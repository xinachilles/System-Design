# sumary  

Created: 2017-10-16 22:54:57 -0600

Modified: 2017-10-22 11:44:46 -0600

---

five main components:

Desktop Client Application, Master Service, Message Queuing

Service and chuck service



The Client Application and the Master Service

interact through the Message Queuing Service. (like redis)



The Master Service also interacts with the Metadata

Database for data persistence. Client Application directly

interacts with the Cloud Storage to upload and

download files.



Desktop client



we have a folder, work place store the files also store in the dropbox



The Desktop Client Application monitors the workspace or sync folders and synchronizes them with the remote Cloud Storage.



Some of the most important requirements of the Desktop Client include upload and download of the files, detecting file changes in the sync folder, and handling conflicts due to offline or concurrent updates.



The main components of the desktop client are Watcher, Chunker, Indexer, and Internal DB as





Watcher monitors the sync folders and notifies the Indexer of any action performed by the user for example when user create, delete, or update files or

folders.



Indexer processes the events received from the Watcher and updates the internal database with information about the chunks of the modified files.



the Indexer will communicate with the Master Service using the Message Queuing Service to update the Metadata Database with the

changes.



chunks are successfully submitted to the Cloud Storage,



![How to write a file? 1, write Chunk index---I 2, Assign CSI client 4. Write Finish 3. Transfer Data= lgfs/home/dengchao. mp4-01-Of-09 ChunkServer1 4. Write Finish info N gchao. master Chunk O I •>ChunkServerI Chunk 02 •>ChunkServerI Chunk 03->ChunkServer2 Key • Master clientfiötchunk ChunkServer2 ](../../media/File-System-File-System-sumary-image1.png){width="5.0in" height="2.5347222222222223in"}



![One time to write, Many time to read. hfflN/gfs/home/dengchao.mp4 ](../../media/File-System-File-System-sumary-image2.png){width="5.0in" height="2.0069444444444446in"}











Chunker splits the files into smaller pieces called chunks (64 M). To reconstruct a file, chunks will be joined back together in the correct order. A chunking

algorithm can detect the parts of the files that have been modified by user and only transfer those parts to the Cloud Storage, saving on cloud storage space





Internal Database keeps track of the chunks, files, their versions, and their location in the file system.



![Metadata Fileinfo Name=dengchao.mp4 CreatedTimF201505031232 Size-2044323232 Chunk I I ->diskOffsetl Chunk 12->diskOffset2 Chunk 13->diskOffset3 Disk chunks 12 15 13 Key point 1 chunk= 64M = 64*1024K Advantage • Reduce size of metadata Disadvantage Waste space for small files ](../../media/File-System-File-System-sumary-image3.png){width="5.0in" height="2.4375in"}





on the service side:



![Scale about the Storage ChunkServer5 mpQ1 Lmk-O I mpQ1 Lmk-(j2 mpQ1 mpQ1 i _ mp-4<imk-O I i _ mp-4<imk-02 Copyright O wwvv.jiuzhang.com Master Meta Data Name=dengchao mp4 CreatedTime=201505031232 Size---10042044323 Index Chunk 01->cs5 Chunk 02->cs5 Chunk 03->cs5 Chunk 04->cs3 Chunk 05->cs3 Chunk 06->cs3 Key point • The master don't record the diskOffset of a chunk Advantage Reduce the size of metadata in master Reduce the traffic between master and ChunkServer(chunk offsetüö ](../../media/File-System-File-System-sumary-image4.png){width="5.0in" height="2.7291666666666665in"}

The Master Service is the component that

processes file updates made by a client and applies these

changes to other subscribed clients.





The master stores three major types of metadata: the file and chunknamespaces, the mapping from files to chunks, and the locations of each chunk's replicas. All metadata is kept in the master's memory. The first two types (namespaces and file-to-chunkmapping) are also kept persistent by logging mutations to an operation log stored on the master's local diskand replicated on remote machines.



Using a log allows us to update the master state simply, reliably, and without risking inconsistencies in the event of a master crash. The master does not store chunklocation information persistently. Instead, it asks each chunkserver about its chunks at master startup and whenever a chunkserver joins the cluster.

every chuck will ask master about the check service




