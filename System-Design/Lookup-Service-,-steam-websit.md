# Lookup Service , steam website



---

![Sorted Way (18 Memory MetaData DiskO, EastKeyOfDiskO(D), addressO Diskl, lastKeyOfDiskI(N), addressl DiskO A hump B nice C good D mother Diskl E hump F nice G good N mother Key • Sort Key + Split Key Advantage Don't store every key in the memory Problem split ](../media/System-Design-Lookup-Service-,-steam-website-image1.png)



![Consistent Hash Memory MetaData DiskO, address0 Diskl, addressl Consistent Hash Map DiskO: [0-300] Diskl: [301-600] Disk2: [601-999] Has 100 20 50 DiskO A hump D mother E hump F nice Diskl B nice C good G good N mother ](../media/System-Design-Lookup-Service-,-steam-website-image2.png)













![How do Slave Server store the key value on disk? 5. Look up ke•F'A' on server O Slave Server O client 1. Look Up 10. Return [Ahumpl 6. Look Up key Return chunk 01 Memory 8. Find chu 01 4, Return Slave Server() Master 2. Look up Hash( BIOOO Retum Slave ServerO Consistent Hash Map Chunk Chunk Offset t Return chunk index 01 A hump D mother Slave Server Oneclient ](../media/System-Design-Lookup-Service-,-steam-website-image3.png)

![](../media/System-Design-Lookup-Service-,-steam-website-image4.png)

这个表只有读过一遍以后才会生成







chunk id



chunk offset



![Summary of Lookup Service • Design Client + Master + Server Client • . Look up • Consistent Hash Map Server Maintain the Data (Key value pairs) • Connect to GFS Master Shard the file Maintain the MetaData (Similar to GFS master) Manage the servers health ](../media/System-Design-Lookup-Service-,-steam-website-image5.png)





