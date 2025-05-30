# 23 Amazon Elastic Container Registry and Amazon Elastic Kubernetes Service



---

![](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image1.png)



![Now, we're gonna want to integrate this in with our CI/CD pipeline, and we can do so, in this example, through the use of Jenkins handling those build processes. We're gonna install our Docker plug-ins into our Jenkins build environment, I run, not my buildspec file, but the same sort of set of instructions to run my Docker build command to build that image, and then I have to authenticate Jenkins to either push that image into ECR, into Docker Hub or into a private repo that you're hosting, potentially on-premise. In this diagram, we can see our continuous integration server performing a rolling update inside of ECS. We'll get more into our updating options a little bit later on when we talk about deployments. But the continuous integration server will first go through and generate a new task definition or a new version of that task and update that service to reflect that new version, placing that new task behind our Elastic Load Balancer. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image2.png)



![Video Transcript The Amazon Elastic Kubernetes Service, or shorthand, EKS, is a managed version of Kubernetes. Now, Kubernetes started as a Google application and has since been open sourced by our good friends over there. It allows us an alternative way to schedule and run containers inside of our environment. Now, let's take a look at a diagram here that really specifies what EKS is. Here we see a diagram of EKS. We see some new terms that we haven't covered before. They're specific to Kubernetes. I have my master nodes up there in the corner. They are publishing out that API that I can then hit to schedule containers to run on those worker nodes. Containers are grouped together in pods, pods are placed on those worker nodes, and are run effectively, giving me that same kind of environment that we talked about building out a minute ago with ECS. At the end of the day, ECS and EKS are two different ways to effectively get to the same point. It's whether you wanna run that open source Kubernetes application, or you want to consume and utilize our ECS scheduler. EKS can play a similar role inside of our pipeline here. Instead of publishing into ECS, I'm building my image, I'm hosting it inside of ECR, and part of my deployment process includes a Lambda function that reaches out to tell Kubernetes, "Hey, we've got a new image that we need to run. Let's update that pod to run on EKS and serve out that content to our end users." This is a similar diagram, now utilizing ECS inside of our pipeline. The same process applies. I build my image, I deploy the image into ECR. Now, from here, instead of going straight into ECS, as I did in my demonstration, we're going to CloudFormation and to tell CloudFormation to update our ECS environment. Now, why are we using CloudFormation here? Well, if we've templatized everything in CloudFormation, I can easily pick this content up and run it anywhere I want because I followed that best practice principle of infrastructure as code. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image3.png)



![We should have an understanding now of what it looks like to build and run a container inside of AWS. We've taken a look at ECS, Fargate, EKS, and ECR. A lot of acronyms here to run all of these containers. Later on in this course, you'll be taking a look at running one of these yourself, but there's a few other concepts we have to hit on before that. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image4.png)





![Amazon Elastic Container Registry (Amazon ECR) is a fully managed Docker container registry that makes it easy to store, share and deploy container images. To watch the instructor video, choose the play button. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image5.png)



![Key concepts and topics Review the content below to reinforce some of the key concepts and topics presented to you in the video above. To learn more, choose appropriate tab. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image6.png)



![AMAZON ECR AMAZON ELASTIC KUBERNETES Amazon Elastic Container Registry (Amazon ECR) is a managed AWS Docker registry service that is secure, scalable, and reliable. Amazon ECR supports private Docker repositories with resource- based permissions. It uses AWS Identity and Access Management (IAM) so that specific users or Amazon EC2 instances can access repositories and images. Developers can use the Docker CLI to push, pull, and manage images. Write code Write and package code as a Docker image Amazon ECR O Compress, encrypt, and control access to images o Version, tag, and manage image lifecycles Run containers Pull image and run containers anywhere Amazon ECS Amazon EKS AWS Cloud On-premises ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image7.png)



![AMAZON ECR AMAZON ELASTIC KUBERNETES Amazon Elastic Kubernetes Service (Amazon EKS) is a managed service that you can use to run Kubernetes on AWS without needing to install and operate your own Kubernetes clusters. Challenges with Kubernetes Kubernetes is an open-source system for automating the deployment, scaling, and management of containerized applications. Operating Kubernetes for production applications presents a number of challenges: • You must manage the scaling and availability of your Kubernetes masters and persistence layer by ensuring that you have chosen appropriate instance types, ran them across multiple Availability Zones, monitored their health, and replaced unhealthy nodes. • You need to patch and upgrade your masters and worker nodes to provide that you are running the latest version of Kubernetes. This all requires expertise and a lot of manual work. Amazon Elastic Kubernetes Service With Amazon EKS, upgrades and high availability are managed for you by AWS. Amazon EKS runs three Kubernetes masters across three Availability Zones to provide high availability. Amazon EKS automatically detects and replaces unhealthy masters, and it provides automated version upgrades and patching for the masters. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image8.png)



![Amazon EKS EKS makes it easy to run Kubernetes on AWS Provision an EKS cluster Amazon EC2 Deploy worker nodes for your EKS cluster Connect to EKS Run Kubemetes apps ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image9.png)



![Sample architecture for Amazon EKS and a CD pipeline Amazon Elastic Container Registry AWS CodeBuild Developer AWS CodeCommit AWS CodePipeline Amazon Elastic Kubernetes Service AWS Lambda Role Parameter Store ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image10.png)



![last Ser SL iePi 1st Developers commit code to an AWS CodeCommit repository and create pull requests to review proposed changes to the production code. When the pull request is merged into the master branch in the AWS CodeCommit repository, AWS CodePipeline automatically detects the changes to the branch and starts processing the code changes through the pipeline. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image11.png)



![---Ä.JA¯ m 2nd AWS CodeBuild packages the code changes in addition to any dependencies and builds a Docker image. Another pipeline stage might test the code and the package, also using AWS CodeBuild. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image12.png)



![3rd The Docker image is pushed to Amazon ECR after a successful build or test stage. > Elastic Cont Registry S CodeBuild ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image13.png)



![er Ama elo 4th AWS CodePipeline invokes an AWS Lambda function that includes the Kubernetes Python client as part of the function's resources. The Lambda function performs a string replacement on the tag used for the Docker image in the Kubernetes deployment file to match the Docker image tag applied in the build, one that matches the image in Amazon ECR. AWS CodeBuild also updates the tag references in the deployment manifest. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image14.png)



![5th After the deployment manifest update is completed, AWS Lambda uploads it to Amazon EKS. AWS Lambda invokes the Kubernetes API to update the image in the Kubernetes application deployment. Ela ws ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image15.png)



![6th Based on the image in the manifest, the image is pulled from Amazon ECR. Kubernetes performs a rolling update of the pods in the application deployment to match the Docker image specified in Amazon ECR. ner ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image16.png)



![Sample architecture for a CI/CD pipeline for containers Developers AWS CodeCommit AWS CloudFormation AWS CodePipeline AWS CodeBuild Amazon ECS Amazon ECR ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image17.png)



![1st Developers commit code to an AWS CodeCommit repository. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image18.png)



![WSÆloudFormation 2nd AWS CodePipeline continuously polls AWS CodeCommit repository for any new code change. It then triggers subsequent stages in the CI/CD pipeline. :ommit Amazon EC ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image19.png)



![3rd AWS CodePipeline sends the newly committed code revision to AWS CodeBuild. AWS CodeBuild builds a Docker container image from the code. AWS ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image20.png)



![co 4th AWS CodeBuild pushes the Docker container image tagged with build ID to Amazon ECR repository. AWS CodeBuild ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image21.png)



![AWS C 5th AWS CodePipeline uses AWS CloudFormation as a deployment tool and updates AWS CloudFormation stack, which re-defines the Amazon ECS task definition and service. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image22.png)



![WS.CloudFormation I 6th AWS CloudFormation creates a new task definition (however, in this scenario, AWS CloudFormation is not needed to be run every time) by referencing the newly created Docker sc image and updates the Amazon ECS service. ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image23.png)



![AWS CloudFormation 7th Amazon ECS fetches the new container from Amazon ECR and replaces the old task with the new one, and restarts the service that completes the deployment. AWS CodeBuild Amazon ECS Amazon ECR ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image24.png)







![Amazon Elastic Container Registry (Amazon ECR) Write code Write and package code as a Docker image Amazon ECR o Compress, encrypt, and control access to images O Version, tag, and manage image lifecycles Run containers Pull image and run containers anywhere training and -y certification Amazon ECS Amazon EKS AWS Cloud On-premises 0 Ama.•on Weh Inc O' AL rescued ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image25.png)



![Amazon ECR components • Registry: store images in container repositories • Authorization token: Used by Docker CLI for authentication • Repository: Contains your images • Repository policy: Controls access to repository and images • Image: Static file that contains all the details to run an application ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image26.png)



![Amazon ECR repositories Access repository by using Docker CLI or AWS CLI, AWS SDK, AWS Management Console User aws ecr get-login training and -y certification Amazon ECR Dev repository Test repository • myApp A . myApp C "Sid": " "Effect "Principal "Dev" , "Action" "ecr : GetDown10adUr1ForLayer" , "ecr : BatchGetImage " , • BatchCheckLayerAvai1abi1ity" ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image27.png)



![Integrate Amazon ECR with your CI system training and certification To publish Docker images to Amazon ECR or Docker Hub, use the Docker Build and Publish plugin with Jenkins. Jenkins Continuous Integration Server Pull Code Docker Build and Publish plugin Amazon ECR Publish Docker image Docker Hub ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image28.png)



![Integrate Amazon ECS with your CI system aWS training -7 certifica To enable rolling deployments of your application, use Amazon ECS to work with your Cl system. Pull code Continuous integration server Update service Elastic Load Balancing Amazon ECS cluster Integrate ilinazon ECR z.'aur s stem ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image29.png)





![Amazon Elastic Kubernetes Service (Amazon EKS) Amazon EKS EKS makes it easy to run Kubernetes on AWS Provision an EKS cluster Deploy worker nodes for your EKS cluster Connect to EKS training and certification Run Kubernetes apps ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image30.png)



![What is Amazon EKS? Controller manager Masters etcd Worker nodes us-east-la kube-proxy pod Cloud controller API Server docker pod training and -y certification Scheduler kubelet pod ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image31.png)



![Example: Amazon EKS and CD pipeline training and -7 certification Amazon Elastic Container Registry 3. Developer 2. AWS CodeCommit 4. AWS CodeBuild Amazon Elastic Kubernetes Service 5. AWS Lambda Role AWS CodePipeline 0 2020 Ama.•on Weh Inc O' Its Al. Parameter Store ](../../../media/AWS-DevOps-Module-6-23-Amazon-Elastic-Container-Registry-and-Amazon-Elastic-Kubernetes-Service-image32.png)
































