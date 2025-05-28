# 13 Code whisper-2 

Created: 2023-10-04 21:55:19 -0600

Modified: 2023-10-22 17:41:27 -0600

---

Summary

Amazon Code Whisperer, integrated with Amazon CodeGuru, offers a security scanning feature that identifies vulnerabilities in code, highlighting issues such as unsanitized user inputs, logging of credentials, and resource leaks.



Facts

Amazon Code Whisperer provides real-time code suggestions in IDEs.

It can scan code to identify and define security vulnerabilities.

The security scan feature is integrated with Amazon CodeGuru.

CodeGuru filters multiple layers before scanning, focusing on critical issues.

Insecure code examples include:

Running unsanitized inputs as a sub-process, risking OS command injection.



Logging AWS credentials, which can expose them to attackers.

Not releasing allocated resources, leading to potential system slowdowns or crashes.



Code Whisperer's security scan provides detailed findings, linking directly to the problematic lines in the code.



The tool recommends sanitizing inputs, encrypting sensitive data before logging, and properly releasing allocated resources.



Regularly running Code Whisperer's security scan before checking in code can help in identifying and rectifying security vulnerabilities.



![Benefits of Amazon CodeWhisperer Automated code generation simplifies your development process by automating repetitive tasks and saving you time. It eliminates the need for you to invest excessive hours in exploring and learning new technologies, as you can rely on high-quality code suggestions that match your coding style. This approach enhances your productivity and enables you to focus on critical tasks, encouraging innovation and progress in software development. Value to developers • Increases velocity • Less time writing code • In IDE help with APIs • More secure code Value to developers. ](../../../media/AWS-DevOps-Module-3-13-Code-whisper-2-image1.png){width="5.0in" height="2.8333333333333335in"}



![With automated code generation, you can streamline your workflows and achieve significant time savings while ensuring the delivery of code that meets your standards. Value to organizations • All experience levels • Open-source attribution support • Reduce the risk of security vulnerabilities • Increase code quality and reliability Value to organizations Amazon CodeWhisperer code generation offers many benefits for software development organizations. It accelerates application development, for faster delivery of software solutions. By automating repetitive tasks, it optimizes the use of developer time, allowing developers to focus on more critical aspects of the project. Additionally, it helps mitigate security vulnerabilities, safeguarding the integrity of the codebase. ](../../../media/AWS-DevOps-Module-3-13-Code-whisper-2-image2.png){width="5.0in" height="2.8333333333333335in"}



![Amazon Code Whisperer also protects open source intellectual property, by providing the Open Source Reference Tracker. It enhances code quality and reliability, leading to robust and efficient applications. And it enables an efficient response to evolving software threats, keeping the codebase up to date with the latest security practices. Amazon CodeWhisperer has the potential to increase development speed, security, and the quality of software. That wraps-up Module 1 and the general overview of Developing on AWS. Next, you take a quick Knowledge Check to assess topics you may want to explore further. aws 02023, Amazon Web Services, Inc. or affliates. All rights reserved. ](../../../media/AWS-DevOps-Module-3-13-Code-whisper-2-image3.png){width="5.0in" height="2.9375in"}





