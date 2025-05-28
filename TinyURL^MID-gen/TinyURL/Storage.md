# Storage

Created: 2017-10-09 16:08:16 -0600

Modified: 2017-10-09 17:05:37 -0600

---

SQL or NO SQL



1.  don`t need support transaction
2.  don`t need support join option
3.  QPS is not too high ( 2k and is read heavy than write heavy)
4.  need Sequential id





![Web Wver 1k QPS SQL oa 1k ops NOSC_X Cassandra. 'v6ngoOB Nosu oe (Reds) 1 DOk aps NOSQL 0B • IMQPS • me-ncE 10k aps ](../../media/TinyURL^MID-gen-TinyURL-Storage-image1.png){width="3.9791666666666665in" height="1.75in"}













two solution



1.  No SQL





we just random generate a 6 digitals for given long URL, if this short URL is not used. We just insert it into database.



![public String longToShortCString url) { while (true) { String shortURL = randomShortURL(); if C! database. exists() { database. longURL=url); return shortURL; ](../../media/TinyURL^MID-gen-TinyURL-Storage-image2.png){width="5.0in" height="1.6145833333333333in"}





it should be very quick at the begin and will very slow when the url grow









![](../../media/TinyURL^MID-gen-TinyURL-Storage-image3.png){width="5.0in" height="2.1979166666666665in"}



![NoSQL Cassandra row row Long Short _key=longURL, column_key=ShortURL, value=null or timestamp Short Long _key=shortURL, column_key=LongURL, value-null or timestamp ](../../media/TinyURL^MID-gen-TinyURL-Storage-image4.png){width="5.0in" height="2.09375in"}



![](../../media/TinyURL^MID-gen-TinyURL-Storage-image5.png){width="5.0in" height="2.5104166666666665in"}











