- The application writes the log to the local disk 
- The logs collection service will watch the local disk and collect the logs, and send them to the Kafka cluster 
- Another service consumes from Kafka and moves the log to S3
	- uuid -> File 
	- File format: uuid=xxxx&logtype=xxx&appinstance=xxx&key=value (UUID will begin with a timestamp, allowing all entries to be sorted by UUID) 

