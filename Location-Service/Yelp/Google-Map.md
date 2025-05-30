# Google Map



---

Problem description

- Design a google map service
- Support display maps for certain cities(San Francisco, Seattle etc)
- Support tracking the user movement(GPS, telematics data)
- Support routing from place A to place B
- Support the place look up service
- [Bonus] Tracking of the traffic.
- [Bonus] tracking of traffic light etc



[High Level]{.mark}



User end devices -> load balancer -> graph processing services, location services, navigation services, look up services



locations services: to track users' movement,

Navigation services: to provide routes between users' current location to the destinations

Graph processing services: map services, including map renderings

Look up services: basic information about cities, places, etc.

Latency is low

Reliability is high

Scalability is high

1 billion,

30min on google map,

10 cities look up

5 routes look up

3 routes navigation

15 look up

20min / day - tunnel connections

QPS:

Look ups: 1b * 15 / 30min

Tunnels connections ( socket connection) : 20min / day





[Read/Write Ratio:]{.mark}

Location services: 1 : 1

Navigation service: 1 : 10^5

Graph processing service: 10 ^ 5 : 1

Look up services: 10 ^ 5 : 1





Data Storage:

Redis - key value store

MongoDB - Information of cities

User

User_id

location

Location: LatLng

Latitude, longitude



Map:

City: SF

Id

Name : my Current Location

Addresses(street, city, state, country, zipcode): 100 King st

Location (latitude, longitude)

Polygon

City: SF

Id

Name: McDonalds

Addresses(street, city, state, country, zipcode): 94107: 1435 Market st

Nearest vertex:

Location (latitude, longitude)

Polygon

Route:

Start

End

Paths

traffic



Lookup service: (elastic search: localized google search) restaurant: -> my current location to restaurant

Navigation service:

(BFS): <https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm>

Dijkstra

A* weight

->

Vertex -> edge

Edge -> weighted roads with distance and speed limit

Vertex -> road intersection



Pinpoint your location and restaurant location to an edge or vertex of the graph. -> mapping to vertex, add the ETA to that nearest vertex

Restaurant

Address
