# summary



---

**### Summary**

Amazon S3 Tables is a new managed service that provides optimized storage for tabular data using Apache Iceberg format, offering improved performance, automatic maintenance, and better security controls compared to traditional S3 bucket implementations.

**### Facts**

- S3 Tables is a purpose-built service for storing tabular data at scale, based on Apache Iceberg format

- Provides automatic compaction and snapshot management, eliminating manual maintenance tasks

- Apache Iceberg is a table format specification that can use Parquet as one of its underlying file formats, where Iceberg provides the table structure and metadata management while Parquet handles the actual data storage format.

- Iceberg is a table format that defines how data is organized and managed

- Parquet is a columnar storage file format that stores the actual data

- Iceberg can use multiple file formats, with Parquet being the most common

- The relationship works as follows:

- Iceberg manages the table structure, metadata, and transactions

- Parquet files store the actual data in a columnar format

- Iceberg's metadata layer tracks which Parquet files belong to which table

- Iceberg handles features like:

- Time travel

- Schema evolution

- Transaction management

- File organization

- Parquet handles:

- Data compression

- Columnar storage

- Data encoding

- File format specifics

This is why in S3 Tables, you can use Parquet files while getting the benefits of Iceberg's table management features. The combination provides both efficient storage (Parquet) and robust table management (Iceberg).



- Supports multiple file formats including Parquet, ORC, and Arrow (with some feature limitations for ORC and Arrow)

- Maintains 3 copies of data within a region for durability

- Integrates tightly with AWS services like Glue, Lake Formation, and EMR

- Enables fine-grained access control at table and column levels

- Supports time travel and transactional semantics

- Currently doesn't support versioning like traditional S3 buckets

- Doesn't automatically handle PII data - requires manual implementation

- Has limitations with data deletion due to time travel functionality

- Requires newer versions of AWS CLI and SDKs to work with

- Workshop for hands-on experience planned for release by end of month

- No automatic PII masking or special PII handling capabilities

- Data retention and deletion can be managed through snapshot retention policies
