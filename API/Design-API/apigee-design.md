- A key litmus test we use for Web API design is that there should be only **2 base URLs per resource**. /dogs /dogs/1234

![image.png](https://i.imgur.com/hJWrUBd.png)

- Resources almost always have relationships to other resources

- Once you have the primary key for one level, you usually don't need to include the levels above because you've already got your specific object. In other words, you shouldn't need too many cases where a URL is deeper than what we have above

- **Facebook** No matter what happens on a Facebook request, you get back the 200-status code - everything is OK. Many error messages also push down into the HTTP response

- The Google GData API uses only 10 status codes; Netflix uses 9, and Digg, only 8.

- Never release an API without a version; Use a simple ordinal number.

- partial response : /joe.smith/friends?fields=id,name,picture

## paginate:

- Let's look at how Facebook, Twitter, and LinkedIn handle pagination. Facebook uses **offset** and **limit**. Twitter uses page and **rpp** (records per page). LinkedIn uses **start** and **coun**

- To get records 50 through 75 from each system, you would use: /dogs?limit=25&offset=50

- we also suggest including metadata with each response that is paginated that indicated to the developer the total number of records available.

- Actions like the following are your clue that you might not be dealing with a "resource" response.:

Let me search for more context about this statement to provide a better explanation.


Search files...
Let me explain what this statement means in the context of REST API design. The statement "Actions like the following are your clue that you might not be dealing with a 'resource' response" refers to identifying when an API endpoint might not be following RESTful principles.

In REST API design, we typically work with resources (nouns) rather than actions (verbs). Here's what this means:

1. **Resource-Oriented (Good REST Design)**:
   - `/users` - Represents a collection of users
   - `/users/123` - Represents a specific user
   - `/products` - Represents a collection of products

2. **Action-Oriented (Not RESTful)**:
   - `/calculate` - This is an action, not a resource
   - `/translate` - This is an action, not a resource
   - `/convert` - This is an action, not a resource

The document gives a specific example:
```
/convert?from=EUR&to=CNY&amount=100
```
This is not a resource-oriented endpoint because:
- It's performing an action (converting)
- It doesn't represent a persistent resource
- It's more like a function call than a resource manipulation

Instead, a more RESTful approach might be:
```
/currencies/EUR/exchange-rates/CNY
```
or
```
/exchange-rates?from=EUR&to=CNY
```

The key principle is that REST APIs should be resource-oriented, where:
- URLs represent resources (nouns)
- HTTP methods (GET, POST, PUT, DELETE) represent actions on those resources
- The focus is on manipulating resources rather than calling functions

This helps maintain consistency, predictability, and scalability in API design.

## Calculate Translate Convert

- For example, an API to convert 100 euros to Chinese Yen: /convert?from=EUR&to=CNY&amount=100

- We recommend that you support more than one format

 - **Google Data** ?alt=json

- We recommend the Foursquare approach.

To get the JSON format from a collection or specific element:

dogs.json

/dogs/1234.json

## What about default formats?

In my opinion, JSON is winning out as the default format. JSON is the closest thing we have to universal language. Even if the back end is built in Ruby on Rails, PHP, Java, Python etc., most projects probably touch JavaScript for the front-end. It also has the advantage of being terse - less verbose than XML.



























