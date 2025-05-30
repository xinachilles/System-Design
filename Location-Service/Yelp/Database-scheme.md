# Database scheme 



---

Scenario

For POI



Functional Requirements:

1. Users should be able to add/delete/update Places.

2. Given their location (longitude/latitude), users should be able to find all nearby places within a given radius.



3. Users should be able to add feedback/review about a place. The feedback can have pictures, text, and a rating.



Non-functional Requirements:

1. Users should have real-time search experience with minimum latency.

2. Our service should support a heavy search load. There will be a lot of search requests compared to adding a new place.



Service for POI:





How many place

have 500M places and 100K queries per second (QPS).

At a high level, we need to store the location `s information such as address, latitude, longitude



For users can search for nearby places and see the result in real-time.



Given that the location of a place doesn't change that often, we don't need to worry about frequent updates of the data. As a contrast, if we intend to build a service where

objects do change their location frequently, e.g., people or taxis, then we might come up with a very different design.



Store age

Each location can have following fields:

1. LocationID (8 bytes): Uniquely identifies a location.

2. Name (256 bytes)

3. Latitude (8 bytes)

4. Longitude (8 bytes)

5. Description (512 bytes)

6. Category (1 byte): E.g., coffee shop, restaurant, theater, etc.

Although a four bytes number can uniquely identify 500M locations, with future growth in mind, we will go with 8 bytes for LocationID.

Total size: 8 + 256 + 8 + 8 + 512 + 1 => 793 bytes





We also need to store reviews, photos, and ratings of a Place. We can have a separate table to store reviews for Places:

1. LocationID (8 bytes)

2. ReviewID (4 bytes): Uniquely identifies a review, assuming any location will not have more than 2^32 reviews.



3. ReviewText (512 bytes)

4. Rating (1 byte): how many stars a place gets out of ten.

Similarly, we can have a separate table to store photos for Places and Reviews.



How to present the location: GeoHash
