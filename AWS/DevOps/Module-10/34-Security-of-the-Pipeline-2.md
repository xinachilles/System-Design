# 34 Security of the Pipeline-2

Created: 2023-10-11 20:28:18 -0600

Modified: 2023-10-22 17:44:42 -0600

---



Summary

AWS Config provides a comprehensive inventory management toolset, allowing users to monitor resource compliance, enforce tagging structures, and set up automatic remediation for non-compliant resources.

Facts

- AWS Config Overview:
  - Offers insights into resource compliance within an AWS environment.
  - Identifies resources that are out of compliance.
  - Serves as an inventory management tool, detailing resource states.
- Tagging and Compliance:
  - Config has rules like "required-tags" to enforce proper tagging structures.
  - Helps identify resources without proper tagging, such as missing "cost center" tags.
  - Allows for automatic remediation, like deleting resources without required tags using "tag or die" policy.
  - Remediation can be set up through automation documents.
- Resource Management:
  - Users can select specific resources for remediation, e.g., terminating EC2 instances without proper tags.
  - Offers various compliance checks, like VPC flow logs and security group configurations.
  - Provides both pre-built rules and the option to add custom rules.
- Inventory Management:
  - Config allows users to view the compliance timeline of a resource.
  - Shows when a resource was activated, and its compliance status over time.
  - Lists all rules a resource is compliant or non-compliant with.
  - Provides a history of changes, relationships with other resources, and a comprehensive view of resource interactions.
- Configuration Management:
  - Config aids in understanding resource configurations and their compliance status.
  - Offers a centralized environment for querying and understanding resource configurations.





![Compliance and Resource Inventory After setup, AWS Config starts recording your specified resources and evaluating them against your rules. It may take a few minutes for AWS Config to display your resources, rules, conformance packs, and their compliance states. Conformance packs by compliance score Conformance packs by compliance score displays up to 10 of your conformance packs with the lowest compliance score. A compliance score is the percentage of the number of compliant rule- resource combinations in a conformance pack compared to the number of total possible rule- resource combinations in the conformance pack. This metric provides you with a high-level view of the compliance state of your conformance packs, and can be used to identify, investigate, and understand the level of compliance in your conformance packs. You can use the compliance score to track remediation progress, perform comparisons across different sets of requirements, and see the impact a specific change or deployment has on a conformance pack. ](../../../media/AWS-DevOps-Module-10-34-Security-of-the-Pipeline-2-image1.png){width="5.0in" height="3.8541666666666665in"}



![Compliance status Compliance status displays the number of your compliant and noncompliant rules and compliant and noncompliant resources. Resources are compliant or noncompliant based on an evaluation of the rule associated with it. If a resource does not follow the rule's specifications, the resource and the rule are flagged as noncompliant. Rules by noncompliant resources Rules by noncompliant resources displays your top noncompliant rules in descending order by the number of resources. Choose a rule to view its details, parameters, and the resources in scope for that specific rule. Resource inventory Resource inventory displays the total number of resources that AWS Config is recording in descending order by the number of resources, and the count of each resource type in your AWS account. To open all resources for a resource type, choose that resource type to go to its Resources inventory page. You can use the dropdown list to indicate which resource totals you want to view. By default, it is set to view All resources, but you can change it to AWS resources, Third-party resources, or Custom resources. ](../../../media/AWS-DevOps-Module-10-34-Security-of-the-Pipeline-2-image2.png){width="5.0in" height="4.256944444444445in"}


