# 2 Introduction to Infrastructure Automation

Created: 2023-10-02 15:37:03 -0600

Modified: 2023-10-22 17:40:46 -0600

---



Summary

Infrastructure automation is crucial for achieving consistency, efficiency, and traceability in cloud environments. This involves using tools like AWS CloudFormation to define and deploy infrastructure as code.

Facts

- Infrastructure automation is essential for building and managing CI/CD pipelines and ensuring that manual changes are minimized.
- Automation can be achieved through methods like shell scripting, SDKs, and third-party tools, but AWS CloudFormation is a focus in this context.
- Infrastructure as code (IaC) allows developers to define and version their architecture, providing a history of changes and the ability to recreate environments



![O Benefits of infrastructure as code Infrastructure can be: • Versioned and managed just like application source code. • Repeatedly and reliably created, turned off, and re-created. • Created as needed to test the latest version of your application. Infrastructure as code allows: • Creation of multiple environments. • Creation of identical environment for multiple customers. ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image1.png){width="10.083333333333334in" height="5.520833333333333in"}



![Code infrastructure Code your infrastructure from scratch with the Clou&ormation template language, in either YAML or JSON format, or start from many available sample templates Amazon S3 Check out your template code locally, or upload it into an S3 bucket AWS CloudFormation Use AWS CloudFormation via the browser console, command line tools or APIs to create a stack based on your template code Output AWS CloudFormation provisions and configures the stacks and resources you specified on your template ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image2.png){width="10.083333333333334in" height="3.6666666666666665in"}



![YAML AWSTemp1 ate Fo rmatVe rsi on : Descri pti on : stri ng Parameters : set of parameters Mappi ngs : set of mappings Condi ti ons : set of conditions Transform: set of transforms Resources : set of resources Outputs : set of outputs "2020-01-09" ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image3.png){width="10.083333333333334in" height="8.302083333333334in"}



![Format Version Format Version (optional): Corresponding AWS CloudFormation template version ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image4.png){width="10.083333333333334in" height="3.7083333333333335in"}



![Description Description (optional): A text string ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image5.png){width="10.083333333333334in" height="3.1666666666666665in"}



![Parameters Parameters (optional): Inputs into template ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image6.png){width="10.083333333333334in" height="3.40625in"}



![Mappings Mappings (optional): Static variables; key-value pairs ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image7.png){width="10.083333333333334in" height="4.28125in"}



![Conditions Conditions (optional): Controls for if and when certain resources are created or updated ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image8.png){width="10.083333333333334in" height="3.875in"}



![Transform Transform (optional): Specifies the version of AWS SAM to use ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image9.png){width="10.083333333333334in" height="4.302083333333333in"}



![Resources Resources (required): AWS assets to create ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image10.png){width="10.083333333333334in" height="3.4375in"}



![Outputs Outputs (optional): Values of custom resources created by template (URLs, username, etc.) ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image11.png){width="10.083333333333334in" height="3.7604166666666665in"}



![Templates A CloudFormation template is a JSON or YAML formatted text file. You can save these files with any extension, such as .json, .yaml, .template, or .txt. CloudFormation uses these templates as blueprints for building your AWS resources. For example, in a template, you can describe an Amazon EC2 instance, such as the instance type, the AMI ID, block device mappings, and its Amazon EC2 key pair name. Whenever you create a stack, you also specify a template that CloudFormation uses to create whatever you described in the template. For example, if you created a stack with the following template, CloudFormation provisions an instance with an ami-Off8a91507f77f867 AMI ID, t2.micro instance type, testkey key pair name, and an Amazon EBS volume. ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image12.png){width="10.083333333333334in" height="5.46875in"}



![JSON " AWSTemp1ateFormatVersion : " " 2010-09-09 "Description " • "A sample template" , " Resources ' " MyEC2 Instance " : " Type • ". "AWS: :EC2: " Properties ' ' Imageld • : Instance' ". "ami-Off8a91507f77f867" , ' InstanceType ' " t 2 . micro " " KeyName ": " testkey " ' BlockDeviceMappings ' 'DeviceName ": " / dev/sdm" , "Ebs " : " Vo lumeType ' ' lops ' 200, 'iol " , " DeleteOnTermination " : " Vo lumeSize' 20 false, ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image13.png){width="10.083333333333334in" height="8.895833333333334in"}



![YAML AWSTemp1ateFormatVersion: 2010---09---09 Description: A sample template Resources : MyEC2 Instance : Type: ' AWS: : EC2 : : Instance ' Properties : Imageld: ami---0ff8a91507f77f867 InstanceType: t 2 . micro KeyName: testkey BlockDeviceMappings : --- DeviceName: / dev/sdm Ebs : VolumeType: iol lops: 200 DeleteOnTermination : VolumeSize: 20 false ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image14.png){width="10.083333333333334in" height="6.666666666666667in"}



![The previous templates are centered around a single Amazon EC2 instance; however, CloudFormation templates have additional capabilities that you can use to build complex sets of resources and reuse those templates in multiple contexts. For example, you can add input parameters whose values are specified when you create a CloudFormation stacl<. In other words, you can specify a value like the instance type when you create a stack instead of when you create the template, making the template easier to reuse in different situations. For more information about template creation and capabilities, see Template anatomy. ](../../../media/AWS-DevOps-Module-2-2-Introduction-to-Infrastructure-Automation-image15.png){width="10.083333333333334in" height="3.9166666666666665in"}







Template anatomy:

<https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html>















