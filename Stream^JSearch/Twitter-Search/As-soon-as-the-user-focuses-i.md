# As soon as the user focuses in the text box, we send off a request to retrieve all of the user's friends, pages, groups, applications, and upcoming events. We load these results into the browser's cache, so that the user can find these results immediately

Created: 2017-10-15 14:51:00 -0600

Modified: 2017-10-15 15:21:56 -0600

---

As soon as the user focuses in the text box, we send off a request to retrieve all of the user's friends, pages, groups, applications, and upcoming events. We load these results into the browser's cache, so that the user can find these results immediately without sending another request. The old typeahead did this, but stopped here.





if we cannot find the enough result, browser will send the request to index aggregator service and the aggregator service will send the query to multiple lower -level services



the lower services rank and return the result





The aggregator merges the results and features returned from each leaf service and ranks the results according to our model. The top results are returned to the web tier.





The results returned by the aggregator are simply a list of ids. The web tier needs to fetch all the data from memcache/[MySQL](http://www.facebook.com/MySQLatFacebook)to render the results and display information like the name, profile picture, link, shared networks, mutual friends, etc. The web tier also needs to do privacy checking here to make sure that the searcher is allowed to see each result.




