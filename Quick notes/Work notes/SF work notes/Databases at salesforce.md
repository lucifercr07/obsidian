- Reference: https://drive.google.com/file/d/1285UX6xO0VpOsVmtWr_HYK52dXuKfK0T/view 
##### Storage options:
1. Relational DB
2. NoSQL
3. FileSystem, Datalakes, Data warehouse
4. Search
5. Persistent cache: Vagabond

##### Relational DBs:
- Most used
- Makes data modelling easy for engineers and customers
- Easier to code and query
- Joins, Transactions and Referential Integrity
- Performant on a single machine
- **Salesforce core:**
	- Underlying store is relational DB(Oracle, Sayonara/Postgres)
	- Entities (objects) are stored in this DB.
	- Entities built by Salesforce developers in Core:
		- Standard Entities (Legacy)
		- BPO (Base Platform Object):
			- No DB downtime
			- Uses SOQL for mapping
			- Implement referential integrity, indexes, unique constraints
	- Entities built by Customers:
		- Custom Entities(Similar to BPO)
	- Limits on number of fields in BPO entities(~1000 columns), Customer can create custom entities(800 fields).
	- Limits on number of fields in Standard entities(~1000 columns via Standard table), Customer can create custom entities(800 fields).
	- Limits on number of records in an entity based on visibility, transaction, # of fields. For BPO good till ~50 M records, Standard entities ~500 M records, HBPOs 10B+. Limits are in placed based on performance tests.
	- Partitioning based on any key or combination of keys. Partition function helps determine the where record is stored.
	- How does Salesforce Partition? - Primarily partition key is "OrgId". Every DB table has "OrgId" as first column.
		- Total 32 partitions, Mapping stored in config files. There are overrides in DB for later data movement for an org.
