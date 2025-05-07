# 15 CI/CD Pipeline-2

Created: 2023-10-07 12:00:42 -0600

Modified: 2023-10-22 17:41:44 -0600

---

Summary

The text provides an in-depth walkthrough of AWS's CodeCommit service, highlighting its integration with other AWS services, the process of creating pull requests, merging code, and deploying changes using CodeDeploy, and the importance of best practices.

Facts

- CodeCommit offers a unified UI for all code services, simplifying navigation.
- The author has a repository named "cat-site" dedicated to sharing cat photos.
- Pull requests in CodeCommit allow merging changes from one branch to another.
- It's essential to provide detailed descriptions in pull requests for clarity and future reference.
- CodeCommit allows setting approval rules to ensure multiple reviewers approve changes before merging.
- CodeCommit doesn't support server-side hooks but can send notifications or triggers after merges.
- CodePipeline orchestrates the CI/CD process, guiding the flow from one stage to another.
- CodeBuild, a managed build environment, compiles and prepares the code for deployment.
- Build-spec files in CodeCommit instruct CodeBuild on the build process.
- Appspec files guide the deployment commands for CodeDeploy.
- CodeDeploy can perform blue-green deployments, creating a new environment and gradually shifting traffic from the old to the new.
- Blue-green deployments offer the flexibility to verify changes and easily rollback if issues arise.
- The author humorously teases a "fantastic cat website" but delays its reveal for a later demonstration.
