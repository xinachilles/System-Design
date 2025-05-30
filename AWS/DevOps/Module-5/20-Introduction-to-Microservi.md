# 20 Introduction to Microservices



---

Summary

The text delves into the concept of microservices, emphasizing their benefits, characteristics, and the transition from monolithic to microservice architectures, while also highlighting the importance of automation, decentralization, and the role of APIs in microservice communication.

Facts

- Microservices represent the smallest parts of an application, functioning like an assembly line.
- They are defined as small, language-agnostic independent services that operate over well-defined APIs.
- Microservices are like black boxes: they take input, process it, and produce output without revealing the internal workings.
- They are beneficial for DevOps pipelines due to their small size and clear ownership.
- Monolithic applications face challenges in scaling and deployment, whereas microservices alleviate these issues.
- Microservices are decentralized, fail independently, and are automated for deployment.
- Transitioning from monolithic to microservice architectures should start small and simple to avoid overwhelming the process.
- AWS offers various services, such as Elastic Beanstalk, SNS, SQS, and managed databases, to support microservice architectures.
- APIs play a crucial role in microservices, allowing for rapid changes without affecting end users.



![](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image1.png)



![Key concepts and topics Review the content below to reinforce some of the key concepts and topics presented to you in the video above. Characteristics of microservice architectures Decentralized Independent Black box Polyglot Do one thing well You build it, you run it ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image2.png)



![Benefits of microservices Microservices architectures make applications easier to scale and faster to develop. This enables innovation and accelerates time-to-market for new features. Containers also provide isolation and packaging for software, and help you achieve more deployment velocity and resource density. To learn more on the benefits of microservices, flip each card. ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image3.png)



![Scalability Rapid build/test/releas e cycles Easier to maintain and evolve Clear ownership and accountability Increased agility Resilience ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image4.png)



![Each service can independently scale to meet application demand. Enables rapid build/test/release cycles as it does not impact entire architecture. Microservices enable CI/CD. The low cost of failure enables experimentation and rollback in case of failure. Fosters an organization of small, independent teams that take the ownership of their services. Teams can act within a small well understood context and are empowered to work quickly and move independently Service independence increases an application's resistance to failure ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image5.png)



![APIs are the frontdoor of microservices APIs are set of tools, features, or data that allows the creation of a process, service or application that can assist you with microservices. APIs serve as the entry point for the application's logic behind a set of programmatic interfaces. The API accepts and processes calls from clients and might implement functionality such as traffic management, request filtering, routing, caching, authentication, and authorization. Developing microservices using AWS Microservice architecture is supported through various AWS services. To learn more about each type of AWS services flip the card. ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image6.png)



![Compute ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image7.png)



![Amazon EC2 - Infrastructure as a Service AWS Lambda - Serverless functions AWS Elastic Beanstalk - Orchestration service ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image8.png)



![Containers ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image9.png)



![AWS Fargate - Serverless containers Amazon ECS / Amazon EKS - Container orchestration ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image10.png)



![Integration ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image11.png)



![AWS Step Functions - Coordination for distributed applications ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image12.png)



![Messaging/Queues ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image13.png)



![Amazon Simple Notification Service (Amazon SNS) Amazon Simple Queue Server (Amazon SQS) ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image14.png)



![Database ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image15.png)



![Amazon DynamoDB - Serverless NoSQL database ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image16.png)



![Networking ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image17.png)



![• Amazon API Gateway - API Proxy • Application Load Balancer / Network Load Balancer - Elastic load balancing • AWS Cloud Map - Service Discovery • AWS App Mesh - Service Mesh ](../../../media/AWS-DevOps-Module-5-20-Introduction-to-Microservices-image18.png)




















