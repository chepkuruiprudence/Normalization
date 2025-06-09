# Database Normalization

Database normalization is the process of organizing the attributes of the database to reduce or eliminate data redundancy (having the same data but at different places). It helps improve database efficiency, consistency, and accuracy.

**Benefits:**

- Elimination of data redundancy.
- Ensuring data consistency.
- Simplification of data management.
- Improved database design.

## Why Do We Need Normalization?

- Insertion anomalies.
- Deletion anomalies.
- Update anomalies.

---

## Normal Forms in DBMS

| **Normal Form**                   | **Description**                                                                                                               |
| :-------------------------------- | :---------------------------------------------------------------------------------------------------------------------------- |
| **First Normal Form (1NF)**       | A relation is in 1NF if every attribute contains only atomic (indivisible) values.                                            |
| **Second Normal Form (2NF)**      | A relation in 1NF where every non-primary-key attribute is fully functionally dependent on the primary key.                   |
| **Third Normal Form (3NF)**       | A relation in 2NF with no transitive dependency for non-prime attributes. Either: X is a super key OR Y is a prime attribute. |
| **Boyce-Codd Normal Form (BCNF)** | In addition to being in 3NF, for every functional dependency X → Y, X must be a super-key.                                    |
| **Fourth Normal Form (4NF)**      | A relation in BCNF with no multi-valued dependencies.                                                                         |
| **Fifth Normal Form (5NF)**       | A relation in 4NF with no further non-loss decompositions (join dependencies).                                                |

---

## First Normal Form (1NF)

**A relation is in 1NF if:**

- All attributes contain atomic values.
- Each column has values of a single type.
- Each record is unique, identified by a primary key.
- No repeating groups or arrays in any row.

### Example:

**Before:**

| ID  | Name | Courses |
| --- | ---- | ------- |
| 1   | A    | C1, C2  |
| 2   | E    | C3      |
| 3   | M    | C2, C3  |

To make the table in 1NF we have to remove the multivalued attributes from the table as given below:

**After 1NF:**

| ID  | Name | Course |
| --- | ---- | ------ |
| 1   | A    | C1     |
| 1   | A    | C2     |
| 2   | E    | C3     |
| 3   | M    | C2     |
| 3   | M    | C3     |

---

## Second Normal Form (2NF)

**For a table to be in 2NF:**

1. It must be in 1NF.

- All columns contain single, indivisible values.
- No repeating groups of columns.

2. Eliminate partial dependencies — no non-prime attribute should depend on a part of a composite primary key.

### Example:

**Before:**

| STUD_NO | COURSE_NO | COURSE_FEE |
| :------ | :-------- | :--------- |
| 1       | C1        | 1000       |
| 2       | C2        | 1500       |
| 1       | C4        | 2000       |
| 4       | C3        | 1000       |
| 4       | C1        | 1000       |
| 2       | C5        | 2000       |

**Issues:**

- `COURSE_FEE` depends on `COURSE_NO` only, not on the composite key `{STUD_NO, COURSE_NO}` — causing a partial dependency.

**After 2NF:**

**COURSES Table:**

| COURSE_NO | COURSE_FEE |
| :-------- | :--------- |
| C1        | 1000       |
| C2        | 1500       |
| C3        | 1000       |
| C4        | 2000       |
| C5        | 2000       |

**STUDENT_COURSE Table:**

| STUD_NO | COURSE_NO |
| :------ | :-------- |
| 1       | C1        |
| 2       | C2        |
| 1       | C4        |
| 4       | C3        |
| 4       | C1        |
| 2       | C5        |

Now, each table is in 2NF:

- The Course Table ensures that COURSE_FEE depends only on COURSE_NO.
- The Student-Course Table ensures there are no partial dependencies because it only relates students to courses.
  Now, the COURSE_FEE is no longer repeated in every row, and each table is free from partial dependencies. This makes the database more efficient and easier to maintain.

---

## Third Normal Form (3NF)

**A table is in 3NF if:**

1. It is in 2NF.
2. No transitive dependency exists for non-prime attributes.

### Conditions for a Table to be in 3NF
A table is in Third Normal Form (3NF) if, for every non-trivial functional dependency X→Y, at least one of the following holds:

- X is a superkey: This means that the attribute(s) on the left-hand side of the functional dependency (X) must be a superkey (a key that uniquely identifies a tuple in the table).
- Y is a prime attribute: This means that every element of the attribute set Y must be part of a candidate key (i.e., a prime attribute).

**A functional dependency X → Y violates 3NF if:**

- X is not a super key and
- Y is not a prime attribute.

### Example:

**Before:**

| CAND_NO | CAND_NAME | CAND_STATE  | CAND_COUNTRY | CAND_AGE |
| :------ | :-------- | :---------- | :----------- | :------- |
| 1       | Amayra    | West Bengal | India        | 18       |
| 2       | Rihaan    | Haryana     | India        | 17       |
| 3       | Manya     | Haryana     | India        | 19       |

**Functional Dependencies:**

- `CAND_NO → CAND_NAME, CAND_STATE, CAND_AGE`
- `CAND_STATE → CAND_COUNTRY` (transitive dependency)

**After 3NF:**

**CANDIDATE Table:**

| CAND_NO | CAND_NAME | CAND_STATE  | CAND_AGE |
| :------ | :-------- | :---------- | :------- |
| 1       | Amayra    | West Bengal | 18       |
| 2       | Rihaan    | Haryana     | 17       |
| 3       | Manya     | Haryana     | 19       |

**STATE_COUNTRY Table:**

| CAND_STATE  | CAND_COUNTRY |
| :---------- | :----------- |
| West Bengal | India        |
| Haryana     | India        |

---

# ACID Properties in DBMS

ACID stands for Atomicity, Consistency, Isolation, and Durability. These four key properties define how a transaction should be processed in a reliable and predictable manner, ensuring that the database remains consistent, even in cases of failures or concurrent accesses.

**ACID stands for:**

- **Atomicity**: All operations of a transaction are completed; otherwise, none are.
- **Consistency**: Ensures database correctness after a transaction.
- **Isolation**: Ensures transactions occur independently.
- **Durability**: Changes made by a committed transaction persist.

## How ACID Properties Impact DBMS Design and Operation

- **Data Integrity and Consistency**
- **Recovery and Fault Tolerance**

| Property    | Responsible Component       |
| :---------- | :-------------------------- |
| Atomicity   | Transaction Manager         |
| Consistency | Application Programmer      |
| Isolation   | Concurrency Control Manager |
| Durability  | Recovery                    |

## Advantages of ACID Properties

- Data Consistency: ACID properties ensure that the data remains consistent and accurate after any transaction execution.
- Data Integrity: It maintains the integrity of the data by ensuring that any changes to the database are permanent and cannot be lost.
- Concurrency Control: ACID properties help to manage multiple transactions occurring concurrently by preventing interference between them.
- Recovery: ACID properties ensure that in case of any failure or crash, the system can recover the data up to the point of failure or crash.

## Disadvantages of ACID Properties

- Performance Overhead: ACID properties can introduce performance costs, especially when enforcing isolation between transactions or ensuring atomicity.
- Complexity: Maintaining ACID properties in distributed systems (like microservices or cloud environments) can be complex and may require sophisticated solutions like distributed locking or transaction coordination.
- Scalability Issues: ACID properties can pose scalability challenges, particularly in systems with high transaction volumes, where traditional relational databases may struggle under load.

# Examples of the Four ACID Properties

## Atomicity:

**Example:** Transfer of Ksh100 from Account X to Account Y.  
If the debit operation (T1) from Account X succeeds but the credit operation (T2) to Account Y fails, the entire transaction is rolled back so that no money is moved at all, avoiding partial updates.

## Consistency: Maintaining Valid Data States

**Example:** Total bank balance before a transaction is ksh 700 (Account X: ksh 500 + Account Y: ksh 200).  
After transferring 100 from X to Y, the total should remain ksh 700 (Account X: ksh 400 + Account Y: ksh 300).  
If the transaction violates rules (e.g., only debiting X without crediting Y), it is rejected, ensuring the database stays consistent.

## Isolation: Ensuring Concurrent Transactions Don't Interfere

**Example:**  
Transaction T transfers ksh 50 from Account X to Account Y (initially both 500).  
Transaction T'' simultaneously reads the balances and sums them. Without isolation, T'' might see outdated values (ksh 500 + ksh 500 = ksh 1000) while T is still running, leading to incorrect data.  
Isolation ensures T'' sees either the balances before T starts or after T commits, preventing inconsistent reads.

## Durability: Persisting Changes

**Example:** After a successful money transfer from Account A to Account B, the changes are saved to disk.  
Even if the system crashes immediately after, the data remains intact and recoverable, ensuring no data loss.

> **Note:** Database normalization and ACID properties are essential for ensuring data accuracy, minimizing redundancy, and handling transactions reliably, especially in high-volume or mission-critical systems.
