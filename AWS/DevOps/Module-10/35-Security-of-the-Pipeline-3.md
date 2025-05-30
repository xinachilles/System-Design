# 35 Security of the Pipeline-3



---

Summary

AWS Config provides insights into account compliance and best practices, while AWS Secrets Manager and Systems Manager Parameter Store offer secure solutions for credential storage and management, emphasizing the importance of security in the CI/CD pipeline.

Facts

- AWS Config:
  - Provides insights into resource compliance and best practices within an AWS account.
  - More details on Config will be covered in the upcoming module on configuration management.
- AWS Secrets Manager:
  - Safely manages and encrypts credentials.
  - Requires IAM access to retrieve and use







![AWS Secrets Manager training and certification Helps you protect secrets needed to access your applications, services, and IT resources • Rotate secrets safely without the need for code deployments Use: • • IAM roles instead of Access Key ID and Secret Access Key. • AWS Key Management Service (AWS KMS) for secure storage and handling of key pairs. • Use some form of host-level encryption to avoid storing plaintext credentials on instances. O An-won web or Al ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image1.png)



![With security of the pipeline, what should you focus on? User management Least privilege Detective controls Infrastructure controls ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image2.png)



![IAM policy example The following shows an example of a permissions policy that allows a user to enable and disable all stage transitions in the pipeline named MyFirstPipeline in the us-west-2 region. "Version "2012-10-17" , " Statement " . "Effect" : " Allow" , " Action " " codepipeline : EnableStageTransition " , " codep ipeline : DisableStageTransition " " Resource " " arn : aws : codepipeline : : 111222 3334 44 : MyFirstPipe1ine " ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image3.png)



![Restrict AWS CodeBuild project actions based on resource tags The below example denies mentioned list of actions on projects tagged with the key Environment with the key value of Production. A user's administrator must attach this IAM policy in addition to the managed user policy to unauthorized IAM users. The aws:ResourceTag condition key is used to control access to resources based on their tags. "Version : ' " '2012-10-17" , " Statement " : [ "Effect " : "Deny" " Action " : [ " codebuild : CreateProject " , " codebuild : UpdateProject " , " codebuild : DeleteProj ect " "Resource "Condition " : { " ForAnyVa1ue : StringEqua1s " : { aws : RequestTag/Environment " : " Production " ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image4.png)



![](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image5.png)



![Switching roles example If you need to push a new version of an application to the production environment: 0 Switch roles to get deployment permissions in the production environment Perform the tasks Switch back to your normal user account ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image6.png)



![Multi-factor authentication (MFA) MFA requires users to enter a unique authentication code when accessing AWS website or services. It can be used for CLI-, API-, and console-based access. It also acts as an additional layer of security. MFA is strongly recommended for key users and it is recommended to assign a unique MFA device (hardware or virtual) to each IAM user. Possible MFA mechanisms listed below. Do you use any of these? Virtual MFA device IJ2F security key Hardware MFA device SMS text message-based MFA ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image7.png)



![](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image8.png)



![Pipeline security It is important to change to the mindset of treating the pipeline as a resource. It is useful to look at the security of your pipeline, and here are some areas to consider: • Who can commit • Who can build • Who deploys to test/production The Security Perspective of the Cloud Adoption Framework The Security Cloud Adoption Framework (CAF) whitepaper provides prescriptive controls to improve the security posture of your AWS accounts. To embed the DevSecOps discipline in the enterprise, AWS customers are automating CAF controls using a combination of AWS and third- party solutions. To learn more, flip each flash card. ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image9.png)



![To learn more, flip each flash card. Directive Preventive Responsive Detective ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image10.png)



![Establish the governance, risk, and compliance models on AWS Protect your workloads and mitigate threats and vulnerabilities Drive remediation of potential deviations from your security baselines Provide full visibility and transparency over the operation of your deployments in AWS ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image11.png)



![Benefits of using the Parameter Store training and certification Secure, hierarchical storage for configuration data management and secrets management • Use secrets management service with no servers to manage • Separate data from code • Store configuration data and secure strings in versions and hierarchies • Control and audit access at granular level O web or Al ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image12.png)



![Best practices for creating parameters in a Parameter Store training and -7 certification • Develop a naming system for easier access, uniformity, and long-term maintenance • Create a naming system that follows this pattern to identify key information: • (type of configuration data)-(computing environment)-(region) • For example: License-manager-PROD-uswest1 ](../../../media/AWS-DevOps-Module-10-35-Security-of-the-Pipeline-3-image13.png)















