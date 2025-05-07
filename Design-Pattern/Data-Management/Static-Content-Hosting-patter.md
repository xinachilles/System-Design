# Static Content Hosting pattern

Created: 2018-12-13 00:59:26 -0600

Modified: 2018-12-13 00:59:58 -0600

---

Deploy static content to a cloud-based storage service that can deliver them directly to the client. This can reduce the need for potentially expensive compute instances.



Context and problem

Web applications typically include some elements of static content.

This static content might include HTML pages and other resources such as images and documents that are available to the client, either as part of an HTML page (such as inline images, style sheets, and client-side JavaScript files) or as separate downloads (such as PDF documents).



For maximum performance and availability, consider using a content delivery network (CDN) to cache the contents of the storage container.







This pattern might not be useful in the following situations:



The application needs to perform some processing on the static content before delivering it to the client. For example, it might be necessary to add a timestamp to a document.

The volume of static content is very small. The overhead of retrieving this content from separate storage can outweigh the cost benefit of separating it out from the compute resource.



Example



The links in each page will specify the URL of the resource and the client will access it directly from the storage service.
