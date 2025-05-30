# 5 Modifying an AWS CloudFormation Template



---

Summary

The text provides a demonstration of AWS CloudFormation's capabilities, emphasizing the importance of permissions, drift detection, updating resources, and importing manually created resources into CloudFormation.

Facts

- The user demonstrated the limitations of a user with only CloudFormation permissions, showing they can't interact with other AWS services.
- Despite having full CloudFormation permissions, the test user couldn't create a VPC, leading to a failed stack.
- Drift detection in CloudFormation helps identify when resources deviate from their intended configurations.
- The user showcased a security group change, where unauthorized ports were opened, emphasizing the need for monitoring and control.
- The demonstration highlighted the process of updating instance sizes in CloudFormation and the importance of reviewing change sets before applying updates.
- Change sets in CloudFormation allow users to preview the potential impact of their updates.
- CloudFormation now supports importing manually created resources, like DynamoDB tables and S3 buckets, into its management framework.





![What is drift? Drift detection enables you to detect whether a stack's actual configuration differs, or has drifted, from its expected configuration. Use CloudFormation to detect drift on an entire stack, or on individual resources within the stack. A resource is considered to have drifted if any of its actual property values differ from the expected property values. This includes if the property or resource has been deleted. A stack is considered to have drifted if one or more of its resources have drifted. To determine whether a resource has drifted, CloudFormation determines the expected resource property values, as defined in the stack template and any values specified as template parameters. CloudFormation then compares those expected values with the actual values of those resource properties as they currently exist in the stack. A resource is considered to have drifted if one or more of its properties have been deleted, or had their value changed. CloudFormation generates detailed information on each resource in the stack that has drifted. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image1.png)



![Drift detection status codes Flip each card below to learn more about the status codes CloudFormation assigns to stack drift detection operations. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image2.png)



![DETECTION COMPLETE ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image3.png)



![The stack drift detection operation has successfully completed for all resources in the stack that support drift detection. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image4.png)



![DETECTION FAILED ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image5.png)



![The stack drift detection operation has failed for at least one resource in the stack. Results will be available for resources on which CloudFormation successfully completed drift detection. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image6.png)



![DETECTION IN PROGRESS ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image7.png)



![The stack drift detection operation is currently in progress. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image8.png)



![DRIFTED ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image9.png)



![For stacks: The stack differs, or has drifted, from its expected template configuration. A stack is considered to have drifted if one or more of its resources have drifted. For stack instances: A stack instance is considered to have drifted if the stack associated with it has drifted. For stack sets: A stack set is considered to have drifted if one or more stack instances has drifted. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image10.png)



![NOT CHECKED ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image11.png)



![CloudFormation has not checked if the stack, stack set, or stack instance differs from its expected template configuration. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image12.png)



![IN_SYNC ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image13.png)



![The current configuration of each supported resource matches its expected template configuration. A stack, stack set, or stack instance with no resources that support drift detection will also have a status of IN SYNC. ](../../../media/AWS-DevOps-Module-2-5-Modifying-an-AWS-CloudFormation-Template-image14.png)
















