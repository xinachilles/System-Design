# Map Reduce -summary 2

Created: 2017-10-12 17:22:14 -0600

Modified: 2021-05-24 16:42:43 -0600

---

map reduce have map task and reduce task.



map task was independent of the other map task

reduce task is essentiallyindependent of the other



map run on the input data block



the map output is sored in the local disk at the server on which map task is running



reduce input is read from the multiple remote disks, Finally, when the reduce output is done,it is written to the distribute file system back, sothe output of the entire map reduce job is available in the distributed file system.



The resource manager, which runs a scheduler of MapReduce,is responsible forassigning these maps tasks to servers and it's also responsible forassigning the reduce tasks





For instance Server A might have some map tasks that are running on it,that are generating data, output shuffle data,for the reduce task that is assigned to it.That data doesn't go anywhere.It does not go on the network.It stays local to A.However, this third map task might have some output,which needs to be sent to this first reduce task.And that reduce task will need to go over the network and be sent to server A.




