# MapReduce - ACE

Created: 2021-05-24 16:42:49 -0600

Modified: 2021-05-24 17:05:37 -0600

---

map reduce have map task and reduce task. All the task a independent



There are group of map jobs will read the input document and split those doc to groups of words then generate to key value pair -- (it call intermediate files)



Shuffling -- combine the same key together and Mapreduce framework will deploy those key value pair to different reduce job and generate the output file











![MapReduce Input Ri•a Dee Bar Splitting KI,VI Mapping Shuffling car, Reducing Car, 3 Final Result Riv«, 2 ](../../media/Stream^JSearch-Mapreduce-MapReduce---ACE-image1.png){width="10.083333333333334in" height="6.052083333333333in"}





![MapReduce (1) fork (2) assign . .map (4) write user Program (1) fork (1) fork execution log on GFS Master (O assign reduce split O split 1 split 2 split 3 split 4 Input (6) write 3 read worker Map Intermediate files (on GFS) Reduce output Output fie s @ acecodeintervu ](../../media/Stream^JSearch-Mapreduce-MapReduce---ACE-image2.png){width="10.083333333333334in" height="6.208333333333333in"}


