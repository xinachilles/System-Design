# 17 Developer tools -2 

Created: 2023-10-07 21:49:13 -0600

Modified: 2023-10-22 17:41:49 -0600

---

Summary

The content discusses various Jenkins plugins to enhance and integrate Jenkins with other services, especially within the AWS ecosystem, and details how each can be configured and used.

Facts

- Jenkins can be integrated with AWS CodePipeline via a specific plugin.
- Jenkins can be used in conjunction with CodePipeline to manage the CI/CD pipeline, with Jenkins handling the build process.
- The Jenkins environment shows only the status (failure or success) of a job, with detailed configurations available in the build configuration.
- Jenkins constantly polls CodePipeline to check if there's work available and runs builds when necessary.
- There's a plugin to help offload Jenkins build processes to AWS CodeBuild, especially useful when Jenkins struggles with scaling.
- CodeBuild can be configured to act as the build muscle, with Jenkins only triggering the build process.
- Another plugin, the EC2 plugin, allows Jenkins to scale its build nodes by spinning up EC2 instances.
- The EC2 plugin configurations are stored in Jenkins' configuration, not within the project.
- Idle EC2 instances are terminated after a specified timeout to optimize resource usage.
- While there are multiple plugins available, they are generally used independently and not combined in a nested manner.
- Other available plugins include integrations with CloudWatch logs, SQS, SNS, and container services like ECS to enhance Jenkins' capabilities within AWS.
