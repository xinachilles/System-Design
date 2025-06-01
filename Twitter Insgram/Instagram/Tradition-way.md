# Tradition way



---

(Tradition

From an image's URL a Photo Store server extracts the volume and full path to the file, reads the data over NFS, and returns the result to the CDN.



We initially stored thousands of files in each directory of an NFS volume which led to an excessive number of disk operations to read even a single image. Because the NAS metadata is too large to store in the cache



generally incur 3 disk operations to fetch an image: one to read the directory metadata into memory,

a second to load theinodeinto memory, and a third to read the file contents.



To further reduce disk operations we let the Photo Store servers explicitly cache file handles returned by the NAS appliances. When reading a file for the first time a Photo Store server opens a file normally but also caches the filename to file handle mapping inmemcache.

When requesting a file whose file handle is cached, a Photo Store server opens the file directly using a custom system call, open byfilehandle. This file handle cache provides only a minor improvement as less popular photos are less likely to be cached to begin with.



However, that only addresses part of the problem as it relies on the NAS appliance having all of itsinodesin main memory, an expensive requirement for traditional filesystems.

System needs enough main memory so that all of the filesystem metadata can be cached at once. In our NAS-basedapproach,onephoto corresponds to one file and each file requires at least oneinode, which is hundreds of bytes large. Having enough main memory in this approach is not cost-effective.)
