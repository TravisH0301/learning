# GCP Professional Data Engineer Certificate

## Data Processing Concepts
### 3Vs of Big Data
- Volume: scale of information; MB, GB, TB, PB, ZB
- Velocity: speed which data gets processed; real-time, near real-time, batched, ad-hoc
- Variety: diversity of data sources, formats and quality; structured, unstructured 

### Data Warehouse vs. Data Lake
|Data Warehouse|Data Lake|
|---|---|
|Structured and processed|Raw or unstructured|
|Rigid schema|No forced schema|

### OLTP vs. OLAP
|OLTP|OLAP|
|---|---|
|Online Transactional Process|Online Analytical Process|
|Process large volume of small transactions|Process large volumes of historical data and complex analysis|
|Normalised|Denormalised|

## Data Storage
![image](https://user-images.githubusercontent.com/46085656/226648737-60528a6a-8575-4dba-b055-b99496c43329.png)

### Cloud Storage
- Unstructured object storage
- Buckets can be created by regional, dual-region or multi-region
  - Buckets exists within Projects and their name exist in a global namespace
- Provides different storage classes
  - Standard: Availability: 99.99% regional & >99.99% multi and dual-regions
  - Nearline: 30 days min. storage | Availability: 99.9% regional & 99.95% multi and dual-regions
  - Coldline: 90 days min. storage | Availabiity: 99.9% regional & 99.95% multi and dual-regions
  - Archive: 365 days min. storage | Availability: 99.9% regional & 99.95% multi and dual-regions
- Objects are stored as opaque data, are immutable and can be versioned
- Buckets and objects can be accessed via Google cloud console, HTTP API, SDKs, gsutil
- Storage event trigger
- Provides advanced features:
  - Paralle uploads of composite objects
  - Integrity checking (checksum generated when uploading & compared when downloading or periodically to detect file corruption)
  - Transcoding (compressed storage & decompressed when downloaded)
  - Requestor pays
- Costs:
  - Operational charges (upload, download, edit, etc.)
  - Network charges (when data egress across different servers)
  - Data retrieval charges (when downloading from Nearline or Coldline)
- Lifecycle Management: configuration of a bucket to periodically set storage classes or delete objects (ex. move objects from Standard to Nearline after 30 days)
- Security and Access Control:
  - IAM roles for bulk access to buckets
  - ACLs (Access control list) for granular access to buckets
  - Singed URLs for individual objects
  - Signed policy documents - to decide what kinds of files can be uploaded

### Cloud Bigtable
- Petabyte-scale NoSQL database
- High-throughput and scalability
- Wide column key/value data
- Time-series, transactional, IoT data

### BigQuery
- Petabyte-scale analytics data warehouse
- Fast SQL query across large datasets
- Foundation for BI and AI
- Provides public datasets

### Cloud Spanner
- Global SQL-based relational database
- Horizontal scalability and high availability
- Strong consistency suitable for financial transactions

### Cloud SQL
- Managed MySQL, SQL Server and PostgreSQL instances
- Built-in backups, replicas and failover
- Vertically scalable

### Cloud Firestore
- Fully-managed NoSQL document database
- Large collection of small JSON documents
- Realtime database with mobile SDKs
- Strong consistency

### Cloud Memorystore
- Managed Redis instances
- In-memory DB, cache or message broker
- Built-in high availability
- Vertically scalable

## Service Accounts
### Identity & Access Management (IAM)
#### Members
- Email accounts
#### Roles
- Instance admin
- Pub/Sub publisher
#### Human users
- Authenticates with their own credentials
- Should not be used for non-human operations


