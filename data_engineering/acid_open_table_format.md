# ACID in Open Table Format
Open table formats like Delta Lake, Apache Iceberg, and Apache Hudi bring ACID (Atomicity, Consistency, Isolation, Durability) transactional guarantees to data lakes, enabling the Data Lakehouse architecture.
They achieve this by maintaining a log-structured metadata layer, tracking transaction history and snapshots, while applying consistency checks like Optimistic Concurrency Control (OCC) and schema enforcement to manage data files reliably.

## ACID properties
- Atomicity: Transactions either succeed or fail completely.
- Consistency: A transaction will bring data from one valid state to another
- Isolation: Prevention of collision from simultaneous operations.
- Durability: Once a transaction is committed, it is permanently store.

## How open table format achieve ACID
Property|How|Delta Lake|Apache Iceberg|Apache Hudi
--|--|--|--|--
Atomity|Any operation (write/update/delete) is treated as a transaction. Changes are committed only when the full operation succeeds.|Appends new commit entries to a transaction log (_delta_log).|Metadata file is compared and repointed to update snapshot|Time-based log and Write-Ahead-Log (WAL) are used to track successful transaction.
Consistency|Ensures incomplete/conflicting transactions are not committed. Schema and constraints are validated before commit. Snapshot isolation ensures readers see only consistent data.| | |Enforces primary key constraints
Isolation|Uses Optimistic Concurrency Control (OCC) to detect and resolve conflicts before commit. Allows safe concurrent transactions.| |Writers acquire short locks at commit time or use Non-Blocking Concurrency Control (NBCC). |File-level OCC and Multi-Version Concurrency Control (MVCC - allowing multi-version data files) allow concurrent writers to proceed if no overlapping changes.
Durability|All data and metadata are stored in fault-tolerant storage (e.g., S3, HDFS). Snapshots and logs can be replayed for recovery.| | |