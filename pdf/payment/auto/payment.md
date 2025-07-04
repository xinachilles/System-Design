and flexible payment system is essential.   
What is a payment system? According to Wikipedia, "a payment system is any system used to settle financial transactions through the transfer of monetary value. This includes the institutions, instruments, people, rules, procedures, standards, and technologies that make its exchange possible" [1].   
A payment system is easy to understand on the surface but is also intimidating for many developers to work on. A smallslip could potentilly cause significant revenue loss and destroy credibity among users. But fear not! In this

Step 1 - Understand the Problem and Establish Design Scope

Pay or Google Pay. Others may think it's a backend system that handles payments such as PayPal or Stripe It is ve important to determine the exact requirements at the beginning of the interview. These are some questions ye can ask the interviewer:

Candidate: What payment options are supported? Credit cards, PayPal, bank cards, etc?   
Interviewer: The payment system should support all of these options in real life. However, in this interview, we ce use credit card payment as an example.   
Candidate: Do we handle credit card payment processing ourselves?

Candidate: Do we store credit card data in our system? Interviewer: Due to extremely high security and compliance requirements, we do not store card numbers direct in nur euetam IMa raly nn third_nartu navmant nrnracenre tn handl eanritire rrarlit rardl rlata

Interviewer: Great question. Yes, the application would be global but we assume only one currency is used in th interview.   
Candidate: How many payment transactions per day?   
Interviewer: 1 milion transactions per day.   
Candidate: Do we need to support the pay-out flow, which an e-commerce site like Amazon uses to pay sellel

Interviewer: Yes. A payment system interacts with a lot of internal services (accounting, analytics, etc.) and extermal serices (payment service providers). When a service fails, we may see inconsistent states among services. Therefore, we need to perform reconcilition and fix any inconsistencies. This is also a requirement. With these questions, we get a clear picture of both the functional and non-functional requirements. In this interview, we focus on designing a payment system that supports the following.

• Pay-in flow: payment system receives money from customers on behalf of sellers. • Pay-out flow: payment system sends money to sellers around the world. Non-functional requirements

(payment service providers) is required. The process asynchronously verifies that the payment informatio across these systems is consistent.   
Back-of-the-envelope estimation   
The system needs to process 1 million transactions per day, which is 1,000,000 transactions / 10^5 seconds = 1   
transactions per second (TPs). 10 TPs is not a big number for a typical database, which means the focus of thi

system design interview is on how to correctly handle payment transactions, rather than aiming for higi

Step 2 - Propose High-Level Design and Get Buy-In At a high level, the payment flow is broken down into two steps to reflect how money flows: • Pay-in flow

Take the e-commerce site, Amazon, as an example. After a buyer places an order, the money flows into Amazon's bank account, which is the pay-in flow. Although the money is in Amazon's bank account, Amazon does not own all of the money. The seller owns a substantial part of it and Amazon only works as the money custodian for a fee. Later, when the products are delivered and money is released, the balance after fees then fows from Amazon's bank account to the sellers bank account. This is the pay-out flow. The simplifed pay-in and pay-out flows are

Duyci 达 oCLICI ? Crerit Pay-in ABeoknt Pay-out ABcount Figure 1 Simplified pay-in and pay-out flow

The high-level design diagram for the pay-in flow is shown in Figure 2. Let's take a look at each compone   
system. ：

①Payment event PayPal VISA Payment ③Payment Payment 5 stripe Service Order Executor 1 MasterCard @ 6 ④ aulyen Payment Service Card Schemes Ledger Wallet Providers (PSP) Payment System Internal External Figure 2 Pay-in flow

Ine payment service accepts payment events rom users and coorainates tne payment process. Ine tirst tning it usually does is a risk check, assessing for compliance with regulations such as AML/cFT [2], and for evidence of criminal activity such as money laundering or financing of terrorism. The payment service only processes payments that pass this risk check. Usually, the risk check service uses a third-party provider because it is very complicated and highly specialized.

contain several payment orders.   
Payment Service Provider (PSP)   
A PSP moves money from account A to account B. In this simplified example, the PSP moves the money out of th   
buyer's credit card account. d erhamnr

The ledger keeps a financial record of the payment transaction. For example, when a user pays the seller \$1, we record it as debit \$1 from a user and credit \$1 to the seller The ledger system is very important in post-payment analysis, such as calculating the total revenue of the e-commerce website or forecasting future revenue. Wallet

As shown in Figure 2, a typical pay-in flow works like this: 1. When a user cicks the place order"' button, a payment event is generated and sent to the payment service. 2. The payment service stores the payment event in the database. 3. Sometimes, a single payment event may contain several payment orders. For example, you may select product

4. The payment executor stores the payment order in the database.   
5. The payment executor calls an external PsP to process the credit card payment.   
6. After the payment executor has successfull processed the payment, the payment service updates the wallet tc allor ha   
8. After the wallet service has successfully updated the sellers balance information, the payment service clls th ledger to update it.   
9. The ledger service appends the new ledger information to the database. APls for payment service   
We use the RESTful API design convention for the payment service. PoST /v1/payments

Field Description Type buyer_info The information of the buyer json checkout_id A globally unique ID for this checkout string credit_card_info specific. json payment_orders A list of the payment orders list Table 1 API request parameters (execute a payment event)

currency The currencyfor the order string (ISO 4217 [4])

payment_orders look like this: Field Description Type seller_account Which sller il receive the money string amount The transaction amount for the order string

Note that the payment_order_id is globally unique. When the payment executor sends a payment request to a thirc party PSP, the payment_ order_id is used by the PSP as the deduplication ID, also called the idempotency key. You may have noticed that the data type of the "amount" field is "string," rather than 'double' Double is not good choice because: 1. Different protocols, software, and hardware may support different numeric precisions in serializtion and

deserialization. This difference might cause unintended rounding errors. 2. The number could be extremely big (for example, Japan's GDP is around 5x1014 yen for the calendar year 2020), or extremely small (for example, a satoshi of Bitcoin is 10-⁸).

Ine payment API mentioned above is similr to tne API of some wel-known srs. I you are interested in a more comprehensive view of payment APIs, check out Stripe's API documentation [5].   
The data model for payment service   
We need two tables for the payment service: payment event and payment order. When we select a storage solution for a payment system, performance is usually not the most important factor. Instead, we focus on the following: 1 Drnvan ctahilitv Whether the etnrane cuetem hae heen ucer hu nthar hin finanrial firme fnr manv veare Ifnr example more than 5 years) with positive feedback   
2. The richness of supporting tools, such as monitoring and investigation tools.   
3. Maturity of the database administrator (DBA) job market. Whether we can recruit experienced DBAs is a very imnartant fartor tn cnneidar

lally, we prefer a traditional relational database with ACID transaction support over NoSQL/NewSQL. : payment event table contains detailed payment event information. This is what it looks like: Name Type

buyer_info string   
seller_info string   
credit_card_info depends on the card provider

payment_order_id String PK buyer_account string amount string currency string checkout_id string FK payment_order_status string ledger_updated boolean wallet_updated boolean

payment orders. • When we call a third-party PSP to deduct money from the buyer's credit card, the money is not directl transferred to the seller. Instead, the money is transferred to the e-commerce website's bank account. Thi process is called pay-in. When the pay-out condition is satisfied, such as when the products are delivered, th elle initiatee a nav-nut ∩nlv then ic the mnnev traneferrerd frnm the e-rnmmere weheite'e hank arrnunt ta

the seller's bank account. Therefore, during the pay-in flow, we only need the buyers card information, not the seller's bank account information. n the payment order table (Table 4), payment_order_status is an enumerated type (enum) that keeps the execution logic is: 1. The initial status of payment_order_status is NOT_STARTED. 2. When the payment service sends the payment order to the payment executor, the payment_order_status i EXECUTING. 3. The payment service updates the payment_order_status to SUCcESS or FAILED depending on the response o

Once the payment_order_status is SUCCESs, the payment service callsthe wallet service to update the seller balance and update the wallet_updated field to TRUE. Here we simplify the design by assuming wallet updates always rienaadl

updating the ledger_updated field to TRUE. When all payment orders under the same checkout_ id are processed successfully, the payment service updates the is_payment_done to TRUE in the payment event table. A scheduled job usualy runs at a fixed interval to monitor the status of the in-fight payment orders. t sends an alert when a payment order does not finish within a threshold so

Double-entry ledger system There is a very important design principle in the ledger system: the double-entry principle (also called double-enty accounting/bookkeeping [6]). Double-entry system is fundamental to any payment system and is key to accurate

Account Debit Credit buyer S1 seller \$1

Table 5 Double-entry system The double-entry system states that the sum of al the transaction entries must be 0. One cent lost means someone else gains a cent.It provides end-to-end traceability and ensures consistency throughout the payment cycle. To finc out more about implementing the double-entry system, see Square's engineering blog about immutable double

Most companies prefer not to store credit card information internally because if they do, they have to deal with complex regulations such as Payment Card Industry Data Security Standard (PCI DSs) [8] in the United States. To avoid handling credit card information, companies use hosted credit card pages provided by PSPs. For websites, it is a widget or an iframe, while for mobile applications, it may be a pre-buit page from the payment SDK. Figure 3 illustrates an example of the checkout experience with PayPal integration. The key point here is that the PSP

PayPal Pay with PayPal itaauntueliioet pi Purchase Protection, and more.

![](images/7181a81a2b6482265f11e0077da22420d66f26f427c9b410efb53e9d3ae3ba18.jpg)

Email or mobile number

Pay with Debit or Credit Card Figure 3 Hosted pay with PayPal page

Pay-out flow   
The components of the pay-out fow are very imilar to the pay-i fow. One difference is that instead of using Pf to move money from the buyer's credit card to the e-commerce website's bank account, the pay-out flow uses i third-party pay-out provider to move money from the e-commerce website's bank account to the seller's banI account.

Step 3 – Design Deep Dive In this section, we focus on making the system faster, more robust, and secure. In a distributed system, erors and failures are not only inevitable but common. For example, what happens if a customer pressed the "pay' button multiple times? Wil they be charged multipletimes? How do we handle payment failures caused by poor network

myiurun •  Reconciliation • Handling payment processing delays • Communication among internal services • Handling failed payments • Fxart-nnce delivenv

If the payment system can directly connect to banks or card schemes such as Visa or MasterCard, payment can be made without a PSP. These direct connections are uncommon and highly specialized. They are usually reserved for really large companies that can justify such an investment. For most companies, the payment system integrates with a PSP instead, in one of two ways:

APl. The company is responsible for developing the payment web pages, collecting and storing sensitive payment information. PsP is responsible for connecting to banks or card schemes. 2.If a company chooses not to store sensitive payment information due to complex regulations and security rnnrarne DCD nrnuirlae a hnetad nnum mnnt datnile ra tham in Dcn

ueiOm Client Browser   
Checkout Hosted Payment Payment Completion Page Page Page … Redirect - ⑧to completionpage ⑤   
chekut pPspt Display ⑥ Start Payment@ / result payment 1

②Create payment ④Store Payment with nonce PSP token Service ③Retur payment token Payment System

1. The user cicks the "checkout" button in the client browser. The client calls the payment service with th payment order information. 2. After receiving the payment order information, the payment service sends a payment registration request t the PSP. This registration request contains payment information, such as the amount, currency, expiration dat of the pavment reauest. and the redirect URL. Because a pavment order should be reaistered onlv once. ther

the ID of the payment order. 1. The PSP returs a token back to the payment service. A token is a UUID on the PSP side that uniquely identifies the payment registration. We can examine the payment registration and the payment execution status later using this token.

i. Once the token is persisted, the client displays a PSP-hosted payment page. Mobile applications usually use th PSP's SDK integration for this functionality. Here we use Stripe's web integration as an example (Figure 5. Stripe provides a JavaScript library that displays the payment Ul, collects sensitive payment information, an calls the PsP directly to complete the payment. Sensitive payment information is collected by Stripe. It neve reaches our payment system. The hosted payment page usually needs two pieces of information: 1 Tha tnlen we rereiverd in eten 4 The DcD'e iavaerrint rnde uee the tnlen tn retriave detailerl infnrmatini

about the payment request from the PSP's backend. One important piece of information is how muc money to collect. 2. Another important piece of information is the redirect URL. This is the web page URL that is called whe the payment is complete. When the PSP's JavaScript finishes the payment, it redirects the browser to th rarlirert IIRI Ieuallv the rerirert IIRI ic an e-rnmmerre wah nane that chnwe the etatue nf the rherknu

Note that the redirect URL is different from the webhook [11] URL in step 9. D Powdur TEsT MoDE Dau

Pay Powdur  
\$129.00 Or paryEmailPure set \$65.00Qty 1vCard inforPure glow cream \$64.00 1234 1234 1234 1234 nsOty 2 \$32.00 eachMM / YY CVC 。

Figure 5 Hosted payment page by Stripe s. The user fils in the payment details on the PSP's web page, such as the credit card number, holder's nan expiration date, etc, then clicks the pay button. The PSP starts the payment processina.

United State ZIP ed by stripe Pay

tokenID-JI0UIQ123NSF&payResu1t-X324FSa 9. Asynchronously, the PSP calls the payment service with the payment status via a webhook The webhook is & URL on the payment system side that was registered with the PSP during the initial setup with the PSP. Whe the payment system receives payment events through the webhook, it extracts the payment status and update the payment_order_status field in the Payment Order database table.

Reconciliation When system components communicate asynchronously, there is no guarantee that a message will be delivered, a response will be returned. This is very common in the payment business, which often uses asynchrono to verify that they are in agreement. It is usually the last line of defense in the payment system. Every night the PSP or banks send a settlement file to their clients. The settlement fil contains the balance of bank account, together with al the transactions that took place on this bank account during the day. reconciiation system parses the settlement file and compares the details with the ledger system. Figure 6 be chnuie whare the rernnriliatinn nrnrace fite in the euetem

Internal External Payment Service Payment event Providers (PSP) Card Schemes ayPal VISA Payricnt Paycuto stripe Mastercard alyen Ledger Wallet Settlement file Reconciliation Payment System

Figure 6 Reconciliation Reconciliation is also used to verify that the payment system is internally consistent. For example, the states in tI

To fix mismatches found during reconciliation, we usually rely on the finance team to perform manual adjustments. The mismatches and adjustments are usually classified into three categories: 1. The mismatch is classifiable and the adjustment can be automated. In this case, we know the cause of the mismatch, how to fix it, and it is cost-efective to write a program to automate the adjustment. Engineers can

the mismatch and how to fix it, but the cost of writing an auto adjustment program is too high. The mismatct is put into a job queue and the finance team fixes the mismatch manually. 3.The mismatch is unclasifiable. In this case, we do not know how the mismatch happens. The mismatch is pu

Handling payment processing delays As discussed previously, an end-to-end payment request flows through many components and involves bott internal and external parties. While in most cases a payment request would complete in seconds, there an situations where a payment request would stall and sometimes take hours or days before it is completed o rejected. Here are some examples where a payment request could take longer than usual:

• The PSP deems a payment request high risk and requires a human to review it.   
• A credit card requires extra protection like 3D Secure Authentication [13] which requires extra details from : rard hnlder tn verifv a nurrhaee

ry payment requests in the following ways: • The PSP would return a pending status to our client. Our client would display that to the user. Our client would also provide a page for the customer to check the current payment status. The pcp trarke the nendinn naument nn nur hehalf and nntifiee nrire nf anv etatiie uinrdate via

When the payment request is finally completed, the PSP calls the registered webhook mentioned above. The payment service updates its internal system and completes the shipment to the customer. Alternatively, instead of updating the payment service via a webhook, some PSP would put the burden on the

Communication among internal services   
There are two types of communication patterns that internal services use to communicate: synchronous asynchronous. Both are explained below.

syncnronous communication Ike HI IP works wel or small-scale systems, but its snortcomings become obvious the scale increases.It creates a long request and response cycle that depends on many services. The drawbacks this approach are:

• Poor failure isolation. If PsPs or any other services fail, the client will no longer receive a response.   
• Tight coupling. The request sender needs to know the recipient.   
• Hard to scale. Without using a queue to act as a buffer, it's not easy to scale the system to support a sudc increase in traffic.

Asynchronous communication can be divided into two categories: • Single receiver each request (message) is processed by one receiver o service. It's usually implemented via a shared message queue. The message queue can have multiple subscribers, but once a message is processed. it gets removed

Service A ） . m4 m3 m2 ml Service B Figure 7 Message queue Service A (

m1   
∑ 7   
m4 m3 (

Service B Figure 8 Single receiver for each message Multiple receivers; each request (message) is processed by multple receivers or services. Kafka works well here. When consumers receive messages, they are not removed from Kafka. The same message can be processed by different

services. This model maps wel to the payment system, as the same request might trigger multiple side effects such as sending push notifications, updating financial reporting, analytics, etc. An example isillustrated in Figure 11. Payment

system Events 7 Analytics m4 m3 m2 m1 m1

uenieray apcani!y,ayunuus uunauus sper m ues, uu uuea aw seives  ut autonomous. As the dependency graph grows, the overall performance suffers. Asynchronous communicatior trades design simplicity and consistency for scalability and failure resilence. For a large-scale payment system witt complex business logic and a large number of third-party dependencies,asynchronous communication is a bette choice.

cvery payment system nas to nanaie ralea transactions. Keapity and rauit toierance are key requirements. we review some of the techniques for tackling those challenges.   
Tracking payment state   
Having a definitive payment state at any stage of the payment cycle is crucial. Whenever a failure happens, we car Retry queue and dead letter queue   
To gracefully handle failures, we utilize the retry queue and dead letter queue, as shown in Figure 12.   
• Retry queue: retryable errors such as transient errors are routed to a retry queue.   
• Dead letter queue [14]:if a message fails repeatedly, it eventually lands in the dead letter queue. A dead letter

B2 Yes Paymemt >Retryable? 1a Yes Retry Queue Failure 30No Retryable? Failure Dead Letter Queue bNo Database

2. The payment system consumes events from the retry queue and retries failed payment transactions.   
3. If the payment transaction fails again: 3a. If the retry count doesn't exceed the threshold, the event is routed to the retry queue. 3b. If the retry count exceeds the threshold, the event is put in the dead letter queue. Those failed events m need to be investigated. tilizes Kafka to meet the reliability and fault-tolerance requirements [16].   
ixactly-once delivery   
)ne of the most serious problems a payment system can have is to double charge a customer. It is important   
guarantee in our design that the payment system executes a payment order exactly-once [16].   
At first glance, exactly-once delivery seems very hard to tackle, but if we divide the problem into two parts,   
much easier to solve, Mathematically, an operation is executed exactly-once if: 1. It is executed at-least-once. We will explain how to implement at-least-once using retry, and at-most-once using idempotency check. Retry   
Occasionally, we need to retry a payment transaction due to network errors or timeout. Retry provides the at-leas once guarantee. For example, as shown in Figure 13, where the client tries to make a \$10 payment, but the paymel request keeps failing due to a poor network connection. In this example, the network eventually recovered and t request succeeded at the fourth attempt. Pay S10 X Payment faled   
Rety Pay S10 X Payment failed   
Rety Pay S10 Payment failed

Rety Pay 10 Payment succeeded Figure 11 Retry

for subsequent retries. • Exponential backoff [17]: double the waiting time between retries after each failed retry. For example, when ; request fails for the first time, we retry after 1 second; ifit fails a second time, we wait 2 seconds before th next retry; if it fails a third time, we wait 4 seconds before another retry.

)eciding the appropriate time intervals between retries is important. Here are a few common retry strategies. • Immediate retry: client immediately resends a request. • Fixed intervals: wait afixed amount of time between the time of the failed payment and a new retry attempt. • Incremental intervals: client waits for a short time for the first retry, and then incrementally increases the tin

Determining the appropriate retry strategy is difficult There is no "one size fits al" solution. As a general guideline use exponential backoffif the network isue is unlikely to be resolved in a short amount of time. Overly aggressive retry strategies waste computing resources and can cause service overload. A good practice is to provide an erro button twice. Scenario 2: The payment is successfuly processed by the PSP, but the response fails to reach our payment syster due to network errors. The user clicks the "pay" button again or the client retries the payment.

Idempotency s key to ensurng the at-most-once guarantee. According to Wikpedia, Tdempotence s the propert of certain operations in mathematics and computer science whereby they can be applied multiple times withou changing the result beyond the initial application" [18]. From an API standpoint, idempotency means clients car make the same call repeatedly and produce the same result. For communication between clients (web and mobile applications) and servers, an idempotency key is usually : unique value that is generated by the client and expires ater a certain period of time. A UUiD is commonly used a: an idemnotencv kev and it is recommended bv manv tech comnanies such as Strine I19] and PavPal [201. Te

issues mentioned above.   
Scenario 1: what if a customer clicks the "pay" button quickly twice?   
In Figure 14, when a user clicks "pay," an idempotency key is sent to the payment system as part of the HTTP request. In an e-commerce website, the idempotency key is usually the ID of the shopping cart right before the checkout.

POST (idempotency-key: UUID) Client Payment First request Charge succeeded Retry

multiple concurrent requests are detected with the same idempotency key, only one request is processed and the hers receive the \*429 Too Many Requests"status code.

Client Paysment   
Retry Retum previous message Do not process tha requast again Figure 12 Idempotency

database table is served as the idempotency key. Here is how it works: 1. When the payment system receives a payment, it tries to insert a row into the database table. 2. A successful insertion means we have not seen this payment request before. 3.If the insertion fais because the same primary key already exists, it means we have seen this payment request

nonce. Therefore, the token uniquely maps to the payment order.   
When the user clicks the "pay" button again, the payment order is the same, so the token sent to the PsP is th same. Because the token is used as the idempotency key on the PSP side, it is able to identify the double paymel and return the status of the previous execution.   
Cnncictenrv

Scenario 2: The payment is successfully processed by the PSP, but the response fails to reach our payment system due to network errors. Then the user clicks the "pay" again. As shown in Figure 4 (step 2 and step 3), the payment service sends the PSP a nonce and the PSP returns a corresponding token. The nonce uniquely represents the payment order, and the token uniquely maps to the

To maintain data consistency between the internal service and external service (PSP), we usually rely or idempotency and reconciliation. If the external service supports idempotency, we should use the same idempotenc. key for payment retry operations. Even if an external service supports idempotent APl, reconciiation is still neede because we shouldn't assume the external system is always right.

2. The ledger keeps all accounting data.   
3. The wallet keeps the account balance of the merchant.   
4. The PSP keeps the payment execution status.   
5. Data might be replicated among different database replicas to increase reliability.

1 a distributed environment, the communication between any two services can fail, causing data inconsis et's take a look at some techniques to resolve data inconsistency in a payment system. o maintain data consistency between internal services, ensuring exactly-once processing is very important.

1. Serve botn reads and writes trom tne prmary database onily. Inis approacn is easy to set up, but tne obvou drawback is scalability. Replicas are used to ensure data reliability, but they don't serve any trafi, which waste resources. 2. Ensure all replicas are always in-sync. We could use consensus algorithms such as Paxos [21] and Raft [22, c

Payment security   
Payment security is very important, In the final part of this system design, we briefly cover a few techniques f combating cyberattacks and card thefts.

Data tampering Enforce encryption and integrity monitoring Man-in-the-middle attack Use SSL with certificate pinning Data loss Database replication across multiple regions and take snapshots of data

Iokenization. Instead ot using real card numbers, tokens are stored and used   
Card theft for payment PCI DSS is an information security standard for organizations that handle   
PCI compliance branded credit cards

Fraud [26][27] Table 6 Payment security

In this chapter, we investigated the pay-in flow and pay-out flow. We went into great depth about retry idempotency, and consistency. Payment eror handling and security are also covered at the end of the chapter. A payment system is extremely complex. Even though we have covered many topics, there are stil more wort mentioning. The following is a representative but not an exhaustive list of relevant topics.

can answer questions like "What is the average acceptance rate for a specific payment method?", "What is the CPU usage of our servers?", etc. We can create and display those metrics on a dashboard.   
• Alerting. When something abnormal occurs, it is important to alert on-call developers so they respond promptly.   
• Debugqing tools. "Why does a payment fail? is a common question. To make debugqing easier for enqineers

• Cash payment. Cash payment is very common in India, Brazil, and some other countries. Uber [28] and Air [29] wrote detailed engineering blogs about how they handled cash-based payment. • Google/Apple pay integration. Please read [30] if interested. Congratulations on getting this far! Now give yourself a pat on the back. Good job!

processing server history, PSP records, etc. of a payment transaction. • Currency exchange. Currency exchange is an important consideration when designing a payment system for i international user base. • Geography. Different regions might have completely different sets of payment methods.

[14] Kafka Connect Deep Dive – Error Handling and Dead Letter Queues: https://www.confluent.io/blog/kafka-connect-deep-dive-error-handing-dead-letter-queues/ [15] Reliable Processing in a Streaming Payment System: https://www.youtube.com/watch?v=5TD8m7w1xE08list=PLLEUtp5eGr7Dz3fWGUpiSiG3d WgJe

[16] Chain Services with Exactly-Once Guarantees:   
https://www.confluent.io/blog/chain-services-exactly-guarantees/   
[17] Exponential backoff: https://en.wikipedia.org/wiki/Exponential backoff   
[18] Idempotence: https://en.wikipedia.org/wiki/ldempotence   
[19] Stripe idempotent requests: https://stripe.com/docs/api/idempotent requests   
[20] Idempotency: https://developer.paypal.com/docs/platforms/develop/idempotency/   
[211 Paxos: httos://en.wikipedia.ora/wiki/Paxos (comouter science)

[22] Raft: https://raftgithub.io/

lco] nuw rdyierit udieways can veteut dniu rrevert urme riduu:   
https://www.chargebee.com/blog/optimize-online-billing-stop-online-fraud/   
[27] Advanced Technologies for Detecting and Preventing Fraud at Uber:   
https://eng,uber.com/advanced-technologies-detecting=preventing-fraud-uber/   
[28] Re-Architecting Cash and Digital Wallet Payments for India with Uber Engineering:   
https://eng.uber.com/india-payments/   
[29] Scaling Airbnb's Payment Platform:   
https://medium.com/airbnb-engineering/scaling-airbnbs-payment-platform-43ebfc99b   
[30] Payments Integration at Uber: A Case Study:   
httns://www.voutube.com/watch?v=vooCE5B0SRA