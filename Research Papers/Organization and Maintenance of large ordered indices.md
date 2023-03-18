- Authors: R. Bayer and E. Mcreight
- Points to read about: Hash Indexes
- Introduction:
	1. Problem looking at is organisation and maintenance of index for dynamically changing random access file.
	2. Each index will hold (key, pointer to record)
	3. Assumption that index is of large volume and small parts only can be kept in main memory at a time, thus bulk of index is kept in some backup store like hard disks having long access or wait times.
	4. Since data file/database rows keep changing must be allowed to search/insert/delete elements economically.
	5. Index organisation described in paper will allow retrieval, insertion and deletion of key in logk(I) where I is size of index and k is page size.
	6. Index is organized in pages of fixed size capable of holding up to 2K keys but need to be only partially filled.
	7. Pages are nodes of speciallized **B-Tree**
	8. Advantage over Hash Indexing scheme:
		   - Storage is 50% compared to other scheme
		   - Storage is requested and released as the file grows and contracts.
		   - Natural order of the keys is maintained and allows processing based on order
- **==B-Trees==**
  1. `h` is the height of tree and `k` a natural number as index size. Properties would be: Each path from to leaf has same length `h` i.e height of the tree, Each node except the root and leaves has atleast `k + 1` sons, Each node has at most `2k + 1` sons.
  2. Number of nodes in a B-Tree: https://cs.stackexchange.com/a/87574/58492 (Not understood properly)
- Data Structure and Retrieval algorithm:
  1. Pages on which the index is stored are the nodes of B-Tree and can hold upto 2K keys.
  2. Following properties for data structure index:
     i) Each page holds k and 2k index elements except the root page which can hold b/2 1 and 2k keys.
     ii) If number of keys on a page `P` which is not leaf is `l`, then P has `l + 1` sons.
     iii) In each page keys are in sequential increasing order
     iv) 