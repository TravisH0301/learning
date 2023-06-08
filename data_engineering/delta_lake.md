# Delta Lake
Delta Lake is an open source storage layer built on top of Parquet (a.k.a open table format).
Delta Lake provides ACID transactions, enalbing lakehouse architecture on data lakes.

- [Properties](#properties)
  - [Transaction log](#transaction-log)
    - [Checkpoint](#checkpoint)
  - [Optimistic concurrency control](#optimistic-concurrency-control)
  - [Concurrency operations](#concurrency-operations)
- [Benefits](#benefits)
  - [ACID transactions](#acid-transactions)
  - [Schema enforcement & evolution](#schema-enforcement--evolution)
  - [History & Time Travel](#history--time-travel)
  - [Upserts and deletes](#upserts-and-deletes)
- [Limitations](#limitations)
- [Optimisations](#optimisations)

## Properties
Delta Lake's transaction log and optimistic concurrency allow ACID transactions and provide time travel feature.
### Transaction log
Delta Lake has a transaction log that keep the entire historical audit trail of all operations executed on the table.
Only conflict-free transactions are committed to the transaction log.
#### Checkpoint
In Delta Lake, a `checkpoint` is a mechanism to provide a snapshot of the state of a Delta table at a particular point in time. It is a version of the transaction log that includes all the actions from the start of the table up to a specific version. It is stored as a Parquet file and thus, allows reading the state of the table in a single file read, rather than reading the entire transaction log.

Delta Lake uses a checkpoint retention policy to automatically manage checkpoints. By default, Delta Lake ensures that the last 10 checkpoints and all the checkpoints in the last 30 days are retained. This helps to ensure that the table can be read from a checkpoint even if some of the latest transaction log files are lost.

### Optimistic concurrency control
Optimistic concurrency control provides transactional guarantees between writes (conflict-free). Under this mechanism, writes operate in three steps:
- Read: Reads (if needed) the latest available version of the table to identify which files need to be modified
- Write: Stages all the changes by writing new data files
- Validate and commit: Before committing the changes, checks whether the proposed changes conflict with any other changes that may have been concurrently committed since the snapshot that was read. If there are no conflicts, all the staged changes are committed. However, if there are conflicts, the write operation fails.

### Concurrency operations

|                     | Read         | Insert       | Update/Delete/Merge | Compaction   |
|---------------------|--------------|--------------|---------------------|--------------|
| Read                | No conflict  |              |                     |              |
| Insert              | No conflict  | No conflict  |                     |              |
| Update/Delete/Merge | Can conflict | Can conflict | Can conflict        |              |
| Compaction          | No conflict  | No conflict  | Can conflict        | Can conflict |

*Conflicts can be avoided if executed in different partitions

## Benefits
### ACID transactions
Atomicity means that all transactions either succeed or fail completely.
- Delta Lake uses a `transaction log` to ensure atomicity
- During write operation, a new file is written
- As part of Delta Lake's `optimistic concurrency control`, any conflict is checked before a new log entry to the transaction log (= Commit)
- If conflict, new log entry to the transaction log is aborted (= Write operation is aborted due to conflict)
- Else, a new log entry is added to the transaction log that points to the new file
- Created but unused files can be cleaned up by Vacuum operation

Consistency guarantees relate to how a given state of the data is observed by simultaneous operations.
- Delta Lake achieves this through a mechanism known as `Snapshot Isolation`.
- Every time a transaction starts, it takes a snapshot of the current state of the data according to the transaction log
- All subsequent operations in that transaction are based on this snapshot.
- Hence, transactions processing in parallel will have their own consistent view of data regardless of each other's operations
- However, before any changes are commited, Delta Lake checks if any of the data that was read 
during the transaction has been modified by another transaction (Optimistic Concurrency Control)
- If it has, then the transaction is aborted and needs to be retried, preventing any inconsistencies from occurring. 
- Thus, the consistency of the data is maintained at all times.

Isolation refers to how simultaneous operations potentially conflict with one another.
- Delta Lake achieves this through `optimistic concurrency control`
- Every transactions checks if any of the data it read has been modified by another transaction before commit
- If it has, the transaction aborts, thus avoiding conflicts
- Likewise, this makes it as if transactions are executed in series

Durability means that committed changes are permanent.
- Transactions committed to the transaction log are permanent as the `transaction log is stored durably` in a reliable storage system (e.g., S3)
- In addition, Delta Lake also periodically `checkpoints` the state of the transaction log for faster recovery

### Schema enforcement & evolution
Automatically handles schema variations to prevent insertion of bad records during ingestion, unless evolution is specified.

### History & Time Travel
Delta Lake has a transaction log that contains all historical transactions made to the table. Additionally, Delta Lake can rollback to any 
past version shown on the audit log.

### Upserts and deletes

## Limitations
- Large storage: due to multiple versions of data - Setting storage `lifecycle` can help to move cold data into cheaper storage class

## Optimisations
![image](https://github.com/TravisH0301/learning/assets/46085656/dfe1f42f-ac9a-4245-be05-feda754e4ab2)

- Hive-like partitioning: data files are kept stored in different directories enabling data skipping
- Z-Ordering: Co-locates data files to enable data skipping
- Auto statistics collection
- Compaction: small data files are coalesced into larger ones, improving read query performance
- Data skipping: Based on statistics, partitioning and Z-ordering, data skipping enhances read query performance
- Caching: table can be cached in memory for subsequent reads
