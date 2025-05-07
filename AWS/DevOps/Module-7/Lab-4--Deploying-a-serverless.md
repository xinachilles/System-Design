# Lab 4: Deploying a serverless application using AWS Serverless Application Model (AWS SAM) and a CI/CD pipeline | Self-Paced Labs

Created: 2023-10-08 05:19:55 -0600

Modified: 2023-10-08 11:22:00 -0600

---

Clipped from: <https://labs.skillbuilder.aws/sa/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2FILT-TF-200-DEVOPS-3%2Flab-4-SAM%3A3.4.5-9be8fc07/en-US>

# Lab 4: Deploying a Serverless Application Using AWS Serverless Application Model (AWS SAM) and a CI/CD Pipeline

© 2023 Amazon Web Services, Inc. or its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited. All trademarks are the property of their owners.

Note: Do not include any personal, identifying, or confidential information into the lab environment. Information entered may be visible to others.

Corrections, feedback, or other questions? Contact us at *[AWS Training and Certification](https://support.aws.amazon.com/#/contacts/aws-training).*

## Lab overview

In this lab, you use the AWS Serverless Application Model (AWS SAM) to publish an application. You automate your deployment by leveraging AWS CodeDeploy and AWS CodePipeline. Lastly you configure your application deployment to support a traffic shifting deployment model. Traffic shifting allows for a gradual rollout of a new application deployment.

When a DevOps team is maintaining an application, the traffic shifting deployment model is one method for releasing application updates that minimizes interruptions to the customer experience and provides rapid rollback capability. Automating a traffic shifting deployment model increases a DevOps team's delivery speed, supporting the concept of Continuous Integration and Continuous Deployment (CI/CD). AWS offers a suite of developer tools to support CI/CD implementations.

For this lab, you leverage the AWS Serverless Application Model command line interface (AWS SAM CLI) to build a serverless application on AWS Lambda. You then implement and automate a traffic shifting deployment model for your application using AWS CodeDeploy and AWS CodePipeline.

### Objectives

By the end of this lab, you will be able to do the following:

- Build and locally test your application.
- Package and deploy your application.
- Automate the CI/CD pipeline to support a traffic shifting deployment.
- Perform a traffic shifting deployment.

### Technical knowledge prerequisites

- Basic navigation of the AWS management Console.
- Familiarity editing scripts using AWS Cloud9.
- Comfortable interacting with the AWS CLI using AWS Cloud9.

### Duration

This lab requires *50* minutes to complete.

### Icon key

Various icons are used throughout this lab to call attention to different types of instructions and notes. The following list explains the purpose for each icon:

- **Command:** A command that you must run.
- **Expected output:** A sample output that you can use to verify the output of a command or edited file.
- **Note:** A hint, tip, or important guidance.
- **Additional information:** Where to find more information.
- **WARNING:** An action that is irreversible and could potentially impact the failure of a command or process (including warnings about configurations that cannot be changed after they are made).
- **Consider:** A moment to pause to consider how you might apply a concept in your own environment or to initiate a conversation about the topic at hand.

## Start lab

1.  To launch the lab, at the top of the page, choose Start lab.

You must wait for the provisioned AWS services to be ready before you can continue.

1.  To open the lab, choose Open Console.

You are automatically signed in to the AWS Management Console in a new web browser tab.

**Do not change the Region unless instructed.**

### Common sign-in errors

#### *Error: You must first sign out*

![Amazon Web Services Sign In You must first log out before logging into a different AWS account. To logout, click here ](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image1.png){width="13.083333333333334in" height="2.8333333333333335in"}

If you see the message, **You must first log out before logging into a different AWS account:**

- Choose the **click here** link.
- Close your **Amazon Web Services Sign In** web browser tab and return to your initial lab page.
- Choose Open Console again.

#### *Error: Choosing Start Lab has no effect*

In some cases, certain pop-up or script blocker web browser extensions might prevent the **Start Lab** button from working as intended. If you experience an issue starting the lab:

- Add the lab domain name to your pop-up or script blocker's allow list or turn it off.
- Refresh the page and try again.

### Lab environment

Below you find a diagram of the architecture used for this lab:

![Lab Architecture](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image2.png){width="13.083333333333334in" height="6.645833333333333in"}

*The image depicts the pipeline by pulling data from CodeCommit into CodePipeline. Then invokes AWS CodeBuild which calls AWS CloudFormation to create the Amazon APIGateway endpoint and Lambda function created using Node.js. Lastly, it deploys the application using AWS CodeDeploy.*

### AWS Services Not Used in This Lab

AWS services that are not used in this lab are deactivated in the lab environment. In addition, the capabilities of the services used in this lab are limited to what the lab requires. Expect errors when accessing other services or performing actions beyond those provided in this lab guide.

## Task 1: Introduction to AWS SAM elements

Build an application using AWS SAM and use SAM to validate the functionality of your application locally. After validation, deploy your application to AWS Lambda and test your deployment.

### Task 1.1: Connect to the Cloud9 environment

In this task, you connect to your Cloud9 environment.

**Note:** Make sure that you are using the AWS Region that matches the **AwsRegionCode** value from the navigation panel to the left of these instructions.

1.  Open the **Cloud9** environment by copying the **URL** value to the left of these instructions for the heading reading **Cloud9Environment** and pasting it into a new browser tab.

### Task 1.2: Building an application with AWS SAM

Use the provided files to build your application and use the AWS SAM **invoke** command to test your application.

On the left of the Cloud9 new browser tab/window is the Environment pane. On the right, the top pane is the AWS Cloud9 Welcome screen. The lower pane includes three tabs: two with bash terminals and a tab labeled **Immediate**. You can work in either of the bash terminal tabs.

**Note:** When the AWS Cloud9 environment is first launched, a bash script clones the CodeCommit repository that you use for this lab. The repository contains both an **AWS SAM** *hello world* application and an application to the demonstrate the traffic shifting deployment model using AWS SAM.

1.  **Command:** Run the following command to confirm that you are in the **environment** directory:

cd ~/environment/

**Expected Output:**

*None, unless there is an error.*

1.  **Command:** Run the following command to initialize a SAM project:

sam init --runtime python3.9

This initializes the AWS SAM environment that is local to your AWS Cloud9 environment. After a successful initialization the following prompt is displayed.

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

SAM CLI now collects telemetry to better understand customer needs.

You can OPT OUT and disable telemetry collection by setting the
environment variable SAM_CLI_TELEMETRY=0 in your shell.
Thanks for your help!

Learn More: <https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-telemetry.html>

Which template source would you like to use?
1 - AWS Quick Start Templates
2 - Custom Template Location
Choice:

If you see the message **Project initialization is now complete**, skip to **Step 11**. If you are prompted to choose a template source follow the steps below.

1.  **Command:** Enter **1** for the **AWS Quick Start Templates** when prompted to choose the template source and press **Enter**.
2.  **Command:** Enter **1** for the **Hello World Example** and press **Enter**
3.  **Command:** Enter **N** for the **Would you like to enable X-Ray tracing on the function(s) in your application?** prompt and press **Enter**
4.  **Command:** Enter **N** for the **Would you like to enable monitoring using CloudWatch Application Insights?** prompt and press **Enter**
5.  **Command:** Press **Enter** to accept the suggested project name **sam-app**.

The full initialization process should be similar to the output below.

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

Which template source would you like to use?
1 - AWS Quick Start Templates
2 - Custom Template Location
Choice: 1

Choose an AWS Quick Start application template
1 - Hello World Example
2 - Infrastructure event management
3 - Multi-step workflow
4 - Lambda EFS example
Template: 1

Based on your selections, the only Package type available is Zip.
We will proceed to selecting the Package type as Zip.

Based on your selections, the only dependency manager available is pip.
We will proceed copying the template using pip.

Would you like to enable X-Ray tracing on the function(s) in your application? [y/N]: N

Project name [sam-app]:

Cloning from <https://github.com/aws/aws-sam-cli-app-templates> (process may take a moment)

-----------------------
Generating application:
-----------------------
Name: sam-app
Runtime: python3.9
Architectures: x86_64
Dependency Manager: pip
Application Template: hello-world
Output Directory: .

Next steps can be found in the README file at ./sam-app/README.md


Commands you can use next
=========================
[*] Create pipeline: cd sam-app && sam pipeline init --bootstrap
[*] Validate SAM template: sam validate
[*] Test Function in the Cloud: sam sync --stack-name {stack-name} --watch

SAM CLI update available (1.61.0); (1.57.0 installed)
To download: <https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html>

1.  **Command:** Change directories to **sam-app** by running the command:

cd ./sam-app

**Expected output:**

*None, unless there is a change*

1.  **Command:** Next run the following command to perform a build:

sam build

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

Building codeuri: /home/ec2-user/environment/sam-app/hello_world runtime: python3.9 metadata: {} architecture: x86_64 functions: ['HelloWorldFunction']
Running PythonPipBuilder:ResolveDependencies
Running PythonPipBuilder:CopySource

Build Succeeded

Built Artifacts : .aws-sam/build
Built Template : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Invoke Function: sam local invoke
[*] Deploy: sam deploy --guided

After a successful build, test your application **locally**. Use **AWS SAM** to test your application. When reviewing the test output, look for a status code **200**.

1.  **Command:** Run the following command to test the application:

sam local invoke HelloWorldFunction --event events/event.json

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

Invoking app.lambda_handler (python3.9)
Image was not found.
Removing rapid images for repo public.ecr.aws/sam/emulation-python3.9
Building image.................................................................................................................................................
Skip pulling image and use local one: public.ecr.aws/sam/emulation-python3.9:rapid-1.33.0-x86_64.

Mounting /home/ec2-user/environment/sam-app/.aws-sam/build/HelloWorldFunction as /var/task:ro,delegated inside runtime container
START RequestId: 1441e62a-937d-40ae-b470-1ce87b992e54 Version: $LATEST
END RequestId: 1441e62a-937d-40ae-b470-1ce87b992e54
REPORT RequestId: 1441e62a-937d-40ae-b470-1ce87b992e54 Init Duration: 0.25 ms Duration: 80.96 ms Billed Duration: 81 ms Memory Size: 128 MB Max Memory Used: 128 MB
{"statusCode": 200, "body": "{"message": "hello world"}"

Using **invoke** shows you the application output and returns an html status code.

**Note:** If you receive a timeout error in the terminal after running the invoke command, wait 3 seconds and run the invoke command once again. If the function still does not invoke an there are errors mounting the Docker container image, see the Appendix for troubleshooting.

1.  **Command:** Run the following command to start the application:

sam local start-api -p 8080

Using the AWS SAM command **start-api** locally deploys your application to a temporary container, allowing you to browse your web application. Browsing your application allows you to perform simple manual tests.

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

Mounting HelloWorldFunction at <http://127.0.0.1:8080/hello> [GET]
You can now browse to the above endpoints to invoke your functions. You do not need to restart/reload SAM CLI while working on your functions, changes are reflected instantly/automatically. You only need to restart SAM CLI if you update your AWS SAM template
2022-07-15 17:19:56 * Running on <http://127.0.0.1:8080/> (Press CTRL+C to quit)

The **start-api** command directs you to browse to port 8080 on the local system loopback address (127.0.0.1). AWS Cloud9 does not allow this and you receive an error if you use the address included in the command output. AWS Cloud9 does offer a function to securely view your application through the **Preview** feature.

1.  Select **Preview** from the AWS Cloud9 menu bar at the top of your browser window.
2.  Select **Preview Running Application**.

This splits the lower pane. Your terminal is now to the left and a browser window is to the right of the terminal. The browser displayed a message **Missing Authentication Token**; this is expected.

**Note:** If you're running this lab in the Safari web browser, you may receive a Notice about **Third-party cookies disabled**. To complete steps 17-19 without enabling third-party cookies, you need to run them in Firefox or Chrome. If you choose not to change browsers or your settings, you can proceed to step 20.

![SAM Preview](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image3.png){width="13.083333333333334in" height="3.6354166666666665in"}

*Images show example of the SAM notice about third-party cookies being disabled.*

1.  Select the address bar within the browser and move your cursor to the far right of the displayed address.

![Cloud9 Browser Screenshot](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image4.png){width="13.083333333333334in" height="2.75in"}

*Image shows the browser with an example message about a missing authentication token.*

**Note:** If you delete all or part of the address, close the browser tab and reopen the window using the **Preview** menu.

1.  Enter

hello

after the last forward slash (/) and press **Enter** on your keyboard.

In the terminal, you see the lab4 application process your request and return a **GET**. The browser window updates when the application outputs the **GET** and displays the message *hello world*.

![SAM Preview Output](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image5.png){width="7.8125in" height="3.3125in"}

*Image depicts browser example of the hello world message being displayed.*

1.  **Command:** Return to the **terminal** and press **CTRL+C** to stop the test.
2.  Close the **preview browser window** and continue with **Task 1.3**.

### Task 1.3: Using AWS SAM to deploy an application

The first step in deploying the *hello world* application is to create an Amazon Simple Storage Service (Amazon S3) bucket to host your application.

1.  **Command:** Replace the **user values** in the following command and run it to create a variable with your bucket name in it:

labBucket=lab4-sam-[YOUR-INITIALS]-[YOUR-POSTAL-CODE]

**Example:** labBucket=lab4-sam-mh-75135

**Expected output:**

*None, unless there is a change*

**Note:** You can verify the contents of your variable by running

echo $labBucket

1.  **Command:** Use the variable to create a globally unique bucket with the command below:

aws s3 mb s3://$labBucket

**Expected output:** Your bucket name differs from the example below.

******************************
**** This is OUTPUT ONLY. ****
******************************

make_bucket: lab4-sam-mh-75135

**WARNING:** If you receive an error that the bucket name is unavailable, create the variable again with some extra characters after your initials and then try to create the bucket again.

1.  **Command:** Run the following command to package your application and push it to the S3 bucket you created:

sam package --output-template-file packaged.yaml --s3-bucket $labBucket

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

Uploading to 8cfc3d84a3e0ba44c9da03af7b02a7a6 463720 / 463720 (100.00%)

Successfully packaged artifacts and wrote output template to file packaged.yaml.
Execute the following command to deploy the packaged template
sam deploy --template-file /home/ec2-user/environment/sam-app/packaged.yaml --stack-name <YOUR STACK NAME>

1.  **Command:** Run the command below to deploy your application package:

sam deploy --template-file packaged.yaml --stack-name sam-app --capabilities CAPABILITY_IAM

**Expected output:** This output is truncated.

******************************
**** This is OUTPUT ONLY. ****
******************************


Deploying with following values
===============================
Stack name : sam-app
Region : None
Confirm changeset : False
Deployment s3 bucket : None
Capabilities : ["CAPABILITY_IAM"]
Parameter overrides : {}
Signing Profiles : {}

Initiating deployment
=====================

Waiting for changeset to be created..

Changeset created successfully. arn:aws:cloudformation:us-west-2:234098603692:changeSet/samcli-deploy1657908029/7abfbac6-647b-4077-900e-13565680e35d


2022-07-15 18:00:40 - Waiting for stack create/update to complete

CloudFormation outputs from deployed stack
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Outputs
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Key HelloWorldFunctionIamRole
Description Implicit IAM Role created for Hello World function
Value arn:aws:iam::234098603692:role/sam-app-HelloWorldFunctionRole-1QTJRD7UZ5HX8

Key HelloWorldApi
Description API Gateway endpoint URL for Prod stage for Hello World function
Value <https://bp5ncclwm1.execute-api.us-west-2.amazonaws.com/Prod/hello/>

Key HelloWorldFunction
Description Hello World Lambda Function ARN
Value arn:aws:lambda:us-west-2:234098603692:function:sam-app-HelloWorldFunction-1riHJ2Fib3EM
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Successfully created/updated stack - sam-app in None

Performing an **AWS SAM deploy** initiates an AWS CloudFormation stack build. When the stack creation has successfully completed a summary of **stack outputs** is presented in the terminal.

**Note:** If a list of **stack outputs** is not presented in the terminal, run the following command from the terminal:

aws cloudformation describe-stacks --stack-name sam-app --query 'Stacks[].Outputs[?OutputKey==`HelloWorldApi`]' --output table

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

-------------------------------------------------------------------------------------------------------------------------------------------------------------
| DescribeStacks |
+-------------------------------------------------------------------+----------------+----------------------------------------------------------------------+
| Description | OutputKey | OutputValue |
+-------------------------------------------------------------------+----------------+----------------------------------------------------------------------+
| API Gateway endpoint URL for Prod stage for Hello World function | HelloWorldApi | <https://bp5ncclwm1.execute-api.us-west-2.amazonaws.com/Prod/hello/> |
+-------------------------------------------------------------------+----------------+----------------------------------------------------------------------+

1.  Locate the **API Gateway endpoint URL** and copy the **OutputValue**. Paste the **URL** into a new browser tab or window.

**Note:** When the URL loads, expect to see a simple webpage with the following message in the upper left corner of the page.

**{"message": "hello world"}**

**Consider:** The following steps are **optional instructions** for observing the stack creation and obtaining the API URL from the AWS CloudFormation console. These step do not need to be performed, as the same information can be obtained from the AWS Cloud9 terminal pane.

1.  At the top of the AWS Management Console, in the search bar, search for and choose

CloudFormation

.

2.  Select **Stacks** in the navigation pane.
3.  Select the stack **sam-app** in the **Stack name** column of the console.
4.  Select the **Events** tab from the console.
5.  Explore the data **Events** tab and observe the resources of the AWS CloudFormation stack being created.
6.  Wait for the stack status to change to **CREATE_COMPLETE**.
7.  Select the **Outputs** tab from the AWS CloudFormation console.
8.  From the table, copy the **DNS URL value** from the **HelloWorldApi key**, and paste the URL into a new browser tab or window.
9.  Return to the **AWS Cloud9 IDE** for the next lab task.

**Congratulations!** You have deployed and tested the application locally using AWS SAM as well as using AWS SAM with CloudFormation.

## Task 2: Automate application deployment with AWS CodePipeline

For this task, you create a **CI/CD pipeline** to automate the deployment of an application. You create a three-stage pipeline that uses **AWS CodeCommit**, **AWS CodeBuild**, and **AWS CloudFormation**. Your initial **HelloWorld** application was initialized using **Python** as the language but you've chosen to change to **Node.js** for the deployment. You overwrite your existing SAM deployment with this new one. The new SAM project is in the Cloud9 environment. You need to add a new bucket for your deployment and make the appropriate code changes before you can automate the deployment.

### Task 2.1: Prepare the sample serverless application for deployment

1.  **Command:** In the **AWS Cloud9 terminal**, change your directory to **lab4-app** by running to the following command:

cd ~/environment/lab4-app

**Expected output:**

*None, unless there is an error.*

1.  Verify what the bucket name is that created before with the following command:

echo $labBucket

**Expected output:** Your bucket name differs from the bucket name shown in this example.

******************************
**** This is OUTPUT ONLY. ****
******************************

lab4-sam-mh-75135

1.  In the AWS Cloud9 Environment pane, in the **lab4-app** folder, locate the **buildspec.yml** file, open the context menu, and choose **Open**.
2.  In the AWS Cloud9 editor window, replace the text **lab4-sam-<YOUR-INITIALS>-<YOUR-POSTAL-CODE>** on line 5 with the name of the bucket you created earlier.

**Example:**

**********************************
**** This is an EXAMPLE ONLY. ****
**********************************

version: 0.2
phases:
build:
commands:
- export BUCKET=lab4-sam-mh-75135
- aws cloudformation package --template-file template.yaml --s3-bucket $BUCKET --output-template-file outputtemplate.yaml
artifacts:
files:
- outputtemplate.yaml

1.  Save your changes.
2.  **Command:** Confirm that your change to the **buildspec.yml** file was saved by running the following command:

git status

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
(use "git add <file>..." to update what needs be committed)
(use "git restore <file>..." to discard changes in working directory)
modified: buildspec.yml

no changes added to commit (use "git add" and/or "git commit -a")

**Note:** If git does not show the **buildspec.yml** file as *modified*, verify that your change was saved.

1.  **Command:** Update the remote AWS CodeCommit repository by running the following commands:

git add .
git commit -m "Updated buildspec file with bucket name"
git push

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

AWSLabsUser-9MzQSXXiBNVTAx39Ec6oKq:~/environment/lab4-app (main) $ git add .
AWSLabsUser-9MzQSXXiBNVTAx39Ec6oKq:~/environment/lab4-app (main) $ git commit -m "Updated buildspec file with bucket name"
[main e0f185b] Updated buildspec file with bucket name
Committer: EC2 Default User [<ec2-user@ip-10-187-10-174.us-west-2.compute.internal](mailto:%3cec2-user@ip-10-187-10-174.us-west-2.compute.internal)>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

git config --global user.name "Your Name"
git config --global user.email <you@example.com>

After doing this, you may fix the identity used for this commit with:

git commit --amend --reset-author

1 file changed, 1 insertion(+), 1 deletion(-)
AWSLabsUser-9MzQSXXiBNVTAx39Ec6oKq:~/environment/lab4-app (main) $ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 344 bytes | 344.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
To <https://git-codecommit.us-west-2.amazonaws.com/v1/repos/lab4-app>
7c72818..e0f185b main -> main

**Note:** You can ignore the warning message about **Your name and email address**. This warning is raised when the Git global variables (user name and email) are not set in the AWS Cloud9 environment. The warning does not prevent you from successfully running Git commands.

### Task 2.2: Create the continuous delivery pipeline for your lab4 application

Use the **AWS CodePipeline wizard** to create the pipeline stages necessary for continuous delivery of your base image. For this task, return to the **Your environments** tab in your browser and complete the following steps.

1.  At the top of the AWS Management Console, in the search bar, search for and choose

CodePipeline

in a new browser tab.

2.  On the **CodePipeline dashboard**, choose Create pipeline .

**Note:** If you see an introductory page instead of the dashboard, choose **Get Started Now**.

1.  On the **Choose pipeline settings** page, for **Pipeline name**, enter

lab4-sam-pipeline

.

**Note:** The CodePipeline wizard creates a role for the **lab4-sam-pipeline**. Do not modify the role the wizard creates.

1.  Accept the default values for the remaining options on the **Choose pipeline settings** page.
2.  Choose Next .
3.  On the **Add source stage** page, choose **AWS CodeCommit** from the drop-down menu for **Source provider**.

**Note:** After you select **AWS CodeCommit** from the drop-down menu, the browser refreshes the page to display additional options, **Repository name**, **Branch name**, and **Change detection options**.

1.  Configure the following options:



- For **Repository name**, enter

lab4-app

- For **Branch name**, enter

main

- For **Change detection options**, select **Amazon CloudWatch Events (recommended)**
- For **Output artifact format**, select **CodePipeline default**



1.  Choose Next .
2.  On the **Add build stage** page, choose **AWS CodeBuild** from the drop-down menu for **Build provider**.

**Note:** After you select **AWS CodeBuild** from the drop-down menu, the browser refreshes the page to display additional options: **Region**, **Project name**, **Environment variables**.

1.  For **Region**, ensure that selection matches the region in the **AwsRegionCode** value from the navigation panel to the left of these instructions.
2.  For **Project name**, choose Create project .

**Note:** A new browser window opens for creating a CodeBuild project.

1.  For **Project name**, enter

lab4-sam-build

2.  In the **Environment** card, configure the following values:



- For **Environment image**, choose **Managed image**
- For **Operating system**, choose **Ubuntu** from the drop-down menu
- For **Runtime(s)**, choose **Standard** from the drop-down menu
- For **Image**, choose **aws/codebuild/standard: 7.0** from the drop-down menu
- For **Image version**, choose **Always use the latest image for this runtime version** from the drop-down menu
- For **Environment type**, choose **Linux EC2** from the drop-down menu
- For **Service role**, select **Existing service role**
- For **Role ARN**, choose CodeBuild_Service_Role (it auto-updates with the ARN)
- Clear the check box for **Allow AWS CodeBuild to modify this service role so it can be used with this build project**.



1.  In the **Buildspec** and **Logs** sections, keep the default values.
2.  Choose Continue to CodePipeline .

**Expected output:**

*Successfully created lab4-sam-build in CodeBuild.*

**Note:** The CodeBuild project creation page is closed by the browser, and you are returned to the **Add build stage** browser window.

The browser opens to the **Add deploy stage** page.

1.  For **Deploy provider**, choose **AWS CloudFormation** from the drop-down menu.

**Note:** After you select **AWS CloudFormation** from the drop-down menu, the browser refreshes to display additional options, **Region**, **Action mode**, and **Stack name**.

1.  Configure the following options:



- For **Region**, confirm that the value matches the **AwsRegionCode** output to the left of these instructions
- For **Action mode**, choose **Create or replace a change set** from the drop-down menu
- For **Stack name**, choose **sam-app**
- For **Change set name**, enter

lab4-sam-changeset

**Note:**

- You need to manually specify the changeset name **lab4-sam-changeset**.
- After you selected **Create or replace a change set** the page refreshed to display additional options, **Template**, **Capabilities**, and **Role name**.

**Template**

- For **Artifact name**, choose **BuildArtifact** from the drop-down menu
- For **File name**, enter

outputtemplate.yaml

- For **Capabilities**, choose **CAPABILITY_IAM**

**Note:** You may need to mouse away from the dropdown to see that the **CAPABILITY_IAM** option has been selected.

- For **Role name**, choose **SAM_Role** (It auto-populates with arn:aws:iam::ACCOUNT-ID:role/SAM_Role)

Your Deploy stage still needs some work but you have to fix that after the pipeline has been created. When you create the pipeline, it immediately starts running the steps. If the pipeline starts running, it wouldn't hurt anything, it would just lead to unnecessary waiting. Please continue directly to the **next step** after you choose Create pipeline .

1.  On the **review** page, choose Create pipeline .

**Expected output:**

*Success*

*Congratulations! The pipeline lab4-sam-pipeline has been created.*

1.  Choose Stop execution .
2.  For **Select execution**, choose the only option available.
3.  For **Choose a stop mode for the execution**, choose **Stop and abandon**.
4.  Choose Stop .

![stop execution screenshot](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image6.png){width="12.916666666666666in" height="12.895833333333334in"}

*Image show the stop execution window screen as an example of what options should be chosen.*

**Expected output:**

*Success*

*Stop execution 0d2df0b9-ab83-4691-990a-24d2ad15cfb1 successfully.*

Now you can complete your deploy stage. The current configuration creates an AWS CloudFormation change set but you still need to run it to shift traffic to the new version of your application. You could insert a manual approval here to confirm the changes that are deployed. For now, you don't need that step. Create an action to immediately run the change set and shift traffic to the new version.

1.  On the **lab4-sam-pipeline** page, choose Edit .
2.  On the **Edit: Deploy** card, choose Edit stage .
3.  In the **Edit: Deploy** section, choose + Add action group , that appears at the end of that card. Do not click on the action group on the top.
4.  On the **Edit action** page, for **Action name**, enter

deploy-changeset

5.  For **Action provider**, choose **AWS CloudFormation** from the drop-down menu.
6.  For **Region**, confirm that the value matches the **AwsRegionCode** output to the left of these instructions.
7.  Configure the following options.



- For **Input artifacts**, choose **BuildArtifact** from the drop-down menu
- For **Action Mode**, choose **Execute a change set** from the drop-down menu
- For **Stack name**, choose **sam-app**
- For **Change set name**, enter

lab4-sam-changeset

**Note:** You need to manually specify the changeset name **lab4-sam-changeset**

1.  Choose Done .
2.  Choose Done .
3.  At the top of the page, choose Save .
4.  On the **Save pipeline changes** acknowledgment pop-up window, choose Save .

**Expected output:**

*Success*

*Pipeline was saved successfully.*

1.  On the **lab4-sam-pipeline** page, choose Release change , and then choose Release .

Observe the progress of your pipeline through each stage. In less than 5 minutes, you should see **Succeeded** on each stage.

![Lab4 Pipeline Success](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image7.png){width="13.083333333333334in" height="18.677083333333332in"}

*Image shows the CodePipeline deployment steps where all steps have succeeded.*

**Note:** If the **Deploy** action of the **Deploy Stage** succeeds but the **deploy-changeset** action fails, choose Retry for the **Deploy Stage**.

### Task 2.3: Verify your deployment

Verify the successful deployment of the **lab4 application** by opening a browser and opening your application web page.

1.  Choose the the **AWS CloudFormation** link in the **deploy-changeset** action to open CloudFormation in a new browser tab.
2.  On the AWS CloudFormation dashboard, choose the text link **sam-app** .
3.  Select the **Outputs** tab and choose the URL beside the key **Lab4Api** to view the output of the SAM application.

**Expected output:** Web page will have a blue background.

*Lab4 application has been successfully deployed.*

**Congratulations!** You have successfully created a CodePipeline and deployed the sample application. You have also verified it is is functional.

## Task 3: Make a new change to the application and re-deploy

AWS CodePipeline monitors changes to the source code repository. Changes to that repository initiate an automated deployment of the application. In this task, you update the lab4 application to start a new deployment.

1.  Return to the **Lab4 - AWS Cloud9** browser tab.
2.  In the AWS Cloud9 Environment pane, locate the **app.js** file in the **Lab4/lab4-app/lab-app** directory. Open the context menu, and choose **Open**.
3.  On line 21, replace **lab4a.html** with

lab4b.html

.

4.  Save your changes.
5.  **Command:** In the AWS Cloud9 terminal, ensure that you are in the **lab4-app** directory by running the following command:

cd ~/environment/lab4-app

**Expected output:**

*None, unless there is a change*

1.  **Command:** Confirm that your changes to the **app.js** file is saved by running the following command:

git status

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
(use "git add <file>..." to update what needs be committed)
(use "git restore <file>..." to discard changes in working directory)
modified: lab-app/app.js

no changes added to commit (use "git add" and/or "git commit -a")

**Note:** If git does not show **app.js** as **modified**, verify that your changes to the file were saved correctly.

1.  **Command:** Update the remote repository by running the following commands:

git add .
git commit -m "Updated app.js file to display lab4b html page"
git push

**Expected output:**

******************************
**** This is OUTPUT ONLY. ****
******************************

AWSLabsUser-9MzQSXXiBNVTAx39Ec6oKq:~/environment/lab4-app (main) $ git add .
AWSLabsUser-9MzQSXXiBNVTAx39Ec6oKq:~/environment/lab4-app (main) $ git commit -m "Updated app.js file to display lab4b html page"
[main 4459e5f] Updated app.js file to display lab4b html page
Committer: EC2 Default User [<ec2-user@ip-10-187-10-174.us-west-2.compute.internal](mailto:%3cec2-user@ip-10-187-10-174.us-west-2.compute.internal)>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

git config --global user.name "Your Name"
git config --global user.email <you@example.com>

After doing this, you may fix the identity used for this commit with:

git commit --amend --reset-author

1 file changed, 1 insertion(+), 1 deletion(-)
AWSLabsUser-9MzQSXXiBNVTAx39Ec6oKq:~/environment/lab4-app (main) $ git push
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 2 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 404 bytes | 404.00 KiB/s, done.
Total 4 (delta 3), reused 0 (delta 0), pack-reused 0
To <https://git-codecommit.us-west-2.amazonaws.com/v1/repos/lab4-app>
e0f185b..4459e5f main -> main

1.  Return to the **CodePipeline - AWS Developer Tools** browser tab and select **lab4-sam-pipeline**.

When AWS CodePipeline detects the update to the AWS CodeCommit repository, it automatically initiates a re-build of the lab4 application. Observe the outcome of each stage of the pipeline.

**Note:** If the **Deploy** action of the **Deploy Stage** succeeds but the **deploy-changeset** action fails, select Retry for the **Deploy Stage**.

1.  Observe the pipeline as it works through each stage. Once the progress reaches the **deploy-changeset** action of the **Deploy** stage, review the deployment details using CodeDeploy.
2.  In the left navigation pane, choose **Deployments** under **Deploy > CodeDeploy**.

You can view **Deployment history** of all the deployments handled by CodeDeploy.

1.  Choose the **Deployment ID** link that is in progress. It is normally on top of the list.

Take time to review details. You notice various stages of the deployment process and how the traffic is shifting from to newer version.

1.  Return to the browser window/tab with the application URL and refresh the page. You notice traffic gradually shifting between the Original (blue) and Replacement (green) pages.

If you repeatedly refresh the browser window before the **Deploy** stage has completed all deployment stages, you can observe the traffic shifting deployment model in action. The web application switches between the blue and green pages until all deployment stages have successfully completed.

In the deployment console, the status looks like the following when it is in progress and half of the traffic has shifted to the new deployment.

![deployment halfway](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image8.png){width="13.083333333333334in" height="7.552083333333333in"}

*The images show the progress of the blue-green deployment at the 70% blue to 30% green status.*

In the deployment console, the status changes to the following when it has finished and all the traffic has shifted to the new deployment.

![deployment finished](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image9.png){width="13.083333333333334in" height="7.552083333333333in"}

*Image shows the blue-green deployment being completed successfully.*

**Expected output:** The web page background is now green.

*Lab4 application has been updated.*

After the **Deploy** stage is successful, the blue web page has been **completely** replaced by a green web page. The phrase in the upper left corner of the page has also been replaced with **Lab4 application has been updated.**

## Conclusion

**Congratulations!** You can now:

- Build and locally test your application.
- Package and deploy your application.
- Automate the CI/CD pipeline to support a traffic shifting deployment.
- Perform a traffic shifting deployment.

## End lab

Follow these steps to close the console and end your lab.

1.  Return to the **AWS Management Console**.
2.  At the upper-right corner of the page, choose **AWSLabsUser**, and then choose **Sign out**.
3.  Choose End lab and then confirm that you want to end your lab.

## Additional Resources

## Appendix

### Troubleshooting Cloud9 Disk Space

The Cloud9 environment is designed to not have a lot of extra disk space so that you are charged only for what you need. It is easy to add more if necessary but, not easy to shrink a disk that has extra space.

You need more than what you have available because you download docker images. You could add space with the instructions located [here](https://docs.aws.amazon.com/cloud9/latest/user-guide/move-environment.html#move-environment-resize), but you do not need to do that for this lab. There are pre-loaded docker images on the disk that can be pruned since they are not needed for this limited use case. Either approach worked in your real world projects since any pruned docker images are downloaded again if they are needed.

1.  Run the following command to confirm if there is a disk space issue:

df -h

![Disk space issue](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image10.png){width="8.333333333333334in" height="1.6666666666666667in"}

1.  If you have close to 100% in use on **/dev/xvda1**, run the following command to remove any extra Docker images from the Cloud9 environment:

docker system prune -af

For more information about AWS Training and Certification, see *<https://aws.amazon.com/training/>.*

*Your feedback is welcome and appreciated.*
*If you would like to share any feedback, suggestions, or corrections, please provide the details in our [AWS Training and Certification Contact Form](https://support.aws.amazon.com/#/contacts/aws-training).*





![](../../../media/AWS-DevOps-Module-7-Lab-4--Deploying-a-serverless-application-using-AWS-Serverless-Application-Model-(AWS-SAM)-and-a-CI-CD-pipeline---Self-Paced-Labs-image11.png){width="0.8125in" height="13.083333333333334in"}













