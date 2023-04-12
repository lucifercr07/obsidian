- Ref: https://www.youtube.com/watch?v=09E-tVAUqQw 
- Both SQL/NoSQL DBs leverages B+ Trees to store data
- In B+ Trees rows or documents of a table are clubbed in B+ Tree nodes.
- ![[Pasted image 20230319200405.png]]
- Size of B+ Tree nodes ~= disk block size
- One table is just a collection of B+ Tree nodes on the disk
- Leaf nodes of B+ Tree contains the actual row datas.
- Every B+ tree nodes is serialized and stored on disk and non-leaf nodes hold the routing info.
- Insert, update, deletes, range queries are more efficient.