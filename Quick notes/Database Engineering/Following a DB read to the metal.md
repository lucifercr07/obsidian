- Ref: https://medium.com/@hnasr/following-a-database-read-to-the-metal-a187541333c2
- Summary:
1.  DB issues read with correct offset and length that corresponds to the page to the file. DB knows this page and offset to be sent as it is aware of max page size i.e 8 KB in case of Postgres and 16KB in case of MySQL.
2.  Checked in DB buffer pool if not found go to OS and ask for page.
3.  OS maps the bytes to FS block address or LBAs, which is first checked in FS page cache, to see if there's a memory page with those blocks.
4.  If not found in Step 3 it goes to SSD controller and asks with read command.
5.  SSD controller converts the LBAs to physical address and then pulls the page in cache(SSD DRAM cache) and returns to the database.
6.  DB puts it in the buffer pool and starts processing the return the requested single row to user.

PS: The translation of LBAs to physical address done in step 5 is necessary as OS can't maintain the physical address of all the pages as SSD keeps moving data around when OS points to the physical location and offloading this moving data around mapping to host/OS in this case will add to system complexity.