# anti corruption layer



---

<https://www.youtube.com/watch?v=7fT6B7lO9OU&t=5s>



you can think your service code be pristine and has clean model and it's ready use new technology and you want to prevent the latency concept from polling you nice pristine service.



prevent your latency concept from corrupting of your domain model of your service hence the term corruption layer.



left is the service layer right we got the latency application the corruption layer site between the two. the anti corruption layer has API and that express in term of domain language of service. So that is pristine API into the corruption layer.



on the other side of anti corruption layer is facade - the API for latency system which is basic wraps latency functionality, it is written the in term of language of latency application. ~~So the latency domain concept~~ and between two is a adopt implement the API of corruption layer and then it is responsible us to the translator components to translator between two different domain model.



So that two API and between them is transition layer translate from service domain model concept into latency domain model concept vice versa. In some case the anti conception layer is bi-direction layer

~~Where is the anti conception reside. where is remoting boundary between service and anti corruption and latency application. one option is as the anti corruption layer as module inside the service and facade makes remote cools into the latency system. you can use this when the latency system is the code you cannot change anyway.~~



~~other strategy is put the facade into the latency system. if is that code we own. we can implement the facade inside the latency system.~~



~~to help wit the refectory affect implementation of this anti-corruption latency. in this case the adopt making a remote cool into the facade the inside the latency system.~~



~~the final option is put the entry anticorruption into the latency system.~~



~~the trade of this how much code you want to make it. How much the way of coding change do you want to make to the monolith.~~










