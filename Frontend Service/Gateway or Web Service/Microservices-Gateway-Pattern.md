
---

<https://www.youtube.com/watch?v=PKIypT8vmIc&t=279s>





here we've got a typical application and web app that has a customer information microservice ,a wishlist microservices , and an order placement micro service. In this is a this is a typical order entry application all communicating through rest.



The problem is we have a custom payment system in the main frame. There is a third-party product or It could be a custom system. It's all COBOL in the main frame as a matter fact and it communicates to the rest of the world to a protocol called otma.



Now the question is how in the world do we communicate with this kind of system in microservices. The answer is a third kind of micro service and that's called a microservices Gateway.



I'm going to have a separately deployed micro service.Single purpose and it does one thing really well. It know how to take a restful request and convert that to otma.





There for being able to communicate with that custom payment system in the Main frame. for example to do refunds or credits or to check store credit or two maybe look at gift cards or partial refunds.All the kind of operations.



Now, usually the endpoints to get to that custom payment mainframe system either through an API or inter-service.



It would be through the endpoint in that gateway micro service. right there ~~and usually these are dedicated to specific systems but let me show you how I use gateways. Because I usually don't dedicate a gateway to a specific system but rather~~ I have a microservices Gateway for each operation within that custom system







For example a let's say that there's 20 different operations within that custom payment system in the Mainframe COLBO, I can do for example, I get my gift card balance charge, a gift card I select a payment method for a credit card or a debit card maybe I can do a partial refund or a full refund



Let's say there's 20 different operations within my application now I haven't converted yet. It's still in the Main frame.



So now each microservices Gateway is dedicated to a specific operation within that third-party product or custom system.





Now the power of this is twofold. First of all when I convert functionality from COBOL let's say over to Java or C# . I've got a microservices Gateway there that is fronting that kind of protocol transformation or contract transformation to get over to COBOL.



but now as I start rewriting that COBOL over to Java in that Microservices Gateway I have a data switch that now changes it from going to the Main frame to go into custom Java logic within that Gateway. Now I've got a placeholder there so that that Gateway in time becomes a functional service



Here's the other piece. I may have order placement. In the other form of these gateways is that there is one that single solitary service that in turn fronts that custom system not many.



So in other words if order placement needed to charge a card that goes to the custom payment system in the Main frame, but instead order placement does a inner service call to the microservices Gateway which has all the logic to do a payment which then in turn goes to the mainframe system





let's say the customer information system wanted to be able to display your balance on a gift card. That's in the custom payment system there on the Mainframe



so now the customer information system goes to a microservices Gateway which fronts that operation and so now I can do inner service communication within my microservices ecosystem all through rest and I have one and only one microservice that knows about that mainframe system



This is a really effective way of being able to communicate with other types of systems other application whether or not you're planning on moving those two microservices as well


