# Chapter 7- Transactions
A transaction is way to group several reads and writes into a logical unit.
Conceptually, all the reads and writes in a transaction are executed as one operation: either the entire transaction succeeds (commit) or it fails (abort, rollback). 
If it fails, the application can safely retry. With transactions, error handling becomes much simpler for an application, because it doesn’t need to worry about partial failure transactions were created to simplify the programming model for database read and writes.

## Meaning of ACID
Representations of the rules that transactions adhere to. Systems are either ACID compliant or not.
### Atomicity
Atomicity means something that cannot be broken down into further parts. 
In terms of multi-threading it means no thread can get an action which is half completed, such as primitive add, sub
In terms of transactions it means ability to abort a transaction on error and have all writes from that transaction
discarded is the defining feature of ACID atomicity(portability).

### Consistency
Refers to an application-specific notion of the
database being in a “good state.”
A good state, for example, is that if a payment transaction has been made, the amount subtracted from account 1
is consistent to the amount added to account 2.

### Isolation
1. If multiple users are concurrently reading or writing the same data, it should seem isolated, example, incrementing a counter.
2. Isolation in the sense of ACID means that concurrently executing transactions are
  isolated from each other: they cannot step on each other’s toes.

## Durability
Perfect durability does not exist.

## Single object and multi object operations
For multi object transactions we need some way of letting the backend know that a sequence of crud operations might be a complex transaction.
We can do that using BEGIN TRANSACTION and COMMIT keywords. There is no way to let a document db know we are handling a transaction.
## Single Object Write
Atomicity and isolation applies for single object writes as well. Let's say we have a large json object we need to update, anything can happen,
from data loss, network loss, etc. We can take a snapshot of the data before it was updated to abort in case of a dirty write.
## Weak Isolation Levels
We need isolation to mitigate concurrency bugs in transactions. One solution is serializable isolation but that has a performance cost.

## 1. Read committed
### No dirty reads 
A transaction has written something in db but has not yet committed it, if other transactions can see that data is a dirty read.
### No dirty writes 
When two transactions write to the same object and transaction 2 writes data to an uncommitted write by transaction 1
### Dirty write Example 
Alice and bob buy cars, Bob updates winning listing table so he is marked the owner, but Alice updates the invoice table so the payment is made on her name. Although the increment issue will remain but it is not a dirty write, it is a lost update.
Read committed is a very popular isolation level. It is the default setting in Oracle 11g, PostgreSQL, SQL Server 2012, MemSQL databases prevent dirty writes by using row-level locks dirty reads is prevented by making a snapshot of the data before a transaction.

## 2. Snapshot isolation
Read committed is not perfect, it can result in a one read skew and non-repeatable read, a onetime error. For example, Alice has 600 in bank 1 and transferred 100 to another account with 400. If Alice opens the second account when the transaction was taking place she will see 400 in that account even though account 1 has 500.
Snapshot isolation solves this by reading from a consistent snapshot.
Snapshot isolation supported by Postgres, MySQL, Oracle and SQL Server.
A key principle of snapshot isolation is readers never block writers, and writers never block readers.
snapshot isolation uses multiversion concurrency control to have different ids for transactions(always increasing) to make sure everything is sync.

## Visibility rules
1. At the start of each transaction, the db makes note of all transactions not yet completed, they are ignored even if they subsequently complete
2. Writes by aborted transactions are ignored.
3. Writes made by later transaction ids are ignored.
4. All other writes are visible.

## Preventing lost updates
Explicit locking of rows that are modified (for simple operations such as summation on field).
Compare and set (for complex operations such as searching the content of a Wikipedia page).

## Write skews and phantoms
Write skews are not dirty writes or lost updates, it is when two different objects are being updated based on an application condition.
It occurs because the condition might be concurrently read while the update transaction is happening. It’s a race condition.
Solution, explicitly lock the rows the transaction depends on using FOR UPDATE.

## Examples of write skew:
- booking a room.
- Changing your username.
- Multiplayer game.
- Preventing double spending

## Phantoms
This approach is called materializing conflicts, because it takes a phantom and turns it into a lock conflict on a concrete set of rows that exist in the database.

## Actual serial execution
With today’s cheap resources we can actually serially execute transactions. Serial executions is the best way to prevent concurrent problems. A single-threaded loop for executing transactions was feasible.
The approach of executing transactions serially is implemented in VoltDB/H-Store, Redis, and Datomic
We can Encapsulate transactions in stored procedures. implement business logic in stored procedures.

## Two Phase Locking(2PL)
Two phase locking is stricter then snapshot isolation; it has the following rules.
If a client is writing to an object, then neither can the object be written to, neither can it be read.
It is not very performant.











