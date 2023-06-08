# Delta Lake
Delta Lake is an open source storage layer built on top of Parquet (a.k.a open table format).
Delta Lake provides ACID transactions, enalbing lakehouse architecture on data lakes.

ACID transactions on Spark: Serializable isolation levels ensure that readers never see inconsistent data.
Scalable metadata handling: Leverages Spark distributed processing power to handle all the metadata for petabyte-scale tables with billions of files at ease.
Schema enforcement: Automatically handles schema variations to prevent insertion of bad records during ingestion.
Time travel: Data versioning enables rollbacks, full historical audit trails, and reproducible machine learning experiments.
Upserts and deletes: Supports merge, update and delete operations to enable complex use cases like change-data-capture, slowly-changing-dimension (SCD) operations, streaming upserts, and so on.
## Benefits
### ACID transactions
Atomicity means that all transactions either succeed or fail completely.
- Delta Lake uses a `transaction log` to ensure atomicity. 
When writing data into the Delta Lake, first the new data is written to a new file. 
Then, a new log entry is added to the transaction log that points to the new file. 
This log entry is atomic - it either completely succeeds or fails. 
If the process crashes midway, the transaction log entry won't be created, so the new file is as if it never existed. 
It's only when the transaction log entry is successfully created that the new data becomes visible to readers.
And the created but not used files can be cleaned up by Vacuum operation.

Consistency guarantees relate to how a given state of the data is observed by simultaneous operations.
- Delta Lake achieves this through a mechanism known as `Snapshot Isolation`. 
It uses a transaction log to keep track of all changes made to the data. 
Every time a transaction starts, it takes a snapshot of the current state of the data according to the transaction log, 
and all subsequent operations in that transaction are based on this snapshot.
Consequently, even if there are other transactions making changes to the data concurrently, 
they do not affect the view of the data that the transaction has, because its view is based on the snapshot
 taken at the start of the transaction. 
This ensures a consistent view of the data throughout the transaction, even in the presence of concurrent operations.
Moreover, before any changes are committed, Delta Lake checks if any of the data that was read 
during the transaction has been modified by another transaction (Optimistic Concurrency Control). 
If it has, then the transaction is aborted and needs to be retried, preventing any inconsistencies from occurring. 
Thus, the consistency of the data is maintained at all times.

Isolation refers to how simultaneous operations potentially conflict with one another.
- Delta Lake provides serializability, the strongest level of isolation. 
This means each transaction is executed in isolation from other transactions, 
making the system act as if transactions were executed one after the other, serially, 
even though in reality they may be executed concurrently. 
Delta Lake achieves this through `optimistic concurrency control`, 
in which a transaction checks before committing if any of the data it read has been modified by another transaction. 
If it has, the transaction fails, thus avoiding conflicts.

Durability means that committed changes are permanent.
- Once a transaction has been committed and the transaction log has been updated, 
the changes made by the transaction are permanent, even in the face of system failures. 
This is because the `transaction log is stored durably` in a reliable storage system (such as HDFS, S3, Azure Blob Storage, etc.). 
In addition to this, Delta Lake also periodically `checkpoints` the state of the transaction log for faster recovery.

## Concurrency Control

## Other benefits

## Limitations
