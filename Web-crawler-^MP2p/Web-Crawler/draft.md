# draft

Created: 2017-10-15 17:27:49 -0600

Modified: 2017-12-07 12:57:44 -0600

---

2. The fetcher module: The purpose of a fetcher module is to download the document corresponding to a given URL using the appropriate network protocol like HTTP.



Once the document has been written to the RIS, the worker thread invokes the content-seen test to determine whether this document (associated with a different URL) has been seen before . If so, the document is not processed any further



What is RIS

Document input stream: Our crawler's design enables the same document to be processed by multiple processing modules. To avoid downloading a document multiple times, we cache the document locally using an abstraction called a Document Input Stream (DIS).

A DIS is an input stream that caches the entire contents of the document read from the internet. It also provides methods to re-read the document. The DIS can cache small documents (64 KB or less) entirely in memory, while larger documents can be temporarily written to a backing file.

Each worker thread has an associated DIS, which it reuses from document to document. After extracting a URL from the frontier, the worker passes that URL to the relevant protocol module, which initializes the DIS from a network connection to contain the document's contents. The worker then passes the DIS to all relevant processing modules.



Every downloaded document has an associated MIME type. Processing modules associated with the MIME Type.

For instance extracting links from HTML pages, counting the tags found in HTML pages, or collecting statistics about GIF images.

Each link is converted into an absolute URL, and tested against a user-supplied URL filter to determine if it should be downloaded . If the URL passes the filter, the worker performs the

URL-seen test , which checks if the URL has been seen before, namely, if it is in the URL frontier or has already been downloaded. If the URL is new, it is added to the frontier .

What is URL filter

URL filters: This is used to blacklist websites so that our crawler can ignore them. Before adding each URL to the frontier, the worker thread consults the user-supplied URL filter. We can define filters to restrict URLs by domain, prefix, or protocol type





3.2. The URL Frontier

In a standard FIFO queue, elements aredequeuedin the order they wereenqueued. In the context of web crawling.

The fact that it is considered socially unacceptable to have multiple HTTP requests pending to the same server.



First, there is one FIFOsubqueueper worker thread. That is, each worker thread removes URLs from exactly one of the FIFOsubqueues. Second, when a new URL is added, the FIFOsubqueuein which it is placed is determined by the URL's canonical host name.

In actual world wide web crawls, the size of the crawl's frontier numbers in the hundreds of millions of URLs. Hence, the majority of the URLs must be stored on disk. To amortize the cost of reading from and writing to disk, our FIFOsubqueueimplementation maintains fixed-sizeenqueueanddequeuebuffers in memory; our current implementation uses buffers that can hold 600 URLs each.

Related:

Sub-product design pattern



To perform this test, we can calculate a 64-bit checksum of every processed document and store it in a database. For every new document, we can compare its checksum

to all the previously calculated checksums to see the document has been seen before. We can use MD5 or SHA to calculate these checksums



3.5. Content-Seen Test

we maintain a data structure called the document fingerprint set that stores a 64-bit

checksum of the contents of each downloaded document. We compute the checksum using Broder's implementation [5] of Rabin's fingerprinting algorithm [19].

When crawling the entire web, the document fingerprint set will obviously be too large to be stored entirely in memory. We therefore maintain two independent sets of fingerprints: a small hash table kept in memory, and a large sorted list kept in a single disk file.

The content-seen test first checks if the fingerprint is contained in the in-memory table. If not, it has to check if the fingerprint resides in the disk file. To avoid multiple disk seeks and reads per disk search, Mercator performs an interpolated binary search of an in-memory index of the disk file to identify the disk block on which the fingerprint would reside if it were present. It then searches the appropriate disk block, again using interpolated binary search.

When a new fingerprint is added to the document fingerprint set, it is added to the in-memory table. When this table fills up, its contents are merged with the fingerprints on disk, at which time the in-memory index of the disk file is updated as well.

Domain name resolution: Before contacting a Web server, a Web crawler must use the Domain Name Service (DNS) to map the Web server's hostname into an IP address. DNS name resolution will be a big bottleneck of our crawlers given the amount of URLs we will be working with. To avoid repeated requests, we can start caching DNS results by building our local DNS server.





**3.8. URL-Seen Test**



To perform the URL-seen test, we store all of the URLs seen by Mercator in canonical form in a large table called the*URL*

*set*. Again, there are too many entries for them all to fit in memory, so like the document fingerprint set, the URL set is stored mostly on disk.

To save space, we do not store the textual representation of each URL in the URL set, but rather a fixed-sized checksum.



To reduce the number of operations on the backing disk file, we therefore keep

an in-memory cache of popular URLs.



However, due to the prevalence of relative URLs in web pages,



two independent fingerprints: one of the URL's host name, and the other of the complete URL. These two fingerprints are merged so that the high-order bits of the checksum derive from the host name fingerprint.



As a result, checksums for URLs with the same host component are numerically close together. So, the host name locality in the stream of URLs translates

into access locality on the URL set's backing disk file, thereby allowing the kernel's file system buffers to service read requests from memory more often.



Can we use bloom filters for deduping?



A large bit vector

represents the set. An element is added to the set by computing 'n' hash functions of the element and setting the corresponding bits. An element is deemed to be in the set

if the bits at all 'n' of the element's hash locations are set. Hence, a document may incorrectly be deemed to be in the set, but false negatives are not possible.

The disadvantage to using a bloom filter for the URL seen test is that each false positive will cause the URL not to be added to the frontier, and therefore the document

will never be downloaded. The chance of a false positive can be reduced by making the bit vector larger.



Check Point



Mercator writes regular snapshots of its state to disk.

An interrupted or aborted crawl can easily be restarted from the latest checkpoint.











These snapshots are orthogonal to Mercator's other disk-based data structures. An interrupted or aborted crawl can

easily be restarted from the latest checkpoint. We define a generalcheckpointinginterface that is implemented by the



Data Partitioning

Our crawler will be dealing with three kinds of data: 1) URLs to visit 2) URL checksums for dedupe 3) Document checksums for dedupe.

Since we are distributing URLs based on the hostnames, we can store these data on the same host. So, each host will store its set of URLs that need to be visited,

checksums of all the previously visited URLs and checksums of all the downloaded documents. Since we will be using extended hashing, we can assume that URLs will

be redistributed from overloaded hosts.

Each host will performcheckpointingperiodically and dump a snapshot of all the data it is holding into a remote server. This will ensure that if a server dies down,

another server can replace it by taking its data from the last snapshot.





9. Crawler Traps

A crawler trap is a URL or set of URLs that cause a crawler to crawl indefinitely.



For example, people have written

traps that dynamically generate an infinite Web of documents.



AOPIC algorithm (Adaptive Online Page Importance Computation), can help mitigating common types of bot-traps. AOPIC solves this problem by using a credit system.

1. Start with a set of N seed pages.

2. Before crawling starts, allocate a fixed X amount of credit to each page.

3. Select a page P with the highest amount of credit (or select a random page if all pages have the same amount of credit).

4. Crawl page P (let's say that P had 100 credits when it was crawled).

5. Extract all the links from page P (let's say there are 10 of them).

6. Set the credits ofPto 0.

7. Take a 10% "tax" and allocate it to a Lambda page.

8. Allocate an equal amount of credits each link found on page P from P's original credit after subtracting the tax, so: (100 (P credits) - 10 (10% tax))/10 (links) = 9

credits per each link.

9. Repeat from step 3



Since the Lambda page continuously collects the tax, eventually it will be the page with the largest amount of credit, and we'll have to "crawl" it. By crawling the

Lambda page, we just take its credits and distribute them equally to all the pages in our database.



Since bot traps only give internal links credits and they rarely get credit from the outside, they will continually leak credits (from taxation) to the Lambda page. The

Lambda page will distribute that credits out to all the pages in the database evenly, and upon each cycle, the bot trap page will lose more and more credits until it has so

little credits that it almost never gets crawled again. This will not happen with good pages because they often get credits from backlinks found on other pages.



![](../../media/Web-crawler-^MP2p-Web-Crawler-draft-image1.png){width="0.6041666666666666in" height="1.0208333333333333in"}![](../../media/Web-crawler-^MP2p-Web-Crawler-draft-image2.png){width="0.5972222222222222in" height="0.875in"}



![](../../media/Web-crawler-^MP2p-Web-Crawler-draft-image3.png){width="4.479166666666667in" height="0.4861111111111111in"}



