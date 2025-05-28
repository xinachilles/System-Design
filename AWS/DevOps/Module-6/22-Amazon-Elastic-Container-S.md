# 22 Amazon Elastic Container Service and AWS Fargate

Created: 2023-10-08 15:55:39 -0600

Modified: 2023-10-22 17:42:14 -0600

---

Summary

The text delves into the functionalities of Amazon's Elastic Container Service (ECS) and Fargate, highlighting their capabilities in managing and running containers, ensuring security, and integrating with other AWS services.

Facts

- ECS addresses challenges related to managing multiple containers, such as resource allocation and failure recovery.



- ![Amazon Elastic Container Service (Amazon ECS) Build images and store using ECR or any other repository Amazon Elastic Container Service Define your app Select container images and resources needed for your app Launch containers on EC2 Launch containers on Fargate training and -y certification O Manage containers Amazon ECS scales your app and manages your containers for availability ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image1.png){width="5.0in" height="2.7152777777777777in"}
- Fargate offers a serverless environment for running containers, eliminating the need to manage EC2 instances.



- ![AWS Fargate Build a container image Amazon ECS Define the images and resources needed for your app AWS Fargate Launch containers and AWS Fargate manages all of the underlying container infrastructure Launch containers AWS Fargate runs your containers for you 0 Ama.•on vnc o' Al re-sewed training and -7 certification AWS Manage containers Amazon ECS scales your applications and manages your containers for availability 19 ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image2.png){width="5.0in" height="2.8333333333333335in"}
- Containers in Fargate can be placed inside a VPC, assigned a private IP, and be put behind load balancers.
- In terms of security with Fargate, Amazon manages the infrastructure while users control the container and its tasks.
- ECS offers two launch options: the traditional EC2 instance or the serverless Fargate.
- Task definitions in ECS specify the container's requirements, image, CPU, RAM, and roles.



![Amazon ECS task definitions • Required to run Docker containers in Amazon ECS • Contains information such as: Which Docker images to use How much CPU and memory to use with each container aws training and certification Ports from the container that are mapped to the host container instance Data volumes Any environment variables passed to the container IAM role of tasks ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image3.png){width="5.0in" height="2.7152777777777777in"}

- ECS tasks can be assigned specific roles, ensuring that permissions are container-specific and not tied to the EC2 instance.
- ECS integrates with other AWS services, allowing for a workflow where Docker files are built, images are stored in ECR, and tasks are placed on EC2 instances.
- ECS offers flexibility in scheduling tasks, allowing users to decide when, where, and on what platform (EC2 or Fargate) a task will run.
- Task placement in ECS considers cluster constraints, custom constraints, and placement strategies (spread or bin pack).
- ECS supports both short-lived tasks and long-running applications, with the latter being scalable based on demand.
- AWS provides APIs for ECS, enabling users to create custom schedulers and integrate with other AWS services like Lambda, Elasticsearch, and SNS.





![Key concepts and topics Review the content below to reinforce some of the key concepts and topics presented to you in the video above. Amazon Elastic Container Service Amazon Elastic Container Service (Amazon ECS) is a highly scalable, high-performance container management service that supports Docker containers. Use Amazon ECS to easily run applications on a managed cluster of Amazon EC2 instances. It is extensible through API. Amazon ECS is a scalable cluster service for: • Hosting containers that can scale up to thousands of instances • Monitoring the deployment of containers and managing the complete state of clusters. Clusters can use Spot Instances and Reserve Instances • Scheduling containers using built-in scheduler or a third-party scheduler (for example, Apache Mesos, Blox) ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image4.png){width="5.0in" height="3.6666666666666665in"}



![Amazon ECR Build images and store using ECR or any other repository Amazon Elastic Container Service Define your app Select container images and resources needed for your app Launch containers on EC2 Launch containers on Far ate O Manage containers Amazon ECS scales your app and manages your containers for availability ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image5.png){width="5.0in" height="1.8819444444444444in"}



![](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image6.png){width="5.0in" height="2.5in"}



![Build a container image Amazon ECS Define the images and resources needed for your app AWS Faraate Launch containers and AWS Farqate manages all of the underlying container infrastructure Launch containers AWS Farqate runs your containers for you AWS Manage containers Amazon ECS scales your applications and manages your containers for availability ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image7.png){width="5.0in" height="2.2083333333333335in"}



![Amazon ECS modes Amazon ECS has two modes: Fargate launch type and EC2 launch type. Type What are you specifying Tasks Action AWS Fargate launch type Package containers Specify CPU and memory Define networking and IAM Launch Amazon EC2 launch type Run containerized apps on a cluster of Amazon EC2 instances that you manage Schedule placement of containers Provision, patch and scale Determine the type of servers, apps, and containers ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image8.png){width="5.0in" height="3.125in"}



![Tasks and services Let's review tasks. Tasks are the atomic unit of deployment within Amazon ECS. Tasks are made up of one or more tightly coupled containers. A task may run standalone, or it may be part of a service. A service is an abstraction on top of a task. A service runs a specified number of tasks and can include a load balancer to distribute traffic across the tasks that are associated with the service. If any of your tasks should fail or stop, the service scheduler launches another instance of your tasl< to replace it, and maintains the specified number of tasks. On-dem workloads Task Managed by ECS task scheduler Run once or at intervals More effective for batch jobs ing applications Long- Service Task Task Application Load Balancer • Managed by Amazon ECS service scheduler Scale out and in ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image9.png){width="5.0in" height="3.8958333333333335in"}



![On-dem On-demand workloads ask Tasks are managed by the task scheduler and are ideal for on-demand workloads. You can run Managed by tasks once or at intervals, they are great for scheduler batch jobs, and you can use the RunTask API or Run once or even a custom Start Task command. More effecti jobs ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image10.png){width="5.0in" height="2.673611111111111in"}



![I-d Long-running applications For long-running applications, you want to use the service scheduler. It automatically performs health management, scales out and scales in, is ch Availability Zone-aware, and enables the grouping of containers. '10 ing applications emce Task Task Applicatior Load Balancer ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image11.png){width="5.0in" height="2.2291666666666665in"}





![Amazon ECS glossary terms • Cluster: logical grouping of tasks or services • Container instances: runs tasks • Agent: Instructs EC2 instance to start/stop containers • Task: required to run containers • Container definition: configurations to run container • Service: runs/maintains a specified number of tasks ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image12.png){width="5.0in" height="3.4444444444444446in"}



![Press Amazon ECS workflow esc Create image docker Dockerfile 5. Container image to exit full screen Publish to registry Place task training and certification Container registry (ECR, Docker Hub) 4. Download container image Task Definition Service Definition Service Schedule Amazon ECS Run tasks ECS Agent Tasks Amazon ECS cluster ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image13.png){width="5.0in" height="2.6458333333333335in"}







![Amazon ECS glossary terms • Cluster: logical grouping of tasks or services • Container instances: runs tasks • Agent: Instructs EC2 instance to start/stop containers • Task: required to run containers • Container definition: configurations to run container • Service: runs/maintains a specified number of tasks ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image12.png){width="5.0in" height="3.4444444444444446in"}



![Press Amazon ECS workflow esc Create image docker Dockerfile 5. Container image to exit full screen Publish to registry Place task training and certification Container registry (ECR, Docker Hub) 4. Download container image Task Definition Service Definition Service Schedule Amazon ECS Run tasks ECS Agent Tasks Amazon ECS cluster ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image13.png){width="5.0in" height="2.6458333333333335in"}



![Amazon Amazon CloudWatch Amazon ECS ECS event stream AWS Lambda Events Amazon CloudWatch Events Amazon SNS Amazon Elasticsearch Service training and certification o Amazon SNS topic Monitor on dashboard Use Amazon ECS cluster-state information to drive container placement 0 Arna.•on Web O' Al. rese•ved ](../../../media/AWS-DevOps-Module-6-22-Amazon-Elastic-Container-Service-and-AWS-Fargate-image14.png){width="5.0in" height="2.826388888888889in"}
















