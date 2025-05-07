# 32 Introduction to DevSecOps

Created: 2023-10-11 20:21:06 -0600

Modified: 2023-10-22 17:44:36 -0600

---

Summary

DevSecOps emphasizes the integration of security practices within the CI/CD pipeline, ensuring that security measures are seamlessly incorporated throughout the development process, preventing vulnerabilities and ensuring business continuity.

Facts

- DevSecOps Concept:
  - Integrates security into the CI/CD pipeline.
  - Addresses the traditional conflict between developers' need for speed and operations' need for stability.
  - Aims to prevent security from being an afterthought or overlooked.
- Importance of DevSecOps:
  - Ensures businesses remain secure and operational.
  - Security checks and tests are integrated throughout the pipeline.
  - Makes security a community effort, removing the sole responsibility from a single "security czar."
- Principles of DevSecOps:
  - Requires buy-in from all stakeholders.
  - Emphasizes infrastructure as code and continuous feedback.
  - Focuses on both security of the pipeline and security in the pipeline.
  - Utilizes AWS managed architecture and standardized IAM permission sets for enhanced security.
- Security in the CI/CD Pipeline:
  - Scanning for secrets prevents sensitive data from being committed to repositories.
  - Services like GET Secrets help in keeping access keys and passwords secure.
  - Tools like Inspector ensure correct package versions on EC2 instances.
  - Monitoring and feedback tools include Config, SecurityHub, and GuardDuty.
- Example of a CI/CD Pipeline with Security:
  - Artifacts are placed in an S3 bucket, triggering the pipeline.
  - Static code analysis checks for potential vulnerabilities before deployment.
  - Automated and manual security tests can be run before moving to the next stage.
  - Initial test environments are deleted after validation, ensuring resources are used efficiently.
- Benefits of DevSecOps in CI/CD:
  - Matches the speed of development without compromising security.
  - Eliminates the need for lengthy, separate security audits.
  - Integrates security checks as part of the regular pipeline process.





![DevSecOps is certainly about security, but it is just as much about the processes you use to build applications and helping to ensure security is built in to those processes by design. In this lesson, you learn how to implement DevSecOps. ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image1.png){width="10.083333333333334in" height="2.1666666666666665in"}



![Key concepts and topics Review the content below to reinforce some of the key concepts and topics presented to you in the video above. ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image2.png){width="10.083333333333334in" height="2.09375in"}



![Development Build It Faster What is DevSecOps? Business Security Keep It Secure Operations Keep It Stable ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image3.png){width="10.083333333333334in" height="6.854166666666667in"}



![Think of DevSecOps as putting security in the middle of Dev/_/Ops. Cloud security and compliance is a shared responsibility between AWS and the customer. As the cloud provider, AWS is responsible for security OF the cloud, including the hypervisor and hardware. Customers are responsible for security IN the cloud, by securing your applications and network with services from third-party vendors. DevSecOps in the cloud takes that approach to security and builds it into your development and operational processes. ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image4.png){width="10.083333333333334in" height="3.46875in"}



![Why does DevSecOps matter? Delivery pipeline Build Test Security Release Developers Plan Monitor Feedback loop Customers DevOps = Efficiencies that speed up the lifecycle DevSecOps = Validate building blocks without slowing lifecycle An image of the DevSecOps pipeline. DevOps equals efficiencies that speed up the lifecycle. DevSecOps equals validating building blocks without slowing lifecycle. ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image5.png){width="10.083333333333334in" height="5.958333333333333in"}



![DevSecOps principles To learn more on DevSecOps principles, flip each of the flashcard. Principle 1 Principle 4 Principle 2 Principle 5 Principle 3 Principle 6 ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image6.png){width="10.083333333333334in" height="7.770833333333333in"}



![DevSecOps principles To learn more on DevSecOps principles, flip each of the flashcard. Collaborate with all stakeholders Automate everything Codify everything Measure and monitor everything Test everything Deliver business value with continual feedback ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image7.png){width="10.083333333333334in" height="7.8125in"}





![](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image8.png){width="10.083333333333334in" height="9.010416666666666in"}





![DevSecOps is automated, continuous, & visible Code Aws CodeCommit Scan for secrets Build Test Deploy Provision AWS CodeBui1d AWS CodePipelin Tag security artifacts Test security meets standards AWS CodeDeploy AWS CloudFormation Deploy/register security components 0 Ama•on Web Inc O' Its AIL rese-ued training and -y certification Monitor Amazon CloudWatch AWS Config Monitor security standards 8 ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image9.png){width="10.083333333333334in" height="5.84375in"}







![Pipeline example with hig security checks h ---level training and -y certification DevOps 4. Static code analysis Lambda Commit Stage Amazon S3 5. Create stack CloudFormation AWS CodePipeline Stack validation Lambda Approve test stack Delete stack CloudFormation Test Stage 0 2020 Amazon Web Services, Inc. or its Affiliates. All rights reserved. 6. Create ChangeSet Execute ChangeSet CloudFormation CloudFormation Production Stage 9 ](../../../media/AWS-DevOps-Module-10-32-Introduction-to-DevSecOps-image10.png){width="10.083333333333334in" height="5.583333333333333in"}
















