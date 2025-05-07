# Uber 

Created: 2018-01-19 12:57:17 -0600

Modified: 2018-01-19 12:57:27 -0600

---

Scenario (add one words)

Service will store the information about different place and user can search on them

and service will return the result base on user`s search

API

search(api_dev_key, search_terms, user_location, radius_filter, maximum_results_to_return,

category_filter, sort, page_token)

api key, each installed api have a different key and we can user this key to monitor the user and throttle user.

How to update the driver information to customer

When user open the APP, customer will tell the current location to service and service will find out the a push service for this customer and set the nearby drives to that service then send to customer

there should be a hashmap in the push service, the key will be the customer key and value will be the push service the customer is connecting. Dispatch service will send the nearby drive information let assume every minutes and to push service and push service will sent it to the customer

How to partition,

Partition by city id or user way call geo fence, geo fence is human define geographic area. It is a polygon

in this case, we need another table just for geofence, key will be the geofence id and the value will be some points for this geofenc e, this information will use to calcite the given location is in this geofence or not.

geolocation table

key will be the geofence key + a increase number

column will be geohash and place id


