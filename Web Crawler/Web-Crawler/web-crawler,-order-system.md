# web crawler, order system



---

![Design a web crawler Scenario: How many web pages? how long? how large? 2. crawl 1.6m web pages per second 1 trillion web pages crawl all of them every week 10p (petabyte) web page storage average size of a web page: 10k ](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image1.png)



![](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image2.png)



![](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image3.png)



![](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image4.png)



![The web Crawler Machine 1 Crawler Machine 2 Crawler Machine 3 Task table (db) WebPageStorage ](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image5.png)



![](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image6.png)



![A Distributed Web Crawler A: task table sharding! WebPageStorage The web Crawler Machine 1 Crawler Machine 2 Crawler Machine 3 Scheduler Task table (db) Task table (db) Task table (db) ](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image7.png)



![A Distributed Web Crawler Answer: Exponential back-off! success: crawl after 1 week no. 1 failure: crawl after 2 week no.2 failure: crawl after 4 weeks no.3 failure: crawl after 8 weeks . ](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image8.png)



![](../../media/Web-crawler-^MP2p-Web-Crawler-web-crawler,-order-system-image9.png)



store everything to web page storage









