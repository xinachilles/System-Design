(draft)

use application-level circuit breakers for connections to downstream services if it observes significant amout of 5xx errors comming from a particular service (subgraph) in short period of time.

This result in all requests comming to that subgraph or sub-services will get failed immediately for a short period. 

This mechanism was put in place to prevent spiraling negative feedback loops of requests and to give the subgraph room to recove.