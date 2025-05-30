# Summary 



---

1.  requirement

Is it possible assume how many web page need to be crawl?

we want to crawl 15 billion pages within four weeks

How long we need crawl those pages

[What is the expected number of pages we will crawl? How big will the URL database become?]{.mark}

Assuming we need to crawl one billion websites. Since a website can contain many, many URLs, let's assume an upper bound of 15 billion different web pages that will be reached by our crawler.



[What protocols are we looking at? HTTP? What about FTP links? What different protocols should our crawler handle?]{.mark}

[Is it a crawler for HTML pages only? Or should we fetch and store other types of media, such as sound files, images, videos, etc?]{.mark}

What is 'RobotsExclusion' and how should we deal with it? Courteous Web crawlers implement the Robots Exclusion Protocol, which allows Webmasters to declare parts of their sites off limits to crawlers. The Robots Exclusion Protocol requires a Web crawler to fetch a special document called robot.txt, containing these declarations from a Web site before downloading any real content from it.

Capacity Estimation and Constraints

If we want to crawl 15 billion pages within four weeks, how many pages do we need to fetch per second?

15B / (4 weeks * 7 days * 86400 sec) ~= 6200 pages/sec

What about storage?

We assume page has 100k and 500 bytes of metadata

15B * (100KB + 500) ~= 1.5 petabytes

Assuming a 70% capacity model (we don't want to go above 70% of the total capacity of our storage system), total storage we will need:

1.  petabytes / 0.7 ~= 2.14 petabytes

High level design

1.  First we have a URL list, called seeds, crawls in one service or different services should pick up URL from URL list
2.  We need to normalize the url, filter some invalid url , seen that URL before or need to re-visit --- "Last crawler timestamp DB "

And "domine police DB"

3.  Send the url to URL frontier

The URL frontier

A.  will store the URL is ready to crawler



B.  will determine what pages to visited first base on the score, this score is computed base a number of different attributes this process will did by the background process and store in the "Domain police DB " or "URL DB "



| **URL ID** | **URL** | **LAST CRAWLED TS** | **EXCLUDED FROM CRAWLER** | **SIMILAR CONTENT GROUP ID** |
|---------|--------|----------------|--------------------|-------------------|







1.  Determine the IP Address of its host-name.
2.  Establishing a connection to the host and download the corresponding document.
3.  Parse the document contents and get the new URLs.
4.  Add the new URLs to the list of unvisited URLs.
5.  Go back to step 1

Re-visit police:

The web is updating all the times, we need some algorithm to determine if we need to crawler this page again

URL normalization



Crawlers usually perform some type of [URL normalization](https://en.wikipedia.org/wiki/URL_normalization) in order to avoid crawling the same resource more than once. ~~The term *URL normalization*, also called *URL canonicalization*, refers to the process of modifying and standardizing a URL in a consistent manner.~~ There are several types of normalization that may be performed including conversion of URLs to lowercase, removal of "." and ".." segments, and adding trailing slashes to the non-empty path component





Details of Design

1.  Let's assume our crawlers is running multiple server. We also need a crawlers table record the servers of the crawls, status.

Those crawling thread connect the internet and download the document base on the given URL and the appropriate protocol

If we have different protocol, we need remember which crawler user which type protocol

2.  After the document were download, we pass it to RIS , document input stream. This input stream just for the document transfer

it will cache the document locally ( put the small document in the memory and save the large document in the disk) and since we need sent the document to different model so use RIS can avoid download the document multiple times.

3. After we write to the IRS, we need send the document the context test model to test this document has been seen before

(We can use MD5 or SHA to calculate these checksums)

there is algorithm call document fingerprint and store a 64 bit check sum we put some small to the memory and larger sored list in the disk

The content-seen test first checks if the fingerprint is contained in the in-memory table. If not, it has to check if the fingerprint resides in the disk file.

The fingerprint is sort in the desk, we can maintenance an index in the memory and identify which fingerprint in which disk block, in the disk block, we aslo has an index file for the fingerprint and his offset

When a new fingerprint is added to the document fingerprint set, it is added to the in-memory table. When this table fills up, its contents are merged with the fingerprints on disk, at which time the in-memory index of the disk file is updated as well.

4. if the document is new, we can pass it to Process model

we will extract the links from HTML pages and save the pages to the file system.

5.  We pass those URL link to the URL filter first. This is used to blacklist websites we can ignore them. We also need test if the url has been seen before

To perform the URL-seen test, we store all of the URLs a large table called the URL set. Again, there are too many entries for them all to fit in memory, so like the document fingerprint set, the URL set is stored mostly on disk. like the document, we don`t store the whole URL, we just store the checksum of URL

To reduce the number of operations on the backing disk file, we therefore keep an in-memory cache of popular URLs.

We can computer the checksum of host and check sum of result of URL separately and merge together

The checksums for URLs with the same host are close together. So the system can find checksum very quick if the url has the same host

6. after pass the url test, we save the url to task database

The schema of the URL database will be : id, url, status(working, new , done), priority

`

url extractor is FIFO queue, elements are dequeued in the order they were enqueued and send to the web loader mode

7.  after save the new url to task database, Sender service will get notification and pick the URL and send the URL to carwl



Domain name resolution: Before contacting a Web server, a Web crawler must use the Domain Name Service (DNS) to map the Web server's hostname into an IP address.

DNS name resolution will be a big bottleneck. To avoid repeated requests, we can start caching DNS results by building our local DNS server.

3.8. Data Partitioning

Since we are distributing URLs based on the hostnames, we can store these data on the same host. So, each host will store its set of URLs that need to be visited,

checksums of all the previously visited URLs and checksums of all the downloaded documents.


