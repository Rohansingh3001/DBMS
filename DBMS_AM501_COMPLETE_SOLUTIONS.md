# Database Management Systems (AM501) - Complete Solutions

## Group A: Multiple Choice Questions (Answer any 10) - 10×1=10

### 1. A characteristic of an entity
**Answer: b) attribute**

An attribute is a characteristic or property of an entity.

---

### 2. In a relational data model, the columns of a table are called
**Answer: c) attribute**

In a relational model, columns represent attributes while rows represent tuples.

---

### 3. Advantage of locking protocol
**Answer: b) Consistency**

Locking protocol ensures consistency by preventing conflicts in concurrent transactions.

---

### 4. The word 'loss' in lossless refers to
**Answer: b) loss of Information**

"Lossless" decomposition means no information is lost during the decomposition process.

---

### 5. 2NF is always in
**Answer: a) 1NF**

Any relation in 2NF (Second Normal Form) must already be in 1NF (First Normal Form). This is the hierarchical nature of normal forms.

---

### 6. Which of the following operation is used if we are interested in only certain columns of a table?
**Answer: a) PROJECTION**

Projection (π) operation selects specific columns from a table and removes duplicates.

---

### 7. Cartesian Product in relational algebra is
**Answer: b) binary operation**

Cartesian Product operates on two relations, making it a binary operation.

---

### 8. Which of the following is true
**Answer: c) Every relation in BCNF is also in 3NF**

BCNF is stricter than 3NF, so any relation satisfying BCNF automatically satisfies 3NF requirements.

---

### 9. Which of the following is Database Language?
**Answer: d) All of these**

- **DDL** (Data Definition Language) - CREATE, ALTER, DROP
- **DML** (Data Manipulation Language) - INSERT, UPDATE, DELETE
- **Query Language** - SELECT statements

All are database languages.

---

### 10. Time stamp is used for
**Answer: a) Serialization**

Timestamps are used in timestamp-based protocols to ensure serializable execution of transactions.

---

### 11. A top-to-bottom relationship among the items in a database is established by
**Answer: a) Hierarchical Schema**

Hierarchical schema organizes data in a tree-like structure with parent-child relationships.

---

### 12. The concurrency control has the problem of
**Answer: d) all of these**

Concurrency control must handle:
- **Lost updates** - One transaction overwrites another's changes
- **Dirty read** - Reading uncommitted data
- **Unrepeatable read** - Different values on repeated reads

---

## Group B: Short Answer Type Questions (Answer any 3) - 3×5=15

### 13a. Relational Algebra Query - Customers with balance over 1000 (3 marks)

**Query:** Find customers who have a balance of over 1000 from the Borrower relation

**Relational Algebra Expression:**

```
σ(balance > 1000)(Borrower)
```

**Explanation:**
- σ (sigma) is the **selection operator**
- It filters rows based on the condition `balance > 1000`
- Returns only those tuples where the balance exceeds 1000

**Alternative with projection (if only specific columns needed):**
```
π(customer_name, balance)(σ(balance > 1000)(Borrower))
```

---

### 13b. Difference between Strong Entity Set and Weak Entity Set (2 marks)

| **Aspect** | **Strong Entity Set** | **Weak Entity Set** |
|------------|----------------------|---------------------|
| **Primary Key** | Has its own primary key | Does not have sufficient attributes for primary key |
| **Existence** | Exists independently | Depends on owner/parent entity |
| **Representation** | Single rectangle | Double rectangle |
| **Key** | Has complete key | Has partial key (discriminator) |
| **Relationship** | Independent | Connected via identifying relationship (double diamond) |

**Examples:**
- **Strong Entity:** Student (Student_ID is primary key), Employee (Emp_ID is primary key)
- **Weak Entity:** Dependent (depends on Employee), Loan_Payment (depends on Loan)

**Key Point:** A weak entity's primary key = Owner's primary key + Partial key

---

### 14. Queries on SALESPERSON, TRIP, EXPENSE Relations (5 marks)

**Given Relations:**
- SALESPERSON(SSN, Name, Start_Year, Dept_No)
- TRIP(SSN, From_City, To_City, Departure_Date, Return_Date, Trip_ID)
- EXPENSE(Trip_ID, Account#, Amount)

#### a) Details of trips that exceeded Rs. 3,000 in expenses

**Relational Algebra:**
```
π(SSN, From_City, To_City, Departure_Date, Return_Date, Trip_ID)
  (TRIP ⋈(Trip_ID) (σ(Amount > 3000)(EXPENSE)))
```

**Step-by-step:**
1. Select expenses > 3000: `σ(Amount > 3000)(EXPENSE)`
2. Join with TRIP on Trip_ID: `TRIP ⋈(Trip_ID) ...`
3. Project all TRIP attributes: `π(TRIP.*)`

**SQL Query:**
```sql
SELECT DISTINCT T.SSN, T.From_City, T.To_City, 
       T.Departure_Date, T.Return_Date, T.Trip_ID
FROM TRIP T
JOIN EXPENSE E ON T.Trip_ID = E.Trip_ID
WHERE E.Amount > 3000;
```

#### b) Print SSN of salesman who took trips to 'New York'

**Relational Algebra:**
```
π(SSN)(σ(To_City = 'New York')(TRIP))
```

**SQL Query:**
```sql
SELECT DISTINCT SSN
FROM TRIP
WHERE To_City = 'New York';
```

---

### 15. ACID Properties in Transaction (5 marks)

**ACID** stands for **Atomicity, Consistency, Isolation, Durability** - the four key properties that guarantee reliable transaction processing.

#### **A - Atomicity (All or Nothing)**

**Definition:** A transaction is treated as a single, indivisible unit of work.

**Rules:**
- Either ALL operations complete successfully, or
- NONE of the operations take effect

**Example:**
```
Bank Transfer: Debit Rs. 1000 from Account A, Credit Rs. 1000 to Account B
- If debit succeeds but credit fails → ROLLBACK entire transaction
- Money cannot disappear or be duplicated
```

**Mechanism:** Maintained using transaction log and rollback mechanisms.

---

#### **C - Consistency (Correctness)**

**Definition:** Transaction brings database from one consistent state to another consistent state.

**Rules:**
- All integrity constraints must be satisfied
- Database rules and constraints are preserved

**Example:**
```
Constraint: Balance ≥ 0
- Before transaction: A=5000, B=3000 ✓
- After valid transaction: A=4000, B=4000 ✓
- Invalid transaction (A=−1000, B=6000) ✗ REJECTED
```

---

#### **I - Isolation (Independence)**

**Definition:** Concurrent transactions execute independently without interference.

**Rules:**
- Intermediate results are hidden from other transactions
- Appears as if transactions execute serially

**Example:**
```
T1: Transfer Rs. 1000 (A→B)
T2: Calculate total balance

Isolation ensures T2 doesn't see partial state where:
- Money deducted from A but not yet added to B
```

**Levels:** Read Uncommitted, Read Committed, Repeatable Read, Serializable

---

#### **D - Durability (Permanence)**

**Definition:** Once a transaction commits, changes are permanent and survive system failures.

**Rules:**
- Committed data persists even after:
  - Power failure
  - System crash
  - Hardware failure

**Example:**
```
After ATM withdrawal is confirmed (committed):
- Cash dispensed
- Balance updated
- Even if ATM crashes immediately after, balance remains updated
```

**Mechanism:** Write-ahead logging, database backups, and recovery procedures.

---

#### **Why ACID is Important:**

1. **Data Integrity:** Ensures database remains accurate and valid
2. **Reliability:** Users can trust the system
3. **Recovery:** System can recover from failures
4. **Concurrency:** Multiple users can work simultaneously safely
5. **Business Critical:** Essential for banking, e-commerce, healthcare systems
6. **Legal Compliance:** Required for auditing and regulatory requirements

---

### 16. Company-Department E-R Diagram (5 marks)

#### i) Identify all Entities and Relationships

**Entities:**

1. **EMPLOYEE**
   - Attributes: Emp_ID (PK), Name, Salary, Address, Phone, Email
   
2. **DEPARTMENT**
   - Attributes: Dept_ID (PK), Dept_Name, Location, Budget
   
3. **SUPERVISOR** (Specialization of Employee)
   - Attributes: All employee attributes + Supervision_Start_Date

**Relationships:**

1. **WORKS_IN** (Employee ↔ Department)
   - Type: Many-to-Many (M:N)
   - An employee can work in multiple departments
   - A department has multiple employees
   - Constraint: Each employee must work in at least one department (min 1)

2. **MANAGES/SUPERVISES** (Supervisor ↔ Department)
   - Type: One-to-One (1:1)
   - Each department has exactly one supervisor
   - A supervisor manages exactly one department

3. **ISA** (Employee ↔ Supervisor)
   - Type: Specialization/Inheritance
   - Supervisor is a specialized type of Employee

---

#### iii) E-R Diagram

```
                    ┌─────────────────────┐
                    │     EMPLOYEE        │
                    │─────────────────────│
                    │ Emp_ID (PK)         │
                    │ Name                │
                    │ Salary              │
                    │ Address             │
                    │ Phone               │
                    └──────────┬──────────┘
                               │
                               │ M
                         ┌─────────────┐
                         │  WORKS_IN   │  (M:N Relationship)
                         └─────────────┘
                               │ N
                               │
                    ┌──────────┴──────────┐
                    │    DEPARTMENT       │
                    │─────────────────────│
                    │ Dept_ID (PK)        │
                    │ Dept_Name           │
                    │ Location            │
                    │ Budget              │
                    └──────────┬──────────┘
                               │
                               │ 1
                         ┌─────────────┐
                         │  MANAGES    │  (1:1 Relationship)
                         └─────────────┘
                               │ 1
                               │
                    ┌──────────┴──────────┐
                    │    SUPERVISOR       │
                    │─────────────────────│
                    │ (inherits from      │
                    │  EMPLOYEE)          │
                    │ Supervision_Start   │
                    └─────────────────────┘
                            △
                            │
                        (ISA/d)
                            │
                    ┌───────┴───────┐
                    │   EMPLOYEE    │
                    └───────────────┘
```

**Detailed ASCII Diagram:**

```
┌─────────────────┐
│   EMPLOYEE      │◄──────────────┐
│─────────────────│               │
│ Emp_ID (PK)     │               │ ISA
│ Name            │               │ (Disjoint)
│ Salary          │               │
│ Address         │               │
└────────┬────────┘               │
         │                  ┌─────┴──────┐
         │ (1,N)            │ SUPERVISOR │
         │                  │────────────│
    ╔════╧════╗            │ Sup_ID     │
    ║WORKS_IN ║            │ Exp_Years  │
    ╚════╤════╝            └─────┬──────┘
         │ (1,N)                 │ 1
         │                       │
┌────────┴────────┐        ╔════╧═════╗
│  DEPARTMENT     │◄───────║ MANAGES  ║
│─────────────────│     1  ╚══════════╝
│ Dept_ID (PK)    │
│ Dept_Name       │
│ Location        │
└─────────────────┘
```

**Cardinality Constraints:**

- **WORKS_IN:** (1,N) : (1,N)
  - Min 1 employee per department (at least one)
  - Min 1 department per employee (at least one)
  
- **MANAGES:** 1:1
  - Each department has exactly one supervisor
  - Each supervisor manages exactly one department

---

### 17a. Cardinality of Relationship (3 marks)

**Cardinality** specifies the number of entity instances that can be associated with another entity through a relationship.

#### **Types of Cardinality:**

---

#### **1. One-to-One (1:1)**

**Definition:** One entity instance is associated with at most one instance of another entity.

**Example:**
```
PERSON ──────1:1────── PASSPORT
```
- One person has one passport
- One passport belongs to one person

**Real-world:** 
- Country ↔ Capital City
- Employee ↔ Desk (in dedicated desk scenario)

---

#### **2. One-to-Many (1:N)**

**Definition:** One entity instance is associated with multiple instances of another entity.

**Example:**
```
DEPARTMENT ──────1:N────── EMPLOYEE
```
- One department has many employees
- Each employee belongs to one department

**Real-world:**
- Customer ↔ Orders
- Teacher ↔ Students (in one class)
- Mother ↔ Children

---

#### **3. Many-to-One (N:1)**

**Definition:** Multiple entity instances are associated with one instance of another entity (reverse of 1:N).

**Example:**
```
EMPLOYEE ──────N:1────── DEPARTMENT
```
- Many employees belong to one department
- Same as 1:N viewed from opposite direction

---

#### **4. Many-to-Many (M:N)**

**Definition:** Multiple instances of one entity are associated with multiple instances of another entity.

**Example:**
```
STUDENT ──────M:N────── COURSE
```
- One student enrolls in many courses
- One course has many students

**Real-world:**
- Author ↔ Book
- Actor ↔ Movie
- Doctor ↔ Patient

**Implementation:** Requires a junction/bridge table

---

#### **Participation Constraints:**

**Total Participation (Thick line/Double line):**
- Every entity must participate
- Example: Every employee MUST be assigned to a department

**Partial Participation (Thin line):**
- Entity may or may not participate
- Example: Employee MAY have dependents

**Notation:**
```
(min, max) notation:
- (0,1) = Optional, at most one
- (1,1) = Mandatory, exactly one
- (0,N) = Optional, many possible
- (1,N) = Mandatory, at least one
```

---

### 17b. Single-valued and Multi-valued Attributes (2 marks)

#### **Single-valued Attribute**

**Definition:** An attribute that can hold only ONE value for each entity instance.

**Characteristics:**
- Atomic (indivisible)
- One value per entity
- Most common type

**ER Diagram Notation:** 
- Simple oval: `○ Attribute_Name`

**Examples:**
```
Employee:
  ○ Emp_ID        (One ID per employee)
  ○ Date_of_Birth (One birth date)
  ○ Gender        (One gender)
  ○ Salary        (One current salary)
  
Student:
  ○ Roll_Number   (One roll number)
  ○ Age           (One age value)
```

---

#### **Multi-valued Attribute**

**Definition:** An attribute that can hold MULTIPLE values for each entity instance.

**Characteristics:**
- Can have set of values
- Variable number of values
- Requires special handling in relational model

**ER Diagram Notation:** 
- Double oval: `◎ Attribute_Name`

**Examples:**
```
Employee:
  ◎ Phone_Numbers (Mobile, Home, Office)
  ◎ Email_Addresses (Personal, Work)
  ◎ Skills (Java, Python, SQL)
  
Student:
  ◎ Hobbies (Reading, Sports, Music)
  
Person:
  ◎ Languages_Known (English, Hindi, Bengali)
```

---

#### **Implementation in Relational Model:**

**Single-valued:** Direct column in table
```sql
Employee(Emp_ID, Name, Salary, Gender)
```

**Multi-valued:** Separate table required
```sql
Employee(Emp_ID, Name, Salary)
Phone(Emp_ID, Phone_Number)  -- Separate table
```

**Example:**
```
Employee Table:
Emp_ID | Name
-------|-------
E001   | John
E002   | Mary

Phone Table:
Emp_ID | Phone_Number
-------|-------------
E001   | 9876543210
E001   | 9123456789
E002   | 9988776655
```

---

## Group C: Long Answer Type Questions (Answer any 3) - 3×15=45

### 18a. Relational Algebra Expressions (10 marks)

**Given Relations:**
- **Supplier**(Supno, sname, supaddress)
- **Item**(Itemno, Iname, stock)
- **Supp-Item**(Supno, Itemno, rate)

---

#### i) List all suppliers from 'Varanasi' city who supply PISTON

**Relational Algebra Expression:**

```
π(sname, Supno, supaddress)(
  σ(supaddress = 'Varanasi')(Supplier) 
  ⋈(Supno) 
  Supp-Item 
  ⋈(Itemno) 
  σ(Iname = 'PISTON')(Item)
)
```

**Step-by-step Breakdown:**

1. **Filter suppliers from Varanasi:**
   ```
   σ(supaddress = 'Varanasi')(Supplier)
   ```

2. **Filter items that are PISTON:**
   ```
   σ(Iname = 'PISTON')(Item)
   ```

3. **Join all three relations:**
   ```
   Varanasi_Suppliers ⋈ Supp-Item ⋈ Piston_Items
   ```

4. **Project supplier details:**
   ```
   π(sname, Supno, supaddress)(...)
   ```

**Alternative Expression:**
```
π(sname)(
  σ(supaddress='Varanasi' AND Iname='PISTON')
  (Supplier ⋈ Supp-Item ⋈ Item)
)
```

---

#### ii) Display all suppliers who supply PISTON RINGS

**Relational Algebra Expression:**

```
π(Supno, sname, supaddress)(
  Supplier 
  ⋈(Supno) 
  Supp-Item 
  ⋈(Itemno) 
  σ(Iname = 'PISTON RINGS')(Item)
)
```

**Step-by-step:**

1. **Select PISTON RINGS items:**
   ```
   σ(Iname = 'PISTON RINGS')(Item)
   ```

2. **Join with Supp-Item to find suppliers:**
   ```
   Supp-Item ⋈(Itemno) Piston_Rings_Items
   ```

3. **Join with Supplier to get details:**
   ```
   Supplier ⋈(Supno) Result_from_step2
   ```

4. **Project all supplier attributes:**
   ```
   π(Supno, sname, supaddress)(...)
   ```

---

#### iii) Change supplier names to upper case

**Relational Algebra Expression:**

```
π(Supno, UPPER(sname), supaddress)(Supplier)
```

**Note:** This uses **Extended Relational Algebra** with built-in function.

**Alternative Approach:**
```
ρ(Supno, UPPER_NAME, supaddress)(
  π(Supno, UPPER(sname), supaddress)(Supplier)
)
```

**In SQL (for reference):**
```sql
SELECT Supno, UPPER(sname) AS sname, supaddress
FROM Supplier;
```

**Explanation:**
- Pure relational algebra doesn't have string functions
- Extended relational algebra allows UPPER(), LOWER(), etc.
- π operator projects columns with transformation
- Result has uppercase supplier names

---

#### iv) List all suppliers supplying DOOR LOCK from 'Jaipur' city

**Relational Algebra Expression:**

```
π(Supno, sname, supaddress)(
  σ(supaddress = 'Jaipur')(Supplier) 
  ⋈(Supno) 
  Supp-Item 
  ⋈(Itemno) 
  σ(Iname = 'DOOR LOCK')(Item)
)
```

**Complete Expression with intermediate results:**

```
STEP 1: Jaipur_Suppliers ← σ(supaddress = 'Jaipur')(Supplier)

STEP 2: Door_Lock_Items ← σ(Iname = 'DOOR LOCK')(Item)

STEP 3: Temp1 ← Jaipur_Suppliers ⋈(Supno) Supp-Item

STEP 4: Temp2 ← Temp1 ⋈(Itemno) Door_Lock_Items

STEP 5: Result ← π(Supno, sname, supaddress)(Temp2)
```

**Simplified Single Expression:**
```
π(sname)(
  σ(supaddress='Jaipur' AND Iname='DOOR LOCK')
  (Supplier ⋈ Supp-Item ⋈ Item)
)
```

---

### 18b. SELECT, PROJECT, UNION Operators with Examples (5 marks)

#### **1. SELECT Operator (σ) - Unary Operator**

**Definition:** Selects rows (tuples) from a relation that satisfy a given condition/predicate.

**Syntax:**
```
σ(condition)(Relation)
```

**Properties:**
- Horizontal operation (works on rows)
- Result has same schema as input
- Reduces number of tuples
- Commutative: σ(c1)(σ(c2)(R)) = σ(c2)(σ(c1)(R))

**Example 1:**
```
Employee Table:
EmpID | Name    | Salary | Dept
------|---------|--------|------
E001  | John    | 50000  | IT
E002  | Mary    | 75000  | HR
E003  | David   | 45000  | IT
E004  | Sarah   | 80000  | Finance

Query: Find employees with salary > 50000

σ(Salary > 50000)(Employee)

Result:
EmpID | Name  | Salary | Dept
------|-------|--------|--------
E002  | Mary  | 75000  | HR
E004  | Sarah | 80000  | Finance
```

**Example 2:**
```
Query: Find IT department employees

σ(Dept = 'IT')(Employee)

Result:
EmpID | Name  | Salary | Dept
------|-------|--------|-----
E001  | John  | 50000  | IT
E003  | David | 45000  | IT
```

**Complex Conditions:**
```
σ(Salary > 60000 AND Dept = 'IT')(Employee)
σ(Dept = 'IT' OR Dept = 'HR')(Employee)
```

---

#### **2. PROJECT Operator (π) - Unary Operator**

**Definition:** Selects specific columns (attributes) from a relation and eliminates duplicates.

**Syntax:**
```
π(attribute_list)(Relation)
```

**Properties:**
- Vertical operation (works on columns)
- Result has fewer columns
- Automatically removes duplicate rows
- Result size ≤ input size

**Example 1:**
```
Employee Table:
EmpID | Name  | Salary | Dept
------|-------|--------|--------
E001  | John  | 50000  | IT
E002  | Mary  | 75000  | HR
E003  | David | 45000  | IT

Query: Show only names and salaries

π(Name, Salary)(Employee)

Result:
Name  | Salary
------|-------
John  | 50000
Mary  | 75000
David | 45000
```

**Example 2:**
```
Query: Show all departments (removes duplicates)

π(Dept)(Employee)

Result:
Dept
--------
IT
HR
Finance
```

**Combined with Selection:**
```
π(Name)(σ(Salary > 60000)(Employee))

First: Select employees with salary > 60000
Then: Project only their names
```

---

#### **3. UNION Operator (∪) - Binary Operator**

**Definition:** Combines tuples from two relations, eliminating duplicates.

**Syntax:**
```
R1 ∪ R2
```

**Union Compatibility Requirements:**
1. Both relations must have same number of attributes
2. Corresponding attributes must have compatible domains
3. Attribute names can differ (result uses first relation's names)

**Properties:**
- Commutative: R1 ∪ R2 = R2 ∪ R1
- Associative: (R1 ∪ R2) ∪ R3 = R1 ∪ (R2 ∪ R3)
- Eliminates duplicates automatically

**Example 1:**
```
CS_Students:
RollNo | Name
-------|-------
101    | Alice
102    | Bob
103    | Carol

IT_Students:
RollNo | Name
-------|-------
103    | Carol
104    | David
105    | Eve

Query: All students from either CS or IT

CS_Students ∪ IT_Students

Result:
RollNo | Name
-------|-------
101    | Alice
102    | Bob
103    | Carol    (duplicate removed)
104    | David
105    | Eve
```

**Example 2:**
```
Morning_Classes:
StudentID | Course
----------|--------
S001      | DBMS
S002      | OS

Evening_Classes:
StudentID | Course
----------|--------
S003      | DBMS
S001      | Networks

Query: All student-course combinations

Morning_Classes ∪ Evening_Classes

Result:
StudentID | Course
----------|--------
S001      | DBMS
S002      | OS
S003      | DBMS
S001      | Networks
```

**Practical Application:**
```
Query: Find all customers who have either a savings account OR a loan

π(CustomerName)(SavingsAccount) ∪ π(CustomerName)(Loan)
```

---

#### **Comparison of Operators:**

| Operator | Type | Operation | Result Size |
|----------|------|-----------|-------------|
| SELECT (σ) | Unary | Horizontal (rows) | ≤ Input |
| PROJECT (π) | Unary | Vertical (columns) | ≤ Input |
| UNION (∪) | Binary | Combine relations | ≤ Sum of inputs |

---

### 19a. Relational Algebra Queries on Banking Schema (10 marks)

**Given Schema:**
- **Customer**(custname, street, city)
- **Branch**(branchname, branchcity)
- **Account**(accno, branchname, balance)
- **Loan**(loanno, branchname, amount)
- **Borrower**(custname, loanno)
- **Depositor**(custname, accno)

---

#### i) Find the loan no, branch and amount of loans where amount is greater than 1000$

**Relational Algebra Expression:**

```
π(loanno, branchname, amount)(σ(amount > 1000)(Loan))
```

**Step-by-step Execution:**

1. **Selection:** Filter loans with amount > 1000
   ```
   σ(amount > 1000)(Loan)
   ```

2. **Projection:** Select required attributes
   ```
   π(loanno, branchname, amount)(Result_from_Step1)
   ```

**Example:**
```
Loan Table:
loanno | branchname  | amount
-------|-------------|--------
L001   | Downtown    | 5000
L002   | Uptown      | 800
L003   | Midtown     | 12000
L004   | Downtown    | 1500

Result:
loanno | branchname  | amount
-------|-------------|--------
L001   | Downtown    | 5000
L003   | Midtown     | 12000
L004   | Downtown    | 1500
```

**SQL Equivalent:**
```sql
SELECT loanno, branchname, amount
FROM Loan
WHERE amount > 1000;
```

---

#### ii) Find the names of all customers who have a loan AND an account at the bank

**Relational Algebra Expression:**

```
π(custname)(Borrower) ∩ π(custname)(Depositor)
```

**Alternative Method using Natural Join:**
```
π(custname)(Borrower ⋈(custname) Depositor)
```

**Step-by-step Explanation:**

**Method 1: Intersection**
1. Get all customers with loans: `π(custname)(Borrower)`
2. Get all customers with accounts: `π(custname)(Depositor)`
3. Find common customers: `Result1 ∩ Result2`

**Method 2: Join**
```
π(custname)(
  ρ(B_custname, loanno)(Borrower) 
  ⋈(B_custname = D_custname) 
  ρ(D_custname, accno)(Depositor)
)
```

**Example:**
```
Borrower Table:
custname  | loanno
----------|--------
Alice     | L001
Bob       | L002
Carol     | L003
David     | L004

Depositor Table:
custname  | accno
----------|-------
Alice     | A101
Carol     | A102
Eve       | A103

Result (Customers with BOTH loan and account):
custname
---------
Alice
Carol
```

**SQL Equivalent:**
```sql
SELECT DISTINCT custname
FROM Borrower
INTERSECT
SELECT DISTINCT custname
FROM Depositor;

-- OR --

SELECT DISTINCT B.custname
FROM Borrower B
JOIN Depositor D ON B.custname = D.custname;
```

---

#### iii) Find the loan no. for each loan of an amount greater than 10000

**Relational Algebra Expression:**

```
π(loanno)(σ(amount > 10000)(Loan))
```

**Step-by-step:**

1. **Filter loans:** Select loans with amount > 10000
   ```
   σ(amount > 10000)(Loan)
   ```

2. **Project loan numbers:** Get only loanno column
   ```
   π(loanno)(Result_from_Step1)
   ```

**Example:**
```
Loan Table:
loanno | branchname  | amount
-------|-------------|--------
L001   | Downtown    | 5000
L002   | Uptown      | 15000
L003   | Midtown     | 12000
L004   | Downtown    | 8000
L005   | Uptown      | 25000

Result:
loanno
-------
L002
L003
L005
```

**SQL Equivalent:**
```sql
SELECT loanno
FROM Loan
WHERE amount > 10000;
```

---

#### iv) Names of all customers starting with 'A'

**Relational Algebra Expression:**

```
π(custname)(σ(custname LIKE 'A%')(Customer))
```

**Step-by-step:**

1. **Selection:** Filter customers whose name starts with 'A'
   ```
   σ(custname LIKE 'A%')(Customer)
   ```

2. **Projection:** Get only customer names
   ```
   π(custname)(Result_from_Step1)
   ```

**Example:**
```
Customer Table:
custname  | street        | city
----------|---------------|----------
Alice     | Main St       | Boston
Bob       | Oak Ave       | Chicago
Andrew    | Pine Rd       | Boston
Carol     | Elm St        | Seattle
Arthur    | Maple Dr      | Boston

Result:
custname
---------
Alice
Andrew
Arthur
```

**SQL Equivalent:**
```sql
SELECT custname
FROM Customer
WHERE custname LIKE 'A%';
```

---

### 19b. Functional Dependencies and Normalization (5 marks)

#### **Functional Dependency (FD)**

**Definition:** A relationship between two sets of attributes in a relation where the value of one set uniquely determines the value of another set.

**Notation:** X → Y (X determines Y)
- X is the **determinant**
- Y is the **dependent**

**Meaning:** If two tuples have the same value for X, they must have the same value for Y.

---

#### **Types of Functional Dependencies:**

1. **Trivial FD:**
   - Y is a subset of X
   - Example: {Emp_ID, Name} → {Emp_ID}
   - Always true, not useful

2. **Non-Trivial FD:**
   - Y is not a subset of X
   - Example: Emp_ID → Name
   - Meaningful dependencies

3. **Completely Non-Trivial FD:**
   - No attribute of Y appears in X
   - Example: Emp_ID → {Name, Salary}

4. **Partial Dependency:**
   - Non-prime attribute depends on part of candidate key
   - Example: {Student_ID, Course_ID} → Student_Name
   - Violates 2NF

5. **Transitive Dependency:**
   - X → Y and Y → Z, then X → Z
   - Example: Emp_ID → Dept_ID → Dept_Name
   - Violates 3NF

---

#### **Armstrong's Axioms:**

**Primary Rules:**
1. **Reflexivity:** If Y ⊆ X, then X → Y
2. **Augmentation:** If X → Y, then XZ → YZ
3. **Transitivity:** If X → Y and Y → Z, then X → Z

**Secondary Rules:**
4. **Union:** If X → Y and X → Z, then X → YZ
5. **Decomposition:** If X → YZ, then X → Y and X → Z
6. **Pseudo-transitivity:** If X → Y and WY → Z, then WX → Z

---

#### **Normal Forms:**

**Purpose:** Eliminate redundancy and anomalies

---

### **1st Normal Form (1NF)**

**Rules:**
- All attributes must be atomic (indivisible)
- No repeating groups
- Each cell contains single value
- Primary key must be defined

**Example of Violation:**
```
Student Table (NOT in 1NF):
Student_ID | Name  | Courses
-----------|-------|------------------
S001       | Alice | DBMS, OS, Networks
S002       | Bob   | DBMS, AI
```

**After 1NF:**
```
Student_Course Table:
Student_ID | Name  | Course
-----------|-------|----------
S001       | Alice | DBMS
S001       | Alice | OS
S001       | Alice | Networks
S002       | Bob   | DBMS
S002       | Bob   | AI
```

---

### **2nd Normal Form (2NF)**

**Rules:**
- Must be in 1NF
- No partial dependencies
- All non-prime attributes must be fully functionally dependent on entire candidate key

**Example of Violation:**
```
Student_Course Table (in 1NF, NOT in 2NF):
Student_ID | Course_ID | Student_Name | Course_Name | Instructor
-----------|-----------|--------------|-------------|------------
S001       | C101      | Alice        | DBMS        | Prof. Smith
S001       | C102      | Alice        | OS          | Prof. Jones

FDs:
{Student_ID, Course_ID} → {Student_Name, Course_Name, Instructor}
Student_ID → Student_Name (Partial dependency - violates 2NF)
Course_ID → {Course_Name, Instructor} (Partial dependency)
```

**After 2NF (Decompose):**
```
Student Table:
Student_ID | Student_Name
-----------|-------------
S001       | Alice
S002       | Bob

Course Table:
Course_ID | Course_Name | Instructor
----------|-------------|-------------
C101      | DBMS        | Prof. Smith
C102      | OS          | Prof. Jones

Enrollment Table:
Student_ID | Course_ID
-----------|----------
S001       | C101
S001       | C102
```

---

### **3rd Normal Form (3NF)**

**Rules:**
- Must be in 2NF
- No transitive dependencies
- Non-prime attributes must depend directly on primary key

**Example of Violation:**
```
Employee Table (in 2NF, NOT in 3NF):
Emp_ID | Name  | Dept_ID | Dept_Name
-------|-------|---------|------------
E001   | Alice | D01     | IT
E002   | Bob   | D02     | HR
E003   | Carol | D01     | IT

FDs:
Emp_ID → {Name, Dept_ID, Dept_Name}
Dept_ID → Dept_Name (Transitive dependency - violates 3NF)
```

**After 3NF:**
```
Employee Table:
Emp_ID | Name  | Dept_ID
-------|-------|--------
E001   | Alice | D01
E002   | Bob   | D02
E003   | Carol | D01

Department Table:
Dept_ID | Dept_Name
--------|------------
D01     | IT
D02     | HR
```

---

### **Boyce-Codd Normal Form (BCNF)**

**Rules:**
- Must be in 3NF
- For every FD X → Y, X must be a superkey
- Stricter than 3NF
- Eliminates all anomalies based on FDs

**Example of Violation:**
```
Enrollment Table (in 3NF, NOT in BCNF):
Student_ID | Course | Instructor
-----------|--------|-------------
S001       | DBMS   | Prof. Smith
S002       | DBMS   | Prof. Smith
S001       | OS     | Prof. Jones

Assumptions:
- Each course has only one instructor
- An instructor can teach only one course
- A student can enroll in multiple courses

FDs:
{Student_ID, Course} → Instructor (Candidate key)
Instructor → Course (Violates BCNF because Instructor is not a superkey)
```

**After BCNF:**
```
Student_Course Table:
Student_ID | Instructor
-----------|-------------
S001       | Prof. Smith
S002       | Prof. Smith
S001       | Prof. Jones

Instructor_Course Table:
Instructor  | Course
------------|-------
Prof. Smith | DBMS
Prof. Jones | OS
```

---

### **Summary of Anomalies:**

**Insert Anomaly:** Cannot insert data without other required data
**Update Anomaly:** Must update multiple rows for single fact change
**Delete Anomaly:** Deleting data causes loss of other information

**Normalization eliminates these anomalies by proper decomposition.**

---

### 20. Database Schema Design and Implementation

#### **Given Requirements:**

Design a database for a **Library Management System** with the following entities:
- **Books** (Book_ID, Title, Author, ISBN, Publisher, Year, Category, Available_Copies)
- **Members** (Member_ID, Name, Address, Phone, Email, Membership_Date)
- **Transactions** (Trans_ID, Member_ID, Book_ID, Issue_Date, Due_Date, Return_Date, Fine)

---

#### **Tasks:**

1. **Identify entities, attributes, and relationships**
2. **Draw E-R Diagram**
3. **Convert to relational schema**
4. **Normalize to 3NF**
5. **Write SQL queries for common operations**

---

#### **Solution:**

**Entities and Attributes:**

1. **BOOK**
   - Book_ID (PK)
   - Title
   - Author
   - ISBN (Unique)
   - Publisher
   - Year
   - Category
   - Total_Copies
   - Available_Copies

2. **MEMBER**
   - Member_ID (PK)
   - Name
   - Address
   - Phone
   - Email (Unique)
   - Membership_Date
   - Status (Active/Inactive)

3. **TRANSACTION**
   - Trans_ID (PK)
   - Member_ID (FK)
   - Book_ID (FK)
   - Issue_Date
   - Due_Date
   - Return_Date
   - Fine_Amount

**Relationships:**

1. **BORROWS** (Member ↔ Book)
   - Type: M:N (Many-to-Many)
   - Implemented through TRANSACTION entity
   - A member can borrow many books
   - A book can be borrowed by many members (at different times)

---

#### **E-R Diagram:**

```
┌─────────────────┐
│     MEMBER      │
│─────────────────│
│ Member_ID (PK)  │
│ Name            │
│ Address         │
│ Phone           │
│ Email           │
│ Member_Date     │
│ Status          │
└────────┬────────┘
         │ 1
         │
    ╔════╧══════════╗
    ║  TRANSACTION  ║  (Bridge Entity for M:N)
    ║───────────────║
    ║ Trans_ID (PK) ║
    ║ Member_ID (FK)║
    ║ Book_ID (FK)  ║
    ║ Issue_Date    ║
    ║ Due_Date      ║
    ║ Return_Date   ║
    ║ Fine_Amount   ║
    ╚════╤══════════╝
         │ N
         │
┌────────┴────────┐
│      BOOK       │
│─────────────────│
│ Book_ID (PK)    │
│ Title           │
│ Author          │
│ ISBN            │
│ Publisher       │
│ Year            │
│ Category        │
│ Total_Copies    │
│ Available       │
└─────────────────┘
```

---

#### **Relational Schema (3NF):**

```sql
BOOK(
    Book_ID INT PRIMARY KEY,
    Title VARCHAR(200) NOT NULL,
    Author VARCHAR(100) NOT NULL,
    ISBN VARCHAR(20) UNIQUE,
    Publisher VARCHAR(100),
    Publication_Year INT,
    Category VARCHAR(50),
    Total_Copies INT DEFAULT 1,
    Available_Copies INT DEFAULT 1
)

MEMBER(
    Member_ID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Address VARCHAR(200),
    Phone VARCHAR(15),
    Email VARCHAR(100) UNIQUE,
    Membership_Date DATE NOT NULL,
    Status VARCHAR(10) DEFAULT 'Active'
)

TRANSACTION(
    Trans_ID INT PRIMARY KEY,
    Member_ID INT NOT NULL,
    Book_ID INT NOT NULL,
    Issue_Date DATE NOT NULL,
    Due_Date DATE NOT NULL,
    Return_Date DATE,
    Fine_Amount DECIMAL(10,2) DEFAULT 0,
    FOREIGN KEY (Member_ID) REFERENCES MEMBER(Member_ID),
    FOREIGN KEY (Book_ID) REFERENCES BOOK(Book_ID)
)
```

---

#### **Sample SQL Queries:**

**1. Issue a book:**
```sql
INSERT INTO TRANSACTION (Trans_ID, Member_ID, Book_ID, Issue_Date, Due_Date)
VALUES (1001, 101, 5001, '2025-12-16', '2025-12-30');

UPDATE BOOK
SET Available_Copies = Available_Copies - 1
WHERE Book_ID = 5001;
```

**2. Return a book:**
```sql
UPDATE TRANSACTION
SET Return_Date = '2025-12-28'
WHERE Trans_ID = 1001;

UPDATE BOOK
SET Available_Copies = Available_Copies + 1
WHERE Book_ID = 5001;
```

**3. Calculate fine (Rs. 10 per day after due date):**
```sql
UPDATE TRANSACTION
SET Fine_Amount = GREATEST(0, DATEDIFF(Return_Date, Due_Date) * 10)
WHERE Trans_ID = 1001 AND Return_Date IS NOT NULL;
```

**4. Find all overdue books:**
```sql
SELECT M.Name, B.Title, T.Due_Date, 
       DATEDIFF(CURDATE(), T.Due_Date) AS Days_Overdue
FROM TRANSACTION T
JOIN MEMBER M ON T.Member_ID = M.Member_ID
JOIN BOOK B ON T.Book_ID = B.Book_ID
WHERE T.Return_Date IS NULL AND T.Due_Date < CURDATE();
```

**5. Most borrowed books:**
```sql
SELECT B.Title, B.Author, COUNT(*) AS Borrow_Count
FROM TRANSACTION T
JOIN BOOK B ON T.Book_ID = B.Book_ID
GROUP BY B.Book_ID, B.Title, B.Author
ORDER BY Borrow_Count DESC
LIMIT 10;
```

**6. Members with outstanding fines:**
```sql
SELECT M.Member_ID, M.Name, SUM(T.Fine_Amount) AS Total_Fine
FROM MEMBER M
JOIN TRANSACTION T ON M.Member_ID = T.Member_ID
WHERE T.Fine_Amount > 0
GROUP BY M.Member_ID, M.Name
HAVING Total_Fine > 0
ORDER BY Total_Fine DESC;
```

---

## Additional Important Topics

### Indexing

**Definition:** Data structure that improves the speed of data retrieval operations.

**Types:**
1. **Primary Index:** On primary key (sorted)
2. **Secondary Index:** On non-key attributes
3. **Clustered Index:** Data physically sorted
4. **Non-clustered Index:** Logical ordering

**Example:**
```sql
CREATE INDEX idx_author ON BOOK(Author);
CREATE UNIQUE INDEX idx_isbn ON BOOK(ISBN);
```

---

### Transactions and Concurrency Control

**Transaction States:**
- Active
- Partially Committed
- Committed
- Failed
- Aborted

**Concurrency Problems:**
1. **Lost Update:** Two transactions update same data
2. **Dirty Read:** Reading uncommitted data
3. **Unrepeatable Read:** Different values on repeated reads
4. **Phantom Read:** New rows appear in repeated queries

**Locking Protocols:**
- **Shared Lock (S):** Read-only
- **Exclusive Lock (X):** Read-write
- **Two-Phase Locking (2PL):** Growing and shrinking phases

---

### Database Recovery

**Techniques:**
1. **Log-based Recovery:** Write-ahead logging
2. **Checkpoint:** Periodic database snapshots
3. **Shadow Paging:** Maintain two page tables

**REDO:** Reapply committed transactions
**UNDO:** Rollback uncommitted transactions

---

## Conclusion

This document covers comprehensive solutions for Database Management Systems examination questions including:
- Relational Algebra operations
- E-R Modeling
- Normalization (1NF, 2NF, 3NF, BCNF)
- ACID properties
- SQL queries
- Transaction management
- Concurrency control

All concepts are explained with detailed examples and practical implementations.

---

**Document prepared for: AM501 - Database Management Systems**

**Date: December 16, 2025**
