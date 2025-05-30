# Uber draft



---

For U



There are two kinds of users in our system 1) Drivers 2) Customers.

Drivers need to regularly notify the service about their current location and their availability to pick passengers.

Passengers get to see all the nearby available drivers.

Passengers can request a ride; nearby drivers are notified that a customer is ready to be picked up.

Once a driver and customer accept a ride, they can constantly see each other's current location, until the trip finishes.+

Upon reaching the destination, the driver marks the journey complete to become available for the next ride.







3. Capacity Estimation and Constraints

Let's assume we have 300M customer and 1M drivers, with 1M daily active customers and 500K daily active drivers.

Let's assume 1M daily rides.

Let's assume that all active drivers notify their current location every 4 seconds.



The QPS will be 500/4 = 125k

We don`t need calculate the passages QPS

Once a customer puts a request for a ride, the system should be able to contact drivers in real-time.

Storage for U



Service:

We just need two service for 1 dispatch service 2. Geo service



Geo service driver will report his location to dispatch service every 4 seconds and dispatch service will save the location to Geo service

Passengers get to see all the nearby available drivers and request a ride and dispatch service will through Geo service find available driver and return to passage

Trip table and driver location table



Store in the SQL .the table will be drive id and geohash and index the geohash and use like to find out the drive id

(By myself) like u, write heavy system, store drive information to Hbase

Like POI is read heavy then write heavy, store the geo information in Cassandra

From jiuzhang : store the geohash in Casandra and geohash will be the column key and use range query to find nearby place.

Store in HBase ..



If store in the memecahed or redis:

At least will 3 bucket : for example 9q9hvt

Key will be 9q9h, 9q9hv and 9q9hvt ( 0.6km) and 4 digital geohash is 20 km

Table will like { key, {drive id1 , drive id2....}}



Jiuzhang :

Chose redis:

First find 6 digital bucket then 5 then 4

In the redit, there is another table call driver table

If find the available drive

Key: drive id

Value {lat, lng, status, update at(time stamp}, trip id}



From drive side:

Report his location and update the location table( if necessary) and drive table

If drive accept the request

Update the trip id for drive table

And driver will get the trip information



For passage side

Request a ride, create a trip table for that passage

And will check the service every server second to find out any driver accepted his request or not



If any driver accept the request: passenger will get the driver information and drive current location



sharding will base on the geofence










