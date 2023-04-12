- Ref: medium.com/a-5-years-tech-lead-said-they-shard-a-database-to-scale-but-then-he-failed-to-answer-this-question-8be39115dcb0
- Unless you tried all the possible options, otherwise, never shard your database (even for NoSQL which naturally support distributed system, do not shard until you have to).

### Option 1: Split table based on column update frequency
- Try sharding vertically i.e split the table based on columns which are frequently updated, this allows us to reduce data size to be queried for.
  ![[Pasted image 20230411225705.png]]


### Option 2: Archive data based on periods
- If the other table still keeps growing we can try to archive data based on periods.
- What if "Archiving" data is slow i.e in cases we might need to store data 5 years back too.


### Option 3: Split table based on Reads and Writes
- Then consider splitting the Reads and Writes using Master-Slave architecture.
  ![[Pasted image 20230411230343.png]]
- We can use [CQRS + event sourcing](https://www.baeldung.com/cqrs-event-sourcing-java) to implement above logic.
- In Splitting case if high through put is required we can create a cache for frequently read data and write through or write back mechanism can be used.
- What if it's still a fast-growing table, archiving too much data is not an option, and read-write is not enough to get the performance?


### Option 4: Table Partition
- Data will be split between physical files and managed by MySQL.
- Limitation, or the difference from sharding (range based) is, all partitions are on the same machine.
- We can add a disk, however after one point machine will become bottleneck.


### Option 5: Sharding but range based
- In our application logic layer, we can route the data to different databases based on logical ids.
- Example based on customer_id route to different databases.
- In this case if one of the machines has a problem, only affects the particular range of customers.
- What if in my case, the single customer grows very very huge. For that case we should choose keys carefully or we can also give dedicated databases to a given customer to resolve the hotspot issue.

### Option 6: Key based sharding on rows
- Based on row_id send to different machines.
  ![[Pasted image 20230411233737.png]]
- This approach has many cons:
  - Most of RDBMS features go away as now we can't use auto increment Primary keys, Foreign keys etc.
  - Multiple queries will need to be written.
  - Migration is pain in th a**.
  - When queries run across machines it's slow and tough to improve.
  - Transactions are hard to control.
- Pros:
  - No more data hotspot :')

### Conclusion
1. Better to avoid key based/horizontal sharding as much as possible. If not possible to avoid better to look into NoSQL DBs that is naturally distributed and mostly has more built-in features to support sharding.
2. If you have to use SQL like MySQL. use some proven engine like vitess(Used by Slack, YouTube).
3. Always try the simplest ways first.