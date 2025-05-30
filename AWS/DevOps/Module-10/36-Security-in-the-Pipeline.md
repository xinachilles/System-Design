# 36 Security in the Pipeline



---

Summary

Security within the CI/CD pipeline is crucial, and there are various steps and tools available to ensure that code and infrastructure adhere to best practices and security standards.

Facts

- Emphasis on the distinction between security of the pipeline and security in the pipeline.
- Automated testing is essential, and security should be considered a vital test.
- EC2 instances can be tested for compliance with best practices using tools like AWS Inspector.
- Containers should undergo similar security checks to ensure they meet best practice standards.
- CodeBuild can be used to check for unauthorized packages or plugins during the build process.
- Static code analysis and dependency checks can identify potential security issues.
- Infrastructure as Code (IaC) tools like CloudFormation and Terraform can be checked for security compliance.
- Multiple stages of the pipeline offer opportunities to integrate security checks, ensuring robust testing and compliance.



![Key concepts and topics Review the content below to reinforce some of the key concepts and topics presented to you in the video above. Security testing in the pipeline examples: AMIs To learn more, choose each hotspot. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image1.png)



![10 Source My App Source CodeCommit BuildAMl AMIBuilder CodeBuild SecuritvTests LaunchTestlnstance Lambda TestAMl Lambda KillTestlnstance Lambda ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image2.png)



![Trigger pipeline on source change. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image3.png)



![Build AMI. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image4.png)



![3 Launch test instance with AMI. Notice that it is triggered with Lambda. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image5.png)



![4 Trigger Amazon Inspector. Lambda also triggers Inspector which will perform an assessment again the instance (it will check for security requirements such as open SSL or kernel, they are numerous different packages that can be ran). ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image6.png)



![If all the tests succeed, then a final Lambda function is triggered to kill test instance. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image7.png)



![Security testing in the pipeline examples: Docker To learn more, choose each hotspot. Source My App Source ECR Securit Tests TriaaerClair Lambda Tests Inte rationDe 10 CodeDe 10 TestonChrome CodeBuild TestOnFireFox CodeBuild InteaTest End2EndTester ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image8.png)



![1 Trigger pipeline on source change. Here you'll notice that the Source is ECR, so when a new container gets placed into the repository that triggers the pipeline. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image9.png)



![01 Irre 2 Trigger security test against the container. At this step, you have a Lambda function that triggers Clair (open source container tool), and that continues to the next steps. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image10.png)



![3 Deploy to integration environment. The tests pass and then the next step is to deploy to the integration environment and then you notice that the integration tests were triggered. ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image11.png)



![Execute UI tests. uil ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image12.png)



![Security testing in the pipeline examples: AWS CodeBuild To learn more, choose each hotspot. Source My App Source CodeCommit Securi Tests ComolianceTest CodeBuild De 10 ToSta in Deploy CodeDe 10 Sample security test script for i in •cat requirements. txt* ; do grep -q $i whitelist.txt if [ [ $ ) • then echo "Security test failed: unauthorized package in requirements. txt" exit 1 done ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image13.png)



![1 Trigger pipeline on source change. Notice here it is in CodeCommit. in ep ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image14.png)



![Run tests in CodeBuild. in ep ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image15.png)



![Deploy. in echc exil ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image16.png)









![Looking at code analysis and dependencies • Using AWS CodeBuild: • WhiteSource integration to scan source repository • Using Amazon CloudFormation: training and -y certification • Static analysis on CloudFormation templates -Y checking your Infrastructure as Code • Vulnerability scans in your pipelines • Golden AMIs, and scanning using Trivy or Clair ](../../../media/AWS-DevOps-Module-10-36-Security-in-the-Pipeline-image17.png)



















