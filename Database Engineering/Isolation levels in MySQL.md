- Ref: https://mydbops.wordpress.com/2018/06/22/back-to-basics-isolation-levels-in-mysql/
- ![[Pasted image 20230321124012.png]]

### READ_UNCOMMITTED
1. No isolation in this level, no locks acquire.
2. One transaction can look at uncommitted changes of other transaction which leads to **DIRTY READS** , one transaction can't finish or rollback it's changes still other transaction will be able to see the changes done.
   
### READ_COMMITTED
1. In this isolation level other transaction will only see the committed value.
2. Here the value can keep changing within same transaction for a given `SELECT` statement. This phenomenon is called **non-repeatable reads.**
3. This is the default isolation level with most of popular RDBMS software, but not with MySQL.

### REPEATABLE_READ
1. Phenomenon on non-repeatable reads is avoided. It's default isolation in MySQL.
2. This isolation level returns same result throughout the transaction for a given `SELECT` statement.
3. Default isolation in MySQL.
4. Snapshot of the `SELECT` is taken the first time the `SELECT` is run during the transaction and the same snapshot is used throughout the transaction when the same `SELECT` is executed.
5. This isolation solves problem of non-repeatable reads but another problem of Phantom reads start here. A Phantom is a row that appears where it is not visible before. **InnoDB** and **XtraDB** solve the phantom read problem with multi-version concurrency control.
   ![[Pasted image 20230321141129.png]]

### SERIALIZABLE
1. Isolates the affects of one transaction from others.
2. Similar to repeatable read with additional restriction that whenever set of rows are selected in transaction it's unavailable for update by any other transaction. Only after one transaction completes other can proceed.
3. This is the strongest level of isolation available.
4. Not supported by AWS Aurora.