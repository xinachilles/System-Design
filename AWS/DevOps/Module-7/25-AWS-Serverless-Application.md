# 25 AWS Serverless Application Repository and AWS SAM



---

Summary

The Serverless Application Repository and Serverless Application Model (SAM) provide a structured approach to building and deploying serverless applications on AWS, simplifying the process of managing serverless resources and enabling local testing and development.

Facts

- Serverless Application Repository:
  - Acts as a storage for serverless applications.
  - Allows hosting of applications either publicly or privately.
  - Facilitates easy use and download of serverless applications for later use.
- Serverless Application Model (SAM):
  - Provides a simplified way to build complete applications that run inside Lambda.
  - Enables packaging of applications for easy distribution.
  - Integrates directly with AWS code services.
  - Uses CloudFormation templates for deployment.
  - Supports local debugging and testing.
- SAM Template:
  - Similar to a CloudFormation template.
  - Specifies the type of application, code location, policies, handler, runtime, and APIs.
  - Can be used to create applications, Lambda function layers, DynamoDB tables, and APIs.
- Deploying with SAM:
  - Involves initializing a new application, building it, packaging, and deploying.
  - Can be done in just a few lines of code.
  - Allows for local testing and development without switching contexts.
  - Supports overriding environment variables for local testing.
- Benefits of SAM:
  - Reduces the complexity of interacting with infrastructure as code.
  - Eliminates the need for manual CloudFormation template creation.
  - Facilitates local testing, reducing the need for constant switching between development environments.
  - Integrates with other AWS services and tools for a seamless serverless application development experience.



![In this lesson, you are introduced to Serverless Application Repository and AWS Serverless Application Model (AWS SAM). Serverless Application Repository is a managed repository of serverless applications for common use cases and AWS SAM is an open-source framework used to build serverless applications. To watch the instructor video, choose the play button. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image1.png)



![Key concepts and topics Review the content below to reinforce some of the key concepts and topics presented to you in the video above. What is AWS Serverless Application Repository? The AWS Serverless Application Repository is a managed repository for serverless applications. It enables teams, organizations, and individual developers to store and share reusable applications, and easily assemble and deploy serverless architectures in powerful new ways. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image2.png)



![AWS Serverless Application Repository Find and deploy applications for common use cases Search and discover Browse or search the AWS Serverless Application Repository to find an application Configure Set environment variables, parameter values, and more before deploying the app Deploy and manage Deploy the app to your AWS account and manage it from the AWS Management Console ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image3.png)



![What is AWS Serverless Application Model (AWS SAM)? The AWS Serverless Application Model (AWS SAM) is an open-source framework that you can use to build serverless applications on AWS. Remember that a serverless application is a combination of Lambda functions, event sources, and other resources that work together to perform tasks. A serverless application is more than only a Lambda function---it can include additional resources such as API operations, databases, and event source mappings. You can use AWS SAM to define your serverless applications. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image4.png)



![SINGLE- DEPLOYMENT CONFIGURATI... EXTENSION OF BUILT-IN BEST AWS PRACTICES CLOUDFORMATI... LOCAL DEBUGGING AND TESTING WIT Using AWS SAM, you can organize related components and resources, and operate on a single stack. You can use AWS SAM to share configuration (such as memory and timeouts) between resources. You can also use it to deploy all related resources together as a single, versioned entity. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image5.png)



![SINGLE- DEPLOYMENT CONFIGURATI... EXTENSION OF BUILT-IN BEST AWS PRACTICES CLOUDFORMATI... LOCAL DEBUGGING AND TESTING WIT Because AWS SAM is an extension of AWS CloudFormation, you get the reliable deployment capabilities of AWS CloudFormation. You can define resources by using AWS CloudFormation in your AWS SAM template. Also, you can use the full suite of resources, intrinsic functions, and other template features that are available in AWS CloudFormation. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image6.png)



![SINGLE- DEPLOYMENT CONFIGURATI... EXTENSION OF BUILT-IN BEST AWS PRACTICES CLOUDFORMATI... LOCAL DEBUGGING AND TESTING WIT You can use AWS SAM to define and deploy your infrastructure as config. This makes it possible for you to use and enforce best practices such as code reviews. Also, with a few lines of configuration, you can enable safe deployments through AWS CodeDeploy and enable tracing by using AWS X-Ray. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image7.png)



![SINGLE- DEPLOYMENT CONFIGURATI... EXTENSION OF BUILT-IN BEST AWS PRACTICES CLOUDFORMATI... LOCAL DEBUGGING AND TESTING WIT The AWS SAM CLI lets you locally build, test, and debug serverless applications that are defined by AWS SAM templates. The CLI provides a Lambda-like runtime environment locally. It helps you catch issues upfront by providing parity with the actual Lambda runtime environment. To step through and debug your code to understand what the code is doing, you can use AWS SAM with AWS toolkits, such as: • AWS Toolkit for JetBrains • AWS Toolkit for PyCharm • AWS Toolkit for IntelliJ • AWS Toolkit for Visual Studio Code This tightens the feedback loop by making it possible for you to find and troubleshoot issues that you might run into in the cloud. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image8.png)



![EXTENSION OF BUILT-IN BEST AWS PRACTICES CLOUDFORMATI... LOCAL DEBUGGING AND TESTING DEEP INTEGRATION WITH DEVELOP... You can use AWS SAM with a suite of AWS tools for building serverless applications. You can discover new applications in the AWS Serverless Application Repository. For authoring, testing, and debugging AWS SAM based serverless applications, you can use the AWS Cloud9 IDE. To build a deployment pipeline for your serverless applications, you can use CodeBuild, CodeDeploy, and CodePipeline. You can also use AWS CodeStar to get started with a project structure, code repository, and a CI/CD pipeline that's automatically configured for you. To deploy your serverless application, you can use the Jenkins plugin. To build production-ready applications, use the Stackery.io toolkit. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image9.png)



![AWS SAM template AWSTemp1 ateForma t Ver s ion. • '2010-09-09' Transform: AWS: : server less---2016---10---31 Resources : GetHtm1Funct ion : : Server less: : Function Type: AWS: Properties : s 3: / list. zip CodeUri : index. gethtml Handler : Runt ime : nodejs6.10 Policies : Amazon DynamoDBReadOn1yAccess Events : GetHtm1 : Type: Api Properties : Path: / {proxy+} Method: ANY ListTab1e : Type: AWS: : Server less: : SimpleTab1e ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image10.png)



![When you create a serverless application by using AWS SAM, your main objective is to construct an AWS SAM template file that represents the architecture of your serverless application. The AWS SAM template file is a YAML or JSON configuration file that adheres to the open-source AWS Serverless Application Model specification. You use the template to declare all the AWS resources that comprise your serverless application. AWS SAM templates are an extension of AWS CloudFormation templates. That is, any resource that you can declare in an AWS CloudFormation template you can also declare in an AWS SAM template. The benefit of using AWS SAM compared to a cfn-template is that with a smaller number of lines of AWS SAM syntax, you can create multiple resources. AWS SAM enables you to streamline the process. ](../../../media/AWS-DevOps-Module-7-25-AWS-Serverless-Application-Repository-and-AWS-SAM-image11.png)













