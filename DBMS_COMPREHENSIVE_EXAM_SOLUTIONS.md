# Database Management Systems - Comprehensive Exam Solutions

---

## GROUP A - Multiple Choice Questions (10×1=10 marks)

### 1. DDL Command - (1) CO3 BL1
**Answer: a) Create**

- **CREATE** is a Data Definition Language (DDL) command used to define database schema
- **Other DDL commands:** ALTER, DROP, TRUNCATE
- **Update, Delete, Merge** are DML commands

---

### 1.i) Level of Data Abstraction - (1) CO1 BL1
**Answer: d) View Level**

**The three levels of data abstraction are:**
1. **Physical Level:** How data is actually stored
2. **Logical Level:** What data is stored and relationships
3. **View Level:** User view of database (highest level)

---

### 1.ii) True Statement - (1) CO3 BL1
**Answer: c) Every BCNF schema is also in 3NF**

**Explanation:**
- This is **TRUE**: BCNF is stricter than 3NF, so every BCNF relation is automatically in 3NF
- a) **False:** Super key may not be a candidate key
- b) **False:** Specialization is top-down approach
- d) Incorrect since c is true

---

### 1.iv) Foreign Key - (1) CO2 BL1
**Answer: a) Foreign key**

An attribute in one table that references the primary key of another table is called a **foreign key**. It establishes relationships between tables.

---

### 1.v) Dirty Read in DBMS - (1) CO4 BL1
**Answer: b) Reading uncommitted data**

**Dirty read** occurs when a transaction reads data written by another uncommitted transaction. This can lead to inconsistency if the writing transaction rolls back.

---

### 1.vi) Procedural Language - (1) CO2 BL1
**Answer: c) Relational algebra**

- **Relational algebra** is procedural (specifies how to obtain result)
- Domain calculus, Tuple calculus, and Query language (SQL) are more declarative

---

### 1.vii) File Storage Management - (1) CO5 BL1
**Answer: c) File manager**

- **File manager** handles physical storage of data on disk
- Database engine coordinates operations
- Query optimizer optimizes queries
- Transaction manager handles transactions

---

### 1.viii) Inconsistent State - (1) CO5 BL1
**Answer: d) Inconsistent state**

When database doesn't reflect real-world state accurately, it's **inconsistent**. Consistent, parallel, and atomic states represent proper database conditions.

---

### 1.ix) Concurrency Control Goal - (1) CO4 BL1
**Answer: c) To ensure data consistency and integrity in multi-user environments**

Primary goal is maintaining data consistency when multiple users access database simultaneously. Security, storage, and performance are secondary considerations.

---

### 1.x) File Organization - (1) CO5 BL1
**Answer: d) File Organization**

An organized logical sequence of records is called **file organization**. Methods include sequential, indexed, hashed organization.

---

### 1.xi) DBA Role - (1) CO1 BL1
**Answer: b) Managing database security and access**

DBA's primary role includes security, backup, recovery, and access control. Writing application code is developer's job, designing UI is designer's job, hardware creation is infrastructure team's job.

---

### 1.xii) Permanent Transaction - (1) CO4 BL1
**Answer: c) Commit**

- **COMMIT** makes transaction changes permanent
- **ROLLBACK** undoes changes
- **Flashback** recovers data
- **View** is a virtual table

---

## GROUP B - Short Answer Questions (3×5=15 marks)

### 2. Three-Schema Architecture - (5) CO1 BL2

Three-Schema Architecture provides data independence through three levels:

#### **1. External/View Level:**
- User-specific views of database
- Different users see different portions
- Provides logical data independence
- **Example:** Students see only their grades, faculty see all grades

#### **2. Conceptual/Logical Level:**
- Entire database structure for all users
- Describes what data is stored and relationships
- Entity-relationship model
- Independent of physical storage
- **Example:** STUDENT(ID, Name, Grade), COURSE(CourseID, Title)

#### **3. Internal/Physical Level:**
- How data is physically stored
- File organization, indexing, access paths
- Storage allocation and compression
- **Example:** B-tree indexes, hash files, disk blocks

#### **Benefits:**
1. **Logical Data Independence:** Changes to conceptual schema don't affect external schemas
2. **Physical Data Independence:** Changes to physical storage don't affect conceptual schema
3. Security and authorization at different levels
4. Multiple user views possible

---

### 3. Relational Algebra vs Relational Calculus - (5) CO2 BL4

#### **Relational Algebra:**
- **Procedural language:** Specifies **HOW** to retrieve data
- Sequence of operations performed on relations
- **Operators:** SELECT (σ), PROJECT (π), UNION (∪), CARTESIAN PRODUCT (×), JOIN (⋈)
- **Example:** 
  ```
  π(Name)(σ(Age>20)(STUDENT))
  
  1. First select students with age > 20
  2. Then project only names
  ```

#### **Relational Calculus:**
- **Declarative/Non-procedural:** Specifies **WHAT** data to retrieve
- Based on mathematical logic
- **Two types:** Tuple Relational Calculus (TRC), Domain Relational Calculus (DRC)
- **Example (TRC):**
  ```
  {t | t ∈ STUDENT ∧ t.Age > 20}
  
  Describes the result set without specifying steps
  ```

#### **Key Differences:**

| Aspect | Relational Algebra | Relational Calculus |
|--------|-------------------|---------------------|
| **Nature** | Procedural | Declarative |
| **Focus** | How to get data | What data to get |
| **Operations** | Step-by-step | Logical conditions |
| **Basis** | Set operations | Predicate logic |
| **Equivalence** | Both are equivalent in expressive power | |

---

### 4. BCNF and 3NF Relationship - (5) CO3 BL4

**Statement Analysis:**
*"Every relation in BCNF is also in 3NF; however, a relation in 3NF is not necessarily in BCNF"*

#### **Explanation:**

**Third Normal Form (3NF):**
- No transitive dependencies
- For every FD X→Y, either:
  - X is a superkey, **OR**
  - Y is a prime attribute (part of candidate key)

**Boyce-Codd Normal Form (BCNF):**
- Stricter than 3NF
- For every FD X→Y:
  - X **must** be a superkey (no exceptions)

#### **Why BCNF ⊆ 3NF:**
- BCNF has stricter conditions
- Every relation satisfying BCNF automatically satisfies 3NF
- BCNF doesn't allow the exception for prime attributes

#### **Why 3NF ⊄ BCNF (Example):**

**Consider relation:** BOOKING(Hotel, Guest, RoomType)

**Functional Dependencies:**
- {Hotel, Guest} → RoomType
- RoomType → Hotel

**Candidate Keys:** {Guest, RoomType}, {Hotel, Guest}

**Analysis:**
- RoomType → Hotel violates BCNF (RoomType is not a superkey)
- But it's in 3NF (Hotel is a prime attribute)

#### **Conclusion:**
- All BCNF relations are in 3NF
- Some 3NF relations are not in BCNF
- BCNF eliminates all redundancy based on functional dependencies
- 3NF allows some controlled redundancy to preserve dependencies

---

## GROUP C - Long Answer Questions (3×15=45 marks)

### 5. ACID Properties of Transactions - (15) CO4 BL4

ACID Properties ensure reliable transaction processing:

---

#### **A - Atomicity:**

**Definition:** Transaction is "all or nothing"
- Either all operations complete successfully or none do
- If failure occurs, system rolls back to previous state

**Example: Money transfer between accounts**
```
- Debit from Account A: ₹1000
- Credit to Account B: ₹1000
- If credit fails, debit must be rolled back
```

**Implementation:** Using transaction log and rollback mechanisms

---

#### **C - Consistency:**

**Definition:** Database moves from one valid state to another
- All integrity constraints must be maintained

**Example: In banking system**
```
Before: A=₹5000, B=₹3000, Total=₹8000
After transfer: A=₹4000, B=₹4000, Total=₹8000
Total remains constant (consistency rule)
```

**Implementation:** Constraint checking, triggers

---

#### **I - Isolation:**

**Definition:** Concurrent transactions don't interfere with each other
- Each transaction appears to execute alone
- **Isolation Levels:** Read Uncommitted, Read Committed, Repeatable Read, Serializable

**Example: Two users booking same airline seat**
```
Transaction 1: Check availability → Book seat
Transaction 2: Check availability → Book seat
With proper isolation, only one succeeds
```

**Implementation:** Locking protocols, timestamps

---

#### **D - Durability:**

**Definition:** Once transaction commits, changes are permanent
- Survives system failures

**Example:** After ATM confirms withdrawal, power failure shouldn't reverse it

**Implementation:** Write-ahead logging, database backup and recovery

---

#### **Transaction States:**
1. **Active:** Transaction executing
2. **Partially Committed:** After final operation
3. **Committed:** Successfully completed
4. **Failed:** Cannot proceed normally
5. **Aborted:** Rolled back to previous state

---

### 6. ER Diagram for Library Management System - (15) CO2 BL5

#### **Entities and Attributes:**

**1. BOOK**
- BookID (Primary Key)
- ISBN
- Title
- Author
- Publisher
- Edition
- Category
- Total_Copies
- Available_Copies

**2. MEMBER**
- MemberID (Primary Key)
- Name
- Type (Student/Faculty/Staff)
- Department
- Contact
- Email
- Address
- Join_Date

**3. LIBRARIAN**
- LibrarianID (Primary Key)
- Name
- Contact
- Email
- Shift

**4. TRANSACTION**
- TransactionID (Primary Key)
- Issue_Date
- Due_Date
- Return_Date
- Fine_Amount
- Status

---

#### **Relationships:**

**1. BORROWS (Member ↔ Book)**
- Many-to-Many relationship
- Member can borrow multiple books
- Book can be borrowed by multiple members (at different times)
- Creates TRANSACTION entity

**2. MANAGES (Librarian ↔ Transaction)**
- One-to-Many relationship
- One librarian manages many transactions

**3. RESERVES (Member ↔ Book)**
- Many-to-Many relationship
- Optional: for book reservation

---

#### **ER Diagram:**

```
[MEMBER] ---< BORROWS >--- [BOOK]
   |1                          |
   |                           |
   |                           |
   M                           M
   |                           |
[TRANSACTION] >---< MANAGES ---[LIBRARIAN]
                    M         1
```

---

#### **Additional Features:**
- **Fine Calculation:** Automated based on overdue days
- **Book Search:** By title, author, ISBN, category
- **Member History:** Track borrowing history
- **Inventory Management:** Track available vs issued books

---

### 7. Hotel Booking System - Relational Algebra Queries - (15 marks total)

**Given Relations:**
- **HOTEL**(hotelno, name, address)
- **ROOM**(roomno, hotelno, type, price_pn)
- **BOOKING**(hotelno, guestno, dateform, dateto, roomno)
- **GUEST**(guestno, name, address)

---

#### **a) Duties of Database Administrator - (3) CO1 BL3**

**1. Schema Definition and Management:**
- Define database schema and structure
- Create and modify tables, views, indexes
- Ensure proper normalization

**2. Security and Access Control:**
- Define user roles and permissions
- Implement authentication and authorization
- Monitor unauthorized access attempts
- Encrypt sensitive data

**3. Backup and Recovery:**
- Schedule regular backups
- Develop disaster recovery plans
- Test recovery procedures
- Maintain backup logs

**4. Performance Monitoring and Tuning:**
- Monitor query performance
- Optimize slow queries
- Manage indexes
- Configure database parameters
- Monitor resource utilization (CPU, memory, disk)

**5. Data Integrity and Quality:**
- Enforce integrity constraints
- Validate data quality
- Handle data migration
- Ensure referential integrity

**6. Capacity Planning:**
- Forecast storage requirements
- Plan for scalability
- Manage disk space

**7. Documentation:**
- Maintain system documentation
- Document procedures and policies
- Keep change logs

**8. User Support:**
- Assist users with database issues
- Provide training
- Resolve access problems

---

#### **b) Relational Algebra Queries - (12) CO3 BL6**

**i) List all hotels situated in Kolkata:**

```
π(hotelno, name, address)(σ(address='Kolkata')(HOTEL))
```

**Explanation:**
- σ (SELECT): Filter hotels where address = 'Kolkata'
- π (PROJECT): Display hotelno, name, address
- **Result:** All hotel details in Kolkata

---

**ii) List all single rooms with charge below Rs. 1000 per night:**

```
π(roomno, hotelno, type, price_pn)(σ(type='single' ∧ price_pn<1000)(ROOM))
```

**Explanation:**
- σ: Filter rooms where type is 'single' AND price < 1000
- π: Project all room attributes
- **Result:** Single rooms under Rs. 1000/night

---

**iii) List names of all guests staying at ITC Hotel from 25th December to 1st January:**

```
π(name)(GUEST ⋈(guestno) 
  (σ(name='ITC Hotel' ∧ dateform='25-Dec' ∧ dateto='1-Jan')
    (BOOKING ⋈(hotelno) HOTEL)))
```

**Step-by-step:**
1. Join HOTEL with BOOKING on hotelno
2. Filter: hotel name = 'ITC Hotel' AND date range matches
3. Join result with GUEST on guestno
4. Project guest names

**Alternative clearer notation:**
```
TEMP1 ← σ(name='ITC Hotel')(HOTEL)
TEMP2 ← BOOKING ⋈(hotelno) TEMP1
TEMP3 ← σ(dateform='25-Dec-2024' ∧ dateto='1-Jan-2025')(TEMP2)
RESULT ← π(name)(GUEST ⋈(guestno) TEMP3)
```

---

**iv) List price per night and type of all rooms at Grand Hotel:**

```
π(price_pn, type)(σ(name='Grand Hotel')(HOTEL ⋈(hotelno) ROOM))
```

**Explanation:**
- Join HOTEL with ROOM on hotelno
- Filter: hotel name = 'Grand Hotel'
- Project price_pn and type
- **Result:** Room types and prices at Grand Hotel

---

**v) List all guests currently staying at Taj Hotel:**

```
π(name)(GUEST ⋈(guestno) 
  (σ(name='Taj Hotel' ∧ dateto≥CURRENT_DATE ∧ dateform≤CURRENT_DATE)
    (BOOKING ⋈(hotelno) HOTEL)))
```

**Explanation:**
- Join HOTEL with BOOKING
- Filter: hotel = 'Taj Hotel' AND current date is between dateform and dateto
- Join with GUEST to get guest names
- Project guest names

**Note:** "Currently staying" means:
- dateform ≤ today's date (already checked in)
- dateto ≥ today's date (not yet checked out)

---

### 8. Normalization and Normal Forms - (15 marks total)

#### **a) What is Normalization? Types of Anomalies - (5) CO3 BL6**

**Normalization:**

Normalization is the process of organizing data in a database to reduce redundancy and improve data integrity. It involves decomposing tables into smaller tables and defining relationships between them according to rules designed to protect data and make the database more flexible.

**Objectives:**
1. Eliminate redundant data
2. Ensure data dependencies make sense
3. Reduce storage space
4. Prevent data anomalies

---

**Types of Anomalies:**

**1. Insertion Anomaly:**
- Cannot insert data without presence of other data

**Example:**
```
Table: STUDENT_COURSE(StudentID, StudentName, CourseID, CourseName)
- Cannot add a new course unless a student enrolls
- Cannot add student without course enrollment
```

**Problem:** Incomplete information cannot be stored

---

**2. Deletion Anomaly:**
- Deleting data causes unintended loss of other data

**Example:**
```
Same table as above
- If last student drops a course, course information is lost
- Deleting student record removes course details
```

**Problem:** Loss of important information

---

**3. Update Anomaly:**
- Updating data in one place requires updates in multiple places
- Inconsistency if not all instances are updated

**Example:**
```
- Table stores student name with each course enrollment
- If student changes name, must update multiple rows
- If missed, database becomes inconsistent
```

**Problem:** Data inconsistency and redundancy

**Solution:** Proper normalization eliminates these anomalies by decomposing tables appropriately.

---

#### **b) Different Normal Forms with Examples - (10) CO3 BL5**

### **1. First Normal Form (1NF):**

**Rules:**
- Each column contains atomic (indivisible) values
- Each column contains values of single type
- Each column has unique name
- Order doesn't matter

**Example - Not in 1NF:**
```
STUDENT
StudentID | Name    | Courses
----------|---------|------------------
S1        | John    | Math, Physics
S2        | Mary    | Chemistry
```

**Converted to 1NF:**
```
STUDENT
StudentID | Name | Course
----------|------|----------
S1        | John | Math
S1        | John | Physics
S2        | Mary | Chemistry
```

---

### **2. Second Normal Form (2NF):**

**Rules:**
- Must be in 1NF
- No partial dependencies (non-prime attributes fully dependent on entire candidate key)
- Applies to tables with composite primary keys

**Example - In 1NF but not 2NF:**
```
STUDENT_COURSE
StudentID | CourseID | StudentName | CourseName | Grade
----------|----------|-------------|------------|-------
S1        | C1       | John        | Math       | A
S1        | C2       | John        | Physics    | B

Primary Key: {StudentID, CourseID}
Partial Dependencies:
- StudentID → StudentName (partial)
- CourseID → CourseName (partial)
```

**Converted to 2NF (Decompose):**
```
STUDENT                    COURSE                  ENROLLMENT
StudentID | StudentName    CourseID | CourseName   StudentID | CourseID | Grade
----------|------------    ---------|------------  ----------|----------|-------
S1        | John           C1       | Math         S1        | C1       | A
S2        | Mary           C2       | Physics      S1        | C2       | B
```

---

### **3. Third Normal Form (3NF):**

**Rules:**
- Must be in 2NF
- No transitive dependencies (non-prime attributes don't depend on other non-prime attributes)

**Example - In 2NF but not 3NF:**
```
STUDENT
StudentID | StudentName | DeptID | DeptName | DeptHead
----------|-------------|--------|----------|----------
S1        | John        | D1     | CS       | Dr. Smith
S2        | Mary        | D1     | CS       | Dr. Smith

Primary Key: StudentID
Transitive Dependency: StudentID → DeptID → DeptName, DeptHead
```

**Converted to 3NF:**
```
STUDENT                        DEPARTMENT
StudentID | StudentName | DeptID    DeptID | DeptName | DeptHead
----------|-------------|------     -------|----------|----------
S1        | John        | D1        D1     | CS       | Dr. Smith
S2        | Mary        | D1        D2     | EE       | Dr. Jones
```

---

### **4. Boyce-Codd Normal Form (BCNF):**

**Rules:**
- Must be in 3NF
- For every functional dependency X→Y, X must be a superkey
- Stricter version of 3NF

**Example - In 3NF but not BCNF:**
```
BOOKING
StudentID | Subject | Professor
----------|---------|----------
S1        | DB      | Dr. A
S1        | OS      | Dr. B
S2        | DB      | Dr. A

Assumptions:
- Each student can enroll in multiple subjects
- Each subject is taught by only one professor
- A professor can teach multiple subjects

Functional Dependencies:
- {StudentID, Subject} → Professor (candidate key)
- Professor → Subject (violates BCNF! Professor is not a superkey)
```

**Converted to BCNF:**
```
STUDENT_SUBJECT           PROFESSOR_SUBJECT
StudentID | Professor     Professor | Subject
----------|----------     ----------|--------
S1        | Dr. A         Dr. A     | DB
S1        | Dr. B         Dr. B     | OS
S2        | Dr. A
```

---

### **5. Fourth Normal Form (4NF):**

**Rules:**
- Must be in BCNF
- No multi-valued dependencies

**Example - In BCNF but not 4NF:**
```
STUDENT_SKILL_HOBBY
StudentID | Skill      | Hobby
----------|------------|----------
S1        | Java       | Cricket
S1        | Java       | Chess
S1        | Python     | Cricket
S1        | Python     | Chess

Multi-valued dependencies: 
- StudentID →→ Skill
- StudentID →→ Hobby
Skills and hobbies are independent
```

**Converted to 4NF:**
```
STUDENT_SKILL            STUDENT_HOBBY
StudentID | Skill        StudentID | Hobby
----------|----------    ----------|--------
S1        | Java         S1        | Cricket
S1        | Python       S1        | Chess
```

---

### 9. Aggregate Functions, Foreign Key, Join, Two-Phase Locking - (15) CO5 BL5

#### **Aggregate Functions:**

Aggregate functions perform calculations on sets of values and return a single value.

**Common Aggregate Functions:**

**1. COUNT():** Counts number of rows
```sql
SELECT COUNT(*) FROM STUDENT;
SELECT COUNT(DISTINCT Department) FROM STUDENT;
```

**2. SUM():** Calculates sum of numeric column
```sql
SELECT SUM(Salary) FROM EMPLOYEE;
SELECT SUM(Price * Quantity) FROM SALES;
```

**3. AVG():** Calculates average
```sql
SELECT AVG(Marks) FROM RESULT;
SELECT AVG(Salary) FROM EMPLOYEE WHERE Dept='CS';
```

**4. MAX():** Returns maximum value
```sql
SELECT MAX(Salary) FROM EMPLOYEE;
SELECT MAX(Price) FROM PRODUCT;
```

**5. MIN():** Returns minimum value
```sql
SELECT MIN(Age) FROM STUDENT;
```

**GROUP BY Clause:**
```sql
SELECT Department, AVG(Salary)
FROM EMPLOYEE
GROUP BY Department;
```

**HAVING Clause (filter after grouping):**
```sql
SELECT Department, AVG(Salary)
FROM EMPLOYEE
GROUP BY Department
HAVING AVG(Salary) > 50000;
```

---

#### **Foreign Key:**

A foreign key is a column (or set of columns) that creates a link between data in two tables.

**Characteristics:**
- References primary key of another table
- Enforces referential integrity
- Can have NULL values (unless specified NOT NULL)
- Can have duplicate values

**Example:**
```sql
CREATE TABLE STUDENT (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50),
    DeptID INT,
    FOREIGN KEY (DeptID) REFERENCES DEPARTMENT(DeptID)
);
```

**Referential Actions:**
- **ON DELETE CASCADE:** Delete related records
- **ON DELETE SET NULL:** Set foreign key to NULL
- **ON UPDATE CASCADE:** Update related records
- **ON DELETE RESTRICT:** Prevent deletion if referenced

---

#### **Join Operations:**

Joins combine rows from two or more tables based on related columns.

**1. INNER JOIN (or JOIN):**
Returns matching rows from both tables
```sql
SELECT S.Name, D.DeptName
FROM STUDENT S
INNER JOIN DEPARTMENT D ON S.DeptID = D.DeptID;
```

**2. LEFT JOIN (LEFT OUTER JOIN):**
Returns all rows from left table, matching rows from right
```sql
SELECT S.Name, E.CourseID
FROM STUDENT S
LEFT JOIN ENROLLMENT E ON S.StudentID = E.StudentID;
```

**3. RIGHT JOIN (RIGHT OUTER JOIN):**
Returns all rows from right table, matching rows from left
```sql
SELECT S.Name, E.CourseID
FROM STUDENT S
RIGHT JOIN ENROLLMENT E ON S.StudentID = E.StudentID;
```

**4. FULL OUTER JOIN:**
Returns all rows when match in either table
```sql
SELECT S.Name, E.CourseID
FROM STUDENT S
FULL OUTER JOIN ENROLLMENT E ON S.StudentID = E.StudentID;
```

**5. CROSS JOIN:**
Cartesian product of both tables
```sql
SELECT *
FROM TABLE1
CROSS JOIN TABLE2;
```

**6. SELF JOIN:**
Table joined with itself
```sql
SELECT E1.Name AS Employee, E2.Name AS Manager
FROM EMPLOYEE E1, EMPLOYEE E2
WHERE E1.ManagerID = E2.EmployeeID;
```

---

#### **Two-Phase Locking Protocol (2PL):**

Two-Phase Locking is a concurrency control protocol that ensures serializability.

**Two Phases:**

**1. Growing Phase (Lock Acquisition):**
- Transaction may acquire locks
- Cannot release any locks
- Can request new locks

**2. Shrinking Phase (Lock Release):**
- Transaction may release locks
- Cannot acquire new locks
- Once first lock is released, only releases allowed

**Lock Types:**
- **Shared Lock (S):** For read operations, multiple transactions can hold
- **Exclusive Lock (X):** For write operations, only one transaction can hold

**Rules:**
1. Before reading data item A, transaction must obtain S-lock or X-lock on A
2. Before writing data item A, transaction must obtain X-lock on A
3. Transaction cannot request additional locks after releasing any lock
4. Locks released at transaction end (commit/abort)

**Example:**
```
Transaction T1:
1. lock-S(A)    // Growing phase begins
2. read(A)
3. lock-X(B)    // Still growing
4. read(B)
5. write(B)
6. unlock(A)    // Shrinking phase begins
7. unlock(B)    // Only releases allowed now
```

**Types of 2PL:**

**1. Basic 2PL:**
- Follows two phases
- May cause deadlocks
- No cascading rollback guarantee

**2. Conservative/Static 2PL:**
- Acquires all locks at start
- Deadlock-free
- Reduces concurrency

**3. Strict 2PL:**
- Holds exclusive locks until commit/abort
- Prevents cascading rollbacks
- Most commonly used

**4. Rigorous 2PL:**
- Holds all locks (shared and exclusive) until commit/abort
- Simplifies recovery

**Advantages:**
- Guarantees conflict serializability
- Simple to implement
- Widely used in commercial DBMS

**Disadvantages:**
- May cause deadlocks
- Reduces concurrency
- Can lead to starvation

---

## 10. Discussion Topics (Any 3 out of 4) - (15) CO4 BL6

### **a) 4NF (Fourth Normal Form):**

**Definition:**
A relation is in 4NF if it is in BCNF and has no multi-valued dependencies.

**Multi-valued Dependency (MVD):**
X →→ Y means for each value of X, there is a set of values for Y independent of other attributes.

**Formal Definition:**
For MVD X →→ Y to exist:
- For each X value, Y has a set of values independent of other attributes
- X →→ Y is trivial if Y ⊆ X or XY = R

**4NF Rule:**
For every non-trivial MVD X →→ Y, X must be a superkey.

**Example Problem:**
```
EMPLOYEE_SKILL_LANGUAGE
EmpID | Skill    | Language
------|----------|----------
E1    | Java     | English
E1    | Java     | Hindi
E1    | Python   | English
E1    | Python   | Hindi
```

**Multi-valued Dependencies:**
- EmpID →→ Skill (employee's skills independent of languages)
- EmpID →→ Language (employee's languages independent of skills)

**Problem:** Redundancy - if employee knows 3 skills and 2 languages, need 6 rows!

**Conversion to 4NF (Decompose):**
```
EMPLOYEE_SKILL          EMPLOYEE_LANGUAGE
EmpID | Skill           EmpID | Language
------|----------       ------|----------
E1    | Java            E1    | English
E1    | Python          E1    | Hindi
```

**Benefits:**
- Eliminates redundancy
- No update anomalies
- Easier maintenance
- Better storage efficiency

**When to use 4NF:**
- When attributes are independently multi-valued
- To eliminate redundancy from repeated combinations
- When BCNF still has anomalies

---

### **b) Advantages of Concurrency Control:**

Concurrency control allows multiple transactions to execute simultaneously while maintaining database consistency.

**1. Increased Throughput:**
- Multiple transactions execute in parallel
- Better CPU and I/O utilization
- Reduced average response time
- **Example:** While T1 waits for disk I/O, T2 can use CPU

**2. Reduced Waiting Time:**
- Short transactions don't wait for long ones
- Improved system responsiveness
- Better user experience
- **Example:** Balance inquiry doesn't wait for batch processing

**3. Better Resource Utilization:**
- CPU, memory, and disk used efficiently
- Reduced idle time
- Maximized system throughput
- **Example:** One transaction reads while another writes

**4. Improved System Performance:**
- More transactions completed per unit time
- Higher transaction throughput
- Better scalability
- **Example:** ATM network handles thousands of concurrent transactions

**5. Fairness:**
- All users get fair chance to execute
- Prevents monopolization by single user
- Balanced resource allocation
- **Example:** Priority-based scheduling ensures fairness

**6. Data Consistency:**
- ACID properties maintained
- Isolation prevents interference
- Serializable execution
- **Example:** Two users booking same seat - only one succeeds

**7. Deadlock Handling:**
- Detection and resolution mechanisms
- Prevention strategies
- System remains operational
- **Example:** Timeout-based deadlock detection

**8. Recovery Support:**
- Failed transactions can be rolled back
- Other transactions continue
- Minimal impact of failures
- **Example:** Power failure affects only active transactions

**Techniques:**
- Lock-based protocols (2PL)
- Timestamp-based protocols
- Validation-based protocols
- Multi-version concurrency control (MVCC)

---

### **c) Query Optimization:**

Query optimization is the process of selecting the most efficient execution plan for a query.

**Goals:**
- Minimize execution time
- Reduce resource consumption
- Improve response time
- Maximize throughput

**Steps in Query Optimization:**

**1. Parsing and Translation:**
- Check syntax
- Verify table and column names
- Translate to relational algebra
- Build query tree

**2. Optimization:**
- Generate multiple execution plans
- Estimate cost of each plan
- Select plan with minimum cost
- Consider indexes, statistics

**3. Code Generation:**
- Generate executable code
- Prepare execution plan
- Allocate resources

**4. Execution:**
- Execute optimized plan
- Return results

**Optimization Techniques:**

**1. Selection Operation Optimization:**
- Use indexes when available
- Push selections down in query tree
- Apply most restrictive selections first

```sql
-- Optimizer uses index on Dept first if more selective
SELECT * FROM EMP WHERE Salary > 50000 AND Dept = 'CS'
```

**2. Join Operation Optimization:**
- Choose appropriate join algorithm:
  - Nested-loop join
  - Hash join
  - Merge join
- Order joins to minimize intermediate results
- Use indexes on join columns

**3. Projection Operation:**
- Eliminate unnecessary columns early
- Reduce data transfer

```sql
-- Only select needed columns
SELECT Name, Salary FROM EMP WHERE Dept='CS'
-- Instead of SELECT *
```

**4. Use of Indexes:**
- B-tree indexes for range queries
- Hash indexes for equality
- Composite indexes for multiple columns

```sql
CREATE INDEX idx_dept_salary ON EMP(Dept, Salary);
```

**5. Expression Transformation:**
- Simplify complex expressions
- Eliminate redundant operations

```sql
-- Transform: NOT (A OR B) 
-- To: (NOT A) AND (NOT B)
```

**6. Subquery Optimization:**
- Convert correlated subqueries to joins
- Use EXISTS instead of IN when appropriate

```sql
-- Correlated (slow)
SELECT * FROM EMP E 
WHERE Salary > (SELECT AVG(Salary) FROM EMP WHERE Dept=E.Dept)

-- Join (faster)
SELECT E.* FROM EMP E 
JOIN (SELECT Dept, AVG(Salary) AS AvgSal FROM EMP GROUP BY Dept) D
ON E.Dept=D.Dept WHERE E.Salary > D.AvgSal
```

**Cost-Based Optimization:**
- Estimate cost of operations
- Consider:
  - I/O cost (disk access)
  - CPU cost (processing)
  - Memory cost
  - Network cost
- Use statistics:
  - Table size
  - Index availability
  - Data distribution
  - Cardinality

**Heuristic Optimization Rules:**
- Perform selections early (reduce tuples)
- Perform projections early (reduce attributes)
- Combine selections and cartesian products into joins
- Execute most restrictive selections first
- Use indexes when available

**Example:**
```sql
SELECT S.Name, C.CourseName
FROM STUDENT S, ENROLLMENT E, COURSE C
WHERE S.StudentID = E.StudentID 
  AND E.CourseID = C.CourseID
  AND S.Dept = 'CS'
  AND C.Credits > 3
```

**Optimized Plan:**
1. Apply selection σ(Dept='CS') on STUDENT (use index)
2. Apply selection σ(Credits>3) on COURSE
3. Join filtered STUDENT with ENROLLMENT
4. Join result with filtered COURSE
5. Project Name and CourseName

---

### **d) Fixed and Variable Size Records:**

#### **Fixed Size Records:**

**Definition:**
Records where each record occupies same amount of storage space.

**Characteristics:**
- All records have same length
- Simple structure
- Predictable storage requirement
- Fast access and retrieval

**Advantages:**

**1. Simple Implementation:**
- Easy to calculate record location
- Address = Base + (RecordNumber × RecordSize)

**2. Fast Random Access:**
- Direct access to any record
- No need to scan previous records
- O(1) access time

**3. Easy Deletion:**
- Mark record as deleted
- Space can be reused easily
- No fragmentation issues

**4. Efficient Searching:**
- Binary search possible if sorted
- Index structures simpler

**Disadvantages:**

**1. Space Wastage:**
- Unused space in many records
- If VARCHAR fields mostly short
- Storage inefficiency

**2. Limited Flexibility:**
- Maximum size must be predetermined
- Cannot accommodate larger values
- May need to redesign schema

**Example:**
```
EMPLOYEE (Fixed 100 bytes)
EmpID: 10 bytes
Name: 50 bytes
Dept: 20 bytes
Salary: 10 bytes
JoinDate: 10 bytes
---
Record 1: Bytes 0-99
Record 2: Bytes 100-199
Record 3: Bytes 200-299
```

---

#### **Variable Size Records:**

**Definition:**
Records where different records can have different lengths.

**Characteristics:**
- Records vary in size
- Complex storage management
- Efficient space utilization
- Slower access

**Causes of Variable Size:**
1. **Variable-length fields** (VARCHAR)
2. **Repeating fields** (arrays)
3. **Optional fields** (NULL values)
4. **Record types with variants**

**Representation Methods:**

**1. Byte-String Representation:**
- Special separators between fields
- Null value indicators
```
EmpID|Name|Dept|Salary$
E001|John Smith|CS|50000$
E002|Mary|IT|60000$
```

**2. Fixed-Part/Variable-Part Representation:**
- Fixed part contains fixed-length fields
- Pointers to variable-length fields
- Offset and length stored

**3. Slotted Page Structure:**
- Header contains record count
- Array of pointers to records
- Records stored from end of page
```
[Header|Slot 1|Slot 2|...|Free Space|...|Rec 2|Rec 1]
```

**Advantages:**

**1. Space Efficiency:**
- No wasted space
- Optimal storage utilization
- Cost-effective for varying data

**2. Flexibility:**
- Can handle any size data
- Easy to add new fields
- Adaptable structure

**3. Better for Text Data:**
- Names, addresses, descriptions
- Variable-length content
- No artificial limits

**Disadvantages:**

**1. Complex Implementation:**
- Need offset/length information
- Complex pointer structures
- More overhead

**2. Slower Access:**
- Cannot calculate position directly
- Must scan to find record
- Overhead in accessing fields

**3. Fragmentation:**
- Deleted records leave gaps
- Requires periodic reorganization
- Garbage collection needed

**4. Difficult Random Access:**
- Cannot use simple arithmetic
- Need index structures
- More complex algorithms

**Comparison:**

| Aspect | Fixed Size | Variable Size |
|--------|-----------|---------------|
| Storage | Wasteful | Efficient |
| Access Speed | Fast | Slower |
| Implementation | Simple | Complex |
| Flexibility | Limited | High |
| Fragmentation | Minimal | Significant |
| Use Case | Numeric data | Text data |

**Best Practices:**
- Use fixed size for frequently accessed data
- Use variable size for text-heavy applications
- Consider hybrid approach when appropriate
- Use indexes for variable-size records

---

## 11. Additional Topics - (15 marks total)

### **a) Transaction States in RDBMS - (5) CO4 BL2**

A transaction goes through multiple states during its execution lifecycle.

**Transaction States:**

**1. Active State:**
- **Definition:** Initial state; transaction is executing
- **Operations:** Read, Write operations in progress
- **Duration:** From BEGIN TRANSACTION until last operation
- **Example:** Transaction reading and updating account balances

**2. Partially Committed State:**
- **Definition:** After final operation executes, before commit
- **Characteristics:**
  - All operations completed
  - Changes still in buffer/memory
  - Not yet written to disk
  - Can still be aborted
- **Example:** All SQL statements executed, waiting for COMMIT

**3. Committed State:**
- **Definition:** Transaction successfully completed
- **Characteristics:**
  - All changes permanent
  - Written to disk (durable)
  - Cannot be rolled back
  - Transaction log updated
- **Example:** Money transfer completed successfully

**4. Failed State:**
- **Definition:** Transaction cannot proceed normally
- **Causes:**
  - System failure
  - Transaction error
  - Violation of integrity constraints
  - Deadlock victim
  - Timeout
- **Example:** Insufficient balance in account, constraint violation

**5. Aborted State:**
- **Definition:** Transaction rolled back; database restored to pre-transaction state
- **Characteristics:**
  - All changes undone
  - Resources released
  - Locks released
- **Options after abort:**
  - **Restart transaction** (if hardware/software failure)
  - **Kill transaction** (if logical error)
- **Example:** Transaction cancelled due to constraint violation

**State Transition Diagram:**
```
Active → Partially Committed → Committed
  ↓              ↓
Failed → Aborted
```

**Detailed Transitions:**
- Active → Partially Committed: Last statement executes
- Partially Committed → Committed: COMMIT issued, changes persisted
- Active → Failed: Error occurs during execution
- Partially Committed → Failed: Error during commit (disk failure)
- Failed → Aborted: Rollback completed

**Example Transaction Flow:**
```sql
BEGIN TRANSACTION;        -- Active state
UPDATE Account SET Balance = Balance - 100 WHERE AccNo = 'A123';
UPDATE Account SET Balance = Balance + 100 WHERE AccNo = 'B456';
-- After last UPDATE: Partially Committed state
COMMIT;                   -- Committed state
-- If error occurred: Failed → Aborted state
```

---

### **b) Relational Models vs Graph-Based Models - (5) CO2 BL2**

#### **Relational Database Model:**

**Structure:**
- Data organized in tables (relations)
- Rows represent records (tuples)
- Columns represent attributes
- Schema defines structure

**Characteristics:**
- **Fixed Schema:** Predefined structure
- **ACID Compliance:** Strong consistency
- **SQL Language:** Standardized query language
- **Normalization:** Reduce redundancy
- **Relationships:** Foreign keys

**Advantages:**

**1. Data Integrity:**
- Enforced through constraints
- ACID guarantees
- Referential integrity

**2. Mature Technology:**
- Well-established
- Extensive tooling
- Large community

**3. Query Power:**
- Complex joins
- Aggregations
- Powerful SQL

**4. Data Consistency:**
- Strong consistency model
- Transaction support

**Disadvantages:**

**1. Rigid Schema:**
- Schema changes expensive
- Not suitable for evolving data

**2. JOIN Performance:**
- Multiple joins slow for complex relationships
- Deep hierarchies inefficient

**3. Scalability:**
- Vertical scaling primarily
- Horizontal scaling complex

**4. Relationship Handling:**
- Many-to-many requires junction tables
- Deep relationships require multiple joins

**Best Use Cases:**
- Financial systems
- ERP systems
- Transactional systems
- Structured data with clear schema

**Examples:** MySQL, PostgreSQL, Oracle, SQL Server

---

#### **Graph-Based Database Model:**

**Structure:**
- **Nodes:** Represent entities
- **Edges/Relationships:** Connect nodes
- **Properties:** Key-value pairs on nodes and edges
- No rigid schema

**Characteristics:**
- **Flexible Schema:** Schema-less or schema-optional
- **Native Graph Storage:** Optimized for relationships
- **Graph Query Languages:** Cypher, Gremlin
- **Index-free adjacency:** Direct node connections

**Advantages:**

**1. Relationship Performance:**
- O(1) relationship traversal
- No JOIN overhead
- Efficient for connected data
- **Example:** Finding friends-of-friends instantly

**2. Flexible Schema:**
- Easy to add new node/edge types
- Evolving data models
- No migrations needed

**3. Natural Modeling:**
- Intuitive for connected data
- Visual representation
- Matches problem domain
- **Example:** Social networks, knowledge graphs

**4. Deep Traversals:**
- Efficient for recursive queries
- Pattern matching
- Variable-length paths
- **Example:** Shortest path, recommendation algorithms

**Disadvantages:**

**1. Limited Aggregation:**
- Not ideal for analytical queries
- Complex aggregations slower
- Better suited for traversals

**2. Less Mature:**
- Smaller ecosystem
- Fewer tools
- Less standardization

**3. ACID Limitations:**
- Some graph DBs sacrifice ACID for performance
- Eventual consistency models

**4. Learning Curve:**
- New query languages
- Different thinking paradigm
- Fewer developers skilled

**Best Use Cases:**
- Social networks
- Recommendation engines
- Fraud detection
- Knowledge graphs
- Network analysis

**Examples:** Neo4j, Amazon Neptune, ArangoDB

---

#### **Detailed Comparison:**

| Aspect | Relational Model | Graph Model |
|--------|-----------------|-------------|
| **Data Structure** | Tables | Nodes & Edges |
| **Schema** | Fixed, rigid | Flexible, evolving |
| **Relationships** | Foreign keys, JOINs | Native edges |
| **Query Language** | SQL | Cypher, Gremlin |
| **Performance (Joins)** | Degrades with depth | Constant time |
| **Scalability** | Vertical | Horizontal (varies) |
| **ACID** | Strong | Varies |
| **Best For** | Structured transactions | Connected data |

---

#### **Example Comparison:**

**Scenario:** Social Network - Find friends of friends

**Relational Approach:**
```sql
SELECT F2.FriendID
FROM Friends F1
JOIN Friends F2 ON F1.FriendID = F2.UserID
WHERE F1.UserID = 'User123'
AND F2.FriendID != 'User123';
```
- Multiple table scans
- JOIN overhead
- Slow for many levels

**Graph Approach (Cypher):**
```cypher
MATCH (user:Person {id: 'User123'})-[:FRIENDS_WITH*2]-(friend)
RETURN friend
```
- Direct traversal
- O(1) per relationship
- Fast for any depth

**Hybrid Approaches:**
- Use both models for different aspects
- Relational for transactions, Graph for relationships
- **Example:** Store user profiles in relational, friendships in graph

---

### **c) Functional Dependency - (5) CO3 BL4**

**Definition:**
A functional dependency (FD) is a constraint between two sets of attributes in a relation. X→Y means Y is functionally dependent on X.

**Formal Definition:**
X→Y (X determines Y) if for all tuples t1 and t2:
- If t1[X] = t2[X], then t1[Y] = t2[Y]
- For same X values, Y values must be identical

**Example:**
```
STUDENT(StudentID, Name, Email, Department)
StudentID → Name (Same StudentID always has same Name)
StudentID → Email
StudentID → Department
Email → StudentID (Assuming email is unique)
```

**Types of Functional Dependencies:**

**1. Trivial FD:**
- Y ⊆ X (Y is subset of X)
- Always true
- **Examples:**
  - {StudentID, Name} → StudentID
  - {A, B, C} → A
  - {X, Y} → X, Y

**2. Non-Trivial FD:**
- Y ⊄ X (Y not subset of X)
- Actually constrains the relation
- **Examples:**
  - StudentID → Name
  - {CourseID, StudentID} → Grade

**3. Completely Non-Trivial FD:**
- X ∩ Y = ∅ (no common attributes)
- **Example:**
  - StudentID → Name, Email

**4. Partial Dependency:**
- Non-prime attribute depends on part of candidate key
- Only in composite keys
- **Example:**
  - In {StudentID, CourseID} → CourseName
  - StudentID is not involved, so CourseName partially depends

**5. Transitive Dependency:**
- X→Y and Y→Z, then X→Z (but Y doesn't depend on Z)
- **Example:**
  - StudentID → DeptID
  - DeptID → DeptName
  - StudentID → DeptName (transitive)

---

**Armstrong's Axioms (Inference Rules):**

**Primary Rules:**

**1. Reflexivity:**
- If Y ⊆ X, then X→Y
- **Example:** {A, B} → A

**2. Augmentation:**
- If X→Y, then XZ→YZ
- **Example:** If StudentID→Name, then {StudentID, CourseID}→{Name, CourseID}

**3. Transitivity:**
- If X→Y and Y→Z, then X→Z
- **Example:** If StudentID→DeptID and DeptID→DeptName, then StudentID→DeptName

**Secondary Rules (Derived):**

**4. Union:**
- If X→Y and X→Z, then X→YZ
- **Example:** If A→B and A→C, then A→BC

**5. Decomposition:**
- If X→YZ, then X→Y and X→Z
- **Example:** If A→BC, then A→B and A→C

**6. Pseudotransitivity:**
- If X→Y and WY→Z, then WX→Z

---

**Closure of Attributes (X+):**

The set of all attributes functionally determined by X.

**Algorithm:**
```
1. Result = X
2. Repeat:
   For each FD Y→Z:
     If Y ⊆ Result:
       Result = Result ∪ Z
   Until Result doesn't change
3. Return Result
```

**Example:**
```
Given: A→B, B→C, C→D
Find: A+

Step 1: A+ = {A}
Step 2: A→B, so A+ = {A, B}
Step 3: B→C, so A+ = {A, B, C}
Step 4: C→D, so A+ = {A, B, C, D}
```

---

**Minimal Cover (Canonical Cover):**

A minimal set of FDs that is equivalent to original set.

**Properties:**
1. No extraneous attributes in FDs
2. No redundant FDs
3. Right side is single attribute

**Example:**
```
Original: {A→BC, B→C, A→B, AB→C}

Step 1: Decompose right side
{A→B, A→C, B→C, A→B, AB→C}

Step 2: Remove redundant
{A→B, B→C, AB→C}
(A→C removed as A→B→C)

Step 3: Simplify left side
{A→B, B→C}
(AB→C simplified to A→C, which is redundant)

Minimal Cover: {A→B, B→C}
```

---

**Applications:**

**1. Database Design:**
- Identify candidate keys
- Normalize relations
- Eliminate redundancy

**2. Query Optimization:**
- Determine unique constraints
- Optimize join operations

**3. Data Integrity:**
- Enforce constraints
- Validate updates

**4. Schema Refinement:**
- Decompose relations
- Improve normalization

---

## Summary

This exam paper covers core DBMS concepts across multiple modules:

**Module-wise distribution:**

- **CO1 (Introduction):** Database administrator roles, three-schema architecture
- **CO2 (Data Models):** Relational algebra/calculus, ER modeling, relational vs graph models
- **CO3 (Normalization):** Normal forms, functional dependencies, BCNF
- **CO4 (Transactions):** ACID properties, concurrency control, transaction states
- **CO5 (Storage & Query):** Aggregate functions, joins, locking protocols, records
- **CO6 (Advanced):** 4NF, query optimization

---

**Document prepared for: Database Management Systems Comprehensive Exam**

**Date: December 16, 2025**
