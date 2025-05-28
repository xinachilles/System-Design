# Lab2

Created: 2023-09-24 12:24:42 -0600

Modified: 2023-09-25 12:12:50 -0600

---

![Lab 2 application requirements Functional requirements aws training and certification • Allow users to select a bookmark to be shared in the team's knowledge base • Forward information about submissions to managers for review • Register a contest entry for each new user submission Technical requirements • Use the AWS serverless stack • Use an event-driven approach for processing submissions • Use fan-out to perform parallel activities Developer tools • AWS Cloud9 IDE • AWS SAM and AWS SAM CLI @ 2021 Amazon Web Services, Inc. or its Affiliates All rights reserved. 12 ](../../../media/AWS-Developing-Serverless-Solutions-on-AWS-Module-6-Lab2-image1.png){width="5.0in" height="2.7291666666666665in"}





![Lab 2 objectives aws training and certification • Enable DynamoDB Streams as an event source for a Lambda function that is triggered when new items are added to your DynamoDB table • Configure an EventBridge event bus with a Lambda function as its event source and Lambda, Amazon SNS, and CloudWatch as targets • Configure EventBridge rules that route events to your targets based on criteria you specify • Configure an Amazon SNS topic that notifies an email subscriber • Use AWS SAM to deploy your updated application ](../../../media/AWS-Developing-Serverless-Solutions-on-AWS-Module-6-Lab2-image2.png){width="5.0in" height="2.375in"}



![Lab 2: Deployed application with bookmark sharing and message fan-out aWS training -y certificat Amazon Cognito User AWS Lambda (get function) AWS Lambda Amazon API Gateway (store function) Amazon DynamoDB AWS Lambda (stream function) Amazon EventBridge AWS Lambda (contest function) Amazon Simple Notification Service Amazon CloudWatch ](../../../media/AWS-Developing-Serverless-Solutions-on-AWS-Module-6-Lab2-image3.png){width="5.0in" height="2.8472222222222223in"}





![AWS Cloud HTML, CSS JavaScript •gn-up/sign-i Web user GET end POST API cells Amazon API Gateway Amazon Eventaridge Front-end stack UI (SPA) AWS Amplify Amazon Cognito Lab 2 GET API calls AWS Lambda get function AWS Lambda store function Payloa AWS Lambda stream function Contest AWS Lambda contest function Notify Amazon SNS notify user Catch-all Amazon CloudWatch - EventBridge AWS IAM AWS Cloudg AWS SAM Store Bookmark 2 If shared, create one more Submission for contest entry Amazon DynamoOB Bookmark Table Contest entered Catch-All Rule: All events where source is "DynamoD8 Streams". 2 Contest Rule: Shared is true and Contest entry is "Entering'% 3 Notify Rule: Notify user when Shared is true ](../../../media/AWS-Developing-Serverless-Solutions-on-AWS-Module-6-Lab2-image4.png){width="5.0in" height="5.048611111111111in"}






