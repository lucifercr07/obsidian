- Link: https://www.uber.com/en-IN/blog/postgres-to-mysql-migration/
- Summary:
  1. Article describes majorly regarding issues faced by Uber due to ways in which data is stored in Postgres.
  2. Majorly around following:
	- Inefficient architecture for writes
	- Inefficient data replication
	- Issues with table corruption
	- Poor replica MVCC support
	- Difficulty upgrading to newer releases
  3. One of the issues due to how Postgres stores tuple ids with indexes which directly point on disk offset of rows, and also the nature of immutable rows concept by Postgres where each new row update is treated as fresh new with new tuple id, updating both the old and new index with adding with `prev` as `null` for old index and new one pointing to new tuple id with `prev` field set to prior tuples id.
 4. Replication issues as WAL size was more due to multiple operations and causing out of sync replicas (Written very detailed in article)
 5. Data corruption due to a bug in Postgres 9.2. Couldn't understand much around this need to read more on this. Especially the part where they talk about two rows returning when a update happens with index due to different tuple ids but wouldn't old one be marked as inactive here?
 6. Replica MVCC support is not there which causes long running transactions on replicas to either be killed as it causes lag in sync with master.
7. Postgres upgrades not being supported replica syncs with older versions. e.g: A  master database running Postgres 9.3 cannot replicate to a replica running Postgres 9.2, nor can a master running 9.2 replicate to a replica running Postgres 9.3.
8. MySQL Architecture with InnoDB:
   - On disk representation uses Primary key of table itself which adds a two stage lookup but also helps in index updates and making WALs smaller and vaccuming and compacting efficient compared to Postgres.
   - 3 types of replication support in MySQL. And MySQL replication binary log is significantly smaller than Postgres WAL stream.
   - Postgres process spawning for each connection vs thread by MySQL which has evident advantages as IPC is costlier, more memory overhead and more context switches.