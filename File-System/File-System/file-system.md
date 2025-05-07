# file system 

Created: 2017-05-06 15:18:39 -0600

Modified: 2020-01-31 15:38:44 -0600

---

![Interviewer: How to save a file in one machine ](../../media/File-System-File-System-file-system-image1.png){width="5.947916666666667in" height="3.3541666666666665in"}



![Metadata Fileinfo Size-2044323 Index Block 11- >diskOffset Block 12->diskOffset2 Block 13- >diskOffset3 Block 14- >diskOffset4 Disk blocks Block 10 11 Block 12 Block 13 Block Key point 1 block = 1024Byte • Block Advantage? ](../../media/File-System-File-System-file-system-image2.png){width="5.947916666666667in" height="3.6458333333333335in"}



![](../../media/File-System-File-System-file-system-image3.png){width="5.947916666666667in" height="3.2604166666666665in"}



![每 个 chunk 的 Offset 偏 移 量 可 不 可 以 不 存 在 master 上 面 ？ ](../../media/File-System-File-System-file-system-image4.png){width="5.947916666666667in" height="1.8958333333333333in"}



![ChunkServer5 Dengchao chunk .01 Offset3 Dengchao m •chunk •02 Offset3 Den gChao m - Den gchao m - 04- -01 Sun ch -02 Master Meta Data CreatedTime-201505031232 Size-40042044323 Index Chunk Chunk 02->cs5 Chunk 03->cs5 Chunk Chunk 05->cs3 Chunk 06 >CS3 Key point • The master don't record the diskOffset of a chunk Advantage Reduce the size of metadata in master ](../../media/File-System-File-System-file-system-image5.png){width="5.947916666666667in" height="3.0in"}



![Now to write a file. client 3. Transfer Data= 1, write Chunk index---I 2. Assign CSI 4. Write Finish master Key info Nam e c h ao.mp4 Index Chunk 01- > ChunkServerI Chunk 02- > ChunkServerI Chunk 03- >ChunkServer2 /gfs/home/dengchao. mp4 • 01- Of- 09 ChunkServer1 Master hÉdchunkserver% clientfi$frchunk ChunkServer2 ](../../media/File-System-File-System-file-system-image6.png){width="5.947916666666667in" height="3.1979166666666665in"}



![How to read from a file? client 1. Filename=/gfs/home/dengchao.mp4 2. return a chunk list 3. Read /gfs/home/dengchao.mp4-00-of-09 in nk server 1 master N a me: /gfs/home/dengchao.mp4 Chunk List Chunk index 01 Chunk server 4. Return data /gfs/home/dengchao.m OO-of-09 Chunkserverl ](../../media/File-System-File-System-file-system-image7.png){width="5.947916666666667in" height="4.020833333333333in"}



![Check Sum 检 查 一 位 错 误 原 来 错 误 后 数 据 二 进 制 表 示 数 据 二 进 制 表 示 01 01 10 11 11 11 Checksum(xor) 00 Checksum(xor) 01 ](../../media/File-System-File-System-file-system-image8.png){width="5.947916666666667in" height="2.65625in"}



![选 chunk server 的 时 候 有 什 么 策 略 ？ 1 ， 最 近 写 入 比 较 少 的 。 (LRU) 2 ， 硬 盘 存 储 比 较 低 的 。 ](../../media/File-System-File-System-file-system-image9.png){width="5.947916666666667in" height="1.7395833333333333in"}



![需 要 多 少 个 备 份 ？ 每 个 备 份 放 在 哪 ？ 1 ， 三 个 备 份 都 放 在 一 个 地 方 （ 加 州 ） 。 2 ， 三 个 备 份 放 在 三 个 相 隔 较 远 的 地 方 （ 加 州 ， 滨 州 ， 纽 约 州 ） 3 ． 两 个 备 份 相 对 比 较 近 ， 另 一 个 放 在 较 远 的 地 方 （ 2 个 加 州 ， 1 个 滨 州 ） ](../../media/File-System-File-System-file-system-image10.png){width="5.947916666666667in" height="2.9895833333333335in"}



![1, write Chunk index---I WIti•Output Device Talking: Questions Audience Questi on master client 3. Transfer Data: /gfs/home/dengchao.mp4 -C I of-09 2. Assign Chunkserver_server, CSI, CS2, CS3 ChunkServer1 3. Transfer Data: /gfs/home/ ChunkServer2 Audience Question --- we question here. V sent webinar log 140-119-131 e GoToWebinar ChunkServer3 ](../../media/File-System-File-System-file-system-image11.png){width="5.947916666666667in" height="3.0in"}



![](../../media/File-System-File-System-file-system-image12.png){width="10.083333333333334in" height="6.208333333333333in"}![](../../media/File-System-File-System-file-system-image13.png){width="0.96875in" height="0.46875in"}



![](../../media/File-System-File-System-file-system-image14.png){width="10.083333333333334in" height="6.072916666666667in"}

什么时候检查 读的时候检查 A chunkis broken up into 64 KB blocks. Each has a corresponding 32 bit checksum. Like other metadata, checksums are kept in memory and stored persistently with logging, separate from user data.







![](../../media/File-System-File-System-file-system-image15.png){width="0.7604166666666666in" height="0.7708333333333334in"}![](../../media/File-System-File-System-file-system-image16.png){width="0.4166666666666667in" height="1.1041666666666667in"}![](../../media/File-System-File-System-file-system-image17.png){width="0.53125in" height="0.71875in"}![](../../media/File-System-File-System-file-system-image18.png){width="0.53125in" height="0.5833333333333334in"}![](../../media/File-System-File-System-file-system-image19.png){width="0.28125in" height="0.9895833333333334in"}![](../../media/File-System-File-System-file-system-image20.png){width="0.25in" height="0.23958333333333334in"}![](../../media/File-System-File-System-file-system-image21.png){width="0.71875in" height="0.78125in"}![](../../media/File-System-File-System-file-system-image22.png){width="0.4583333333333333in" height="0.5625in"}![](../../media/File-System-File-System-file-system-image23.png){width="0.2708333333333333in" height="0.34375in"}



![](../../media/File-System-File-System-file-system-image24.png){width="10.083333333333334in" height="6.09375in"}![](../../media/File-System-File-System-file-system-image25.png){width="10.083333333333334in" height="5.864583333333333in"}



![How to solve ChunkServer failure? 1, File Name, Chunk id master client 3. data Copyright O wwvv.jiuzhang.com 2, Chunkserver locations , 4. fail ChunkServer1 CSI 3. data unkServer2 . al ChunkServer3 87 ](../../media/File-System-File-System-file-system-image26.png){width="10.083333333333334in" height="5.458333333333333in"}



太阁



checksum 存在chunkservice 的内存里


























