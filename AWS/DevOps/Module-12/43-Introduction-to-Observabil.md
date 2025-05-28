# 43 Introduction to Observability

Created: 2023-10-17 22:10:49 -0600

Modified: 2023-10-22 21:35:40 -0600

---

Summary

Observability is essential for understanding the performance and health of an application, and it encompasses logging, monitoring, and tracing. Monitoring, logging, and tracing are distinct components of observability that provide valuable insights into different aspects of application behavior.

Facts

- Observability includes more than just monitoring; it encompasses logging, monitoring, and tracing.
- Monitoring provides essential metrics such as CPU, memory usage, and network bandwidth to identify trends and make scaling decisions.
- Logging offers insights into application-level performance and behavior, highlighting issues that may not be evident from metrics alone.
- Tracing provides a detailed view of API calls and helps identify performance bottlenecks in microservices.
- The combination of monitoring, logging, and tracing creates a holistic observability framework, enabling data-driven decision-making.
- As architecture patterns evolve, traditional monitoring approaches may not scale to handle serverless, Lambda functions, and Fargate tasks.
- The "golden triangle of observability" includes monitoring, logging, and tracing, with each component serving a specific purpose in providing insights into application behavior.
- Effective observability is crucial for ensuring the performance and health of applications in complex, dynamic environments.



![Monitoring and observability have a close relationship. However, monitoring and observability have slightly different meanings and are not considered the same thing in the DevOps process. To have products that are observable, you must make sure that your products can be monitored. Monitoring consists of collecting, logging, and processing data. The monitoring describes the performance and health of the system. Observability means that you can infer the internal states of a system based on the external outputs. Observability is then achieved after that data has been collected from the monitoring phase. Finally, you can analyze your data and products to create a better solution for your business or IT challenges. After the observability, the user would have to translate the log metrics into meaningful insights. ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image1.png){width="5.0in" height="2.3854166666666665in"}



![Observability and monitoring • Observability is not the same as monitoring • Your pipeline/products must be able to be monitored, to have observability • After you achieve observability, you will receive feedback and can analyze your data • Feedback (analysis) is necessary for the CI/CD process ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image2.png){width="5.0in" height="1.8645833333333333in"}



![The golden triangle of observability What are the main components of observability? You may have heard of "The Golden Triangle of Observability," which includes the main components needed for effective observability. To learn more, choose each hotspot. ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image3.png){width="5.0in" height="1.7916666666666667in"}



![O O Monitoring (Metrics) Logging Tracing ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image4.png){width="5.0in" height="4.447916666666667in"}



![Monitoring (metrics) Monitoring and metrics is the most valuable or top priority of the triangle, because without that first step of information, you will not be able to achieve the other components. ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image5.png){width="5.0in" height="4.208333333333333in"}



![Logging Next in the triangle of observability is logging. Logging is important because it enables you to document and collect information that you have found while monitoring. You can use this in-depth information to make informed decisions on new iterations and updates. ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image6.png){width="5.0in" height="4.416666666666667in"}



![Tracing The last portion of the triangle is application trace data or traces. Think of traces as "traces" of data or information about a specific application. ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image7.png){width="5.0in" height="3.4895833333333335in"}



![To learn more, choose appropriate tab. METRICS LOGS TRACES Metrics are a numerical representation of data measured over time, useful for identifying trends and predictions. If a resource is unhealthy, metrics will show the trend and may allow you to predict when a node becomes unhealthy. Metrics training and certification • Numeric representation of data measured over intervals of time Monitoring • Used for identifying trends, modeling, and prediction 2020 Web All nghts ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image8.png){width="5.0in" height="4.458333333333333in"}



![To learn more, choose appropriate tab. METRICS LOGS TRACES Logs are immutable, time-stamped records of an event, and are useful for uncovering what happened. If a resource is unhealthy, a log is going to help answer why it became unhealthy. Logs Logging 2020 Ama.•on Web its Affiliates t/4hts training and certification • Immutable, timestamped record of the events • Used for uncovering emergent and unpredictable behavior ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image9.png){width="5.0in" height="4.395833333333333in"}



![To learn more, choose appropriate tab. METRICS LOGS TRACES Traces are useful for understanding the path a request takes or its flow, and is very user-centric. For example, if a resource is unhealthy, traces will tell you the impact on the user/request path. Typically, traces will provide insight on the OS or application level, showing "traces" of information of the request and the different paths it takes. Traces Tracing @ 2020 Web 0' its All nqhts reserved training and certification • Representation of series of distributed events that encode end-to-end request flow • Used to provide visibility in both the path and structure of request 10 ](../../../media/AWS-DevOps-Module-12-43-Introduction-to-Observability-image10.png){width="5.0in" height="4.395833333333333in"}










