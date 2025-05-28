# 24 AWS Lambda and AWS Fargate

Created: 2023-10-08 11:20:33 -0600

Modified: 2023-10-22 17:42:22 -0600

---

Summary

Serverless computing, exemplified by AWS services like Lambda and Fargate, allows developers to focus on code while AWS manages the underlying infrastructure. This approach simplifies tasks such as scaling, maintenance, and patching, and integrates seamlessly with various AWS services and tools.

Facts

- Serverless Computing: Developers only focus on code; AWS manages the infrastructure.
- Lambda:
  - Does not require managing the OS or physical hardware.
  - Automatically scales based on the number of incoming requests.
  - Functions are short-lived, with a maximum execution time of 15 minutes.
  - Supports multiple runtimes including NodeJS, Python, Java, and more.
  - Functions are triggered by events, and AWS provides built-in monitoring via CloudWatch.
  - Lambda functions require permissions to be triggered and to interact with other AWS services.
  - Two invocation patterns: push (event-driven) and pull (Lambda polls a data source).
- Fargate:
  - Offers serverless compute for containers.
  - Suitable for longer-running tasks compared to Lambda.
  - Works in conjunction with other AWS services for tasks like storing code and building images.
- Serverless Application Model (SAM): A tool for building serverless applications, featuring a simplified template syntax.
- Serverless Storage and Communication: AWS offers services like API Gateway, AppSync, SQS, and SNS to facilitate storage and communication in a serverless architecture.
- Developer Tooling: AWS provides tools like Cloud9 and plugins for IDEs to aid in the development of serverless applications.
- Lambda Function Components:
  - Function: The actual resource running the code.
  - Runtime: The environment in which the code runs.
  - Event: The JSON document passed into the Lambda function.
  - Concurrency: Number of functions running in parallel.
  - Trigger: What initiates the Lambda function.
  - Layers: Reusable pieces of code or data.
- Use Cases: Lambda can be used for data processing, IoT architectures, IT automation, and more.



- ![Common use cases Data processing Real time . Amazon EMR Batch • Real-time stream processing Activity tracking Metrics generation IOT device data telemetry and metering Backends Applications and services Mobile 10T Third-party API requests • • training and -7 certification IT automation Policy engines Extending AWS services Infrastructure management 0 2020 Ama.•on ','.'eb Inc O' Its Al re-sewed ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image1.png){width="5.0in" height="2.826388888888889in"}



- CI/CD with Serverless: Serverless integrates with CI/CD pipelines, allowing for efficient code testing, building, and deployment.





![Serverless application development enables you to build loosely coupled applications, with increased agility and lower total cost of ownership. AWS Lambda and AWS Fargate are compute services for serverless applications. ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image2.png){width="5.0in" height="0.9236111111111112in"}



![Key concepts and topics Review the content below to reinforce some of the key concepts and topics presented to you in the video above. Why serverless? Serverless enables you to build modern applications with increased agility and lower total cost of ownership. Building serverless applications means that your developers can focus on their core product. They don't have to worry about managing and operating servers or runtimes, either in the cloud or on premises. This reduced overhead lets developers reclaim time and energy that can be spent on developing products that scale and are reliable. Another advantage, is the ability to bring your own code. ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image3.png){width="5.0in" height="2.6944444444444446in"}



![AWS Serverless Platform Compute AWS Lambda AWS Fargate Storage and data stores Amazon S3 Amazon EFS Amazon DynamoDB Amazon Aurora Serverless Integration and orchestration API Gateway 0+Ä+0 Amazon SQS Amazon SNS AWS AppSync Amazon EventBridae AWS Step Functions Analytics Amazon Kinesis Data Firehose Amazon Athena Developer tooling AWS SAM AWS Developer Tools SDK AWS Amplify ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image4.png){width="5.0in" height="2.7083333333333335in"}



![AWS Lambda AWS Lambda is the compute service for serverless. You can run code without servers, it triggers based on events that you configure, and it scales automatically based on demand. Lambda also incorporates built-in Amazon CloudWatch for monitoring and logging, so you have key metrics readily available. Lambda is closely integrated with other AWS services. From within your Lambda function, you can do anything your traditional application can do, including calling an AWS SDK or invoking a third- party API, whether on AWS, in your data center, or on the internet. Lambda's permissions model uses AWS Identity and Access Management (IAM) to securely grant access to AWS resources and give you fine-grained control for invoking your functions. Because Lambda is a fully managed service, high availability and fault tolerance are built into the service without any additional configuration on your part. ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image5.png){width="5.0in" height="3.2083333333333335in"}



![Key terms for AWS Lambda Function A function is a resource that you can invoke to run your code in AWS Lambda. Runtime Lambda runtimes allow functions in different languages to run in the same base runtime environment. You configure your function to use a runtime that matches your programming language. The runtime sits between the Lambda service and your function code, relaying invocation events, context information, and responses between the two. You can use runtimes provided by Lambda, or build your own. ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image6.png){width="5.0in" height="3.423611111111111in"}



![Event An event is a JSON formatted document that contains data for a function to process. The Lambda runtime converts the event to an object and passes it to your function code. When you invoke a function, you determine the structure and contents of the event. When an AWS service invokes your function, the service defines the event. Concurrency Concurrency is the number of requests that your function is serving at any given time. When your function is invoked, Lambda provisions an instance of it to process the event. When the function code finishes running, it can handle another request. If the function is invoked again while a request is still being processed, another instance is provisioned, increasing the function's concurrency. ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image7.png){width="5.0in" height="3.5277777777777777in"}



![Layers You can configure your Lambda function to pull in additional code and content in the form of layers. A layer is a ZIP archive that contains libraries, a custom runtime, or other dependencies. With layers, you can use libraries in your function without needing to include them in your deployment package. Trigger A trigger is a resource or configuration that invokes a Lambda function. This includes AWS services that can be configured to invoke a function, applications that you develop, and event source mappings. ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image8.png){width="5.0in" height="3.2916666666666665in"}



![Common use cases Data processing: You can use AWS Lambda to run code in response to triggers, such as changes in data, shifts in system state, or user actions. Lambda can be directly triggered by AWS services, such as Amazon S3, Amazon DynamoDB, Amazon Kinesis, Amazon Simple Notification Service (Amazon SNS), and Amazon CloudWatch, or it can be orchestrated into workflows by AWS Step Functions. This enables you to build a variety of real-time serverless data processing systems. Real-time stream processing: You can use AWS Lambda and Amazon Kinesis to process real-time streaming data for application activity tracking, transaction order processing, click stream analysis, data cleansing, metrics generation, log filtering, indexing, social media analysis, and IOT device data telemetry and metering. Backends: You can build serverless backends using AWS Lambda to handle web, mobile, Internet of Things (IOT), and third-party API requests. IT automation: AWS Lambda can be used for compliance and to create policies that invoke additional AWS services. ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image9.png){width="5.0in" height="3.5208333333333335in"}



![Serverless architecture examples O Object 00+p Data Message 1. File put into bucket 1. Data published to a topic I. Message inserted into a queue S3 bucket SNS topic Amazon Simple Queue Service 2. Lambda invoked 2. Lambda invoked 2. Lambda polls queue and invokes function Lambda function Lambda function Lambda function 3. Function Removes message from queue ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image10.png){width="5.0in" height="3.048611111111111in"}



![Amazon S3 This example shows an object, such as a photo, video, or document. It then goes into the Amazon S3 bucket. The S3 bucket then invokes the Lambda function, which enables you to do whatever you choose to do with this data. Some possibilities include transforming, or transcoding, resize it or using it for machine learning. amb Oked amb Oked ue ar ction ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image11.png){width="5.0in" height="4.3125in"}



![Amazon SNS Using Amazon SNS, the messages go into topics, and then those messages, on your behalf, invoke a Lambda function. I. Message Lambd Oked ambd Oked 2. Lambd ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image12.png){width="5.0in" height="2.8125in"}



![Amazon SQS Finally, with Amazon Simple Queue Service (Amazon SQS), similar to Amazon SNS, you would put that message into the queue, Lambda then polls the queue, and then invokes a Lambda function. This is common with scaling up and scaling down. ambda Oked ambda Oked mbda ue and ction ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image13.png){width="5.0in" height="3.5416666666666665in"}



first one is s3 notification function

3rd is pull model



![Serverless computing with AWS Fargate example In this architecture example below, we show development with Bitbucket, an open-source tool that runs on AWS services. Some other possible options include AWS CodeCommit and GitHub. With AWS Fargate, organizations can take advantage of the serverless computing architecture to implement applications with convenience, leaving behind the need to manage clusters and enabling seamless scaling of the applications. g Bitbucket Developer push Application repo or tag Unit tests run Regression tests run Acceptance test analysis Python connector script Orchestrator repo Clone the application code Build the docker image Push the image to ECR Clone terraform code Run terraform code Deploy image to Ear.gate Run load tests Amazon Elastic Container Registry AWS Fargate ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image14.png){width="5.0in" height="3.576388888888889in"}



![Example: Serverless architecture for a training and -7 certification web application 53 Amazon Route 53 Amazon SDK Amazon CloudFront 0 Arva.•on Weh Inc O' Al. rescued AWS Region CACHE Amazon Web Services API Gateway cache Amazon DynamoDB Amazon API Gateway AWS Lambda Your static website HTML, CSS, JavaScript, media Files Amazon S3 O•Ä+O Amazon SQS Amazon SNS Amazon SS Amazon Kinesis • ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image15.png){width="5.0in" height="2.8125in"}



![Serverless computing AWS Lambda Server-less event---driven code execution Short-lived All language runtimes Optimized for stateless 0 Ama.•on Weh Inc O' Al rese-ved training and certification AWS Fargate Serverless compute engine for containers Use existing code Fully managed orchestration with Amazon ECS ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image16.png){width="5.0in" height="2.875in"}



![AWS Lambda functions Creating a function involves: • Defining permissions • Specifying what events trigger your function training and -y certification • Providing the code, dependencies, and libraries needed to execute your code • Configuring execution parameters • Memory, timeout, and concurrency ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image17.png){width="5.0in" height="2.201388888888889in"}



![Lambda function: Permissions IAM resource policy/function policy aws -7 Amazon DynamoDB training and certification IAM execution role Amazon S3 Triggering event Lambda function Two sides to the access permission: • IAM resource policy/function policy • To invoke the function • IAM execution role • What the function is permitted to do 0 2020 Am-von Web Senores, Inc O' Afflltates Al ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image18.png){width="5.0in" height="2.7291666666666665in"}





![Example: Lambda function in Node.js training and -y certification Object Amazon S3 Bucket with objects AWS Lambda Function handler "Records" : "object" : " 0 2020 Amazon Web Services, Inc. or its Affiliates. All rights reserved. exports. context, callback) { console. log(' Event: + JSON.stri ngify (event)) callback(null, 'Hello World! ') ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image19.png){width="5.0in" height="2.7777777777777777in"}



![Invocation patterns There are two models for invoking a function: training and -7 certification • Push model - Lambda function is invoked every time a particular event occurs with another service • Pull model - Lambda polls a data source and invokes a function with any new records that arrive at the data source 0 2020 Ama.•on Web Inc O' A. re-sewed ](../../../media/AWS-DevOps-Module-7-24-AWS-Lambda-and-AWS-Fargate-image20.png){width="5.0in" height="2.7708333333333335in"}




















