# Summary 

Created: 2020-12-02 12:36:34 -0600

Modified: 2020-12-02 16:33:54 -0600

---

1.  after user press the build button, system push a message to message queue and the worker on other side will subscribe the message will grab the code from a temp location and build the code store the code in the blob storage and update the build table, insert this to zookeeper or configuration service



the configuration will store the binary name, file name and location





2.  there should be one global configuration service and multiple region config service, region will monitor the global change , or the global will push the new binary and location config to the local config center, if there are new binary file build complete,


3.  use p2p network, the local config center will give all machine in the same region a download command

<!-- -->

4.  the region machine will grab the chunk file from the GFS and deploy to locally





zookeep not write too often 1/min










