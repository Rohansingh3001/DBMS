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

### 21a. Armstrong's Axioms (5 marks)

**Armstrong's Axioms** are a set of inference rules used to derive all functional dependencies in a relational database. They form the foundation for reasoning about functional dependencies and are named after William W. Armstrong.

#### **Primary Axioms (Sound and Complete):**

---

#### **1. Reflexivity (Trivial FD)**

**Rule:** If Y ⊆ X, then X → Y

**Explanation:** A set of attributes always determines its subset.

**Examples:**
```
{Emp_ID, Name} → {Emp_ID}
{Emp_ID, Name} → {Name}
{Student_ID, Course_ID, Grade} → {Student_ID, Course_ID}
```

**Why it's called Trivial:** The dependent is already part of the determinant, so this dependency is always true and not particularly informative.

---

#### **2. Augmentation (Addition)**

**Rule:** If X → Y, then XZ → YZ for any attribute set Z

**Explanation:** Adding the same attributes to both sides of a functional dependency preserves the dependency.

**Examples:**
```
Given: Emp_ID → Salary
Then: {Emp_ID, Dept_ID} → {Salary, Dept_ID}

Given: Student_ID → Student_Name
Then: {Student_ID, Course_ID} → {Student_Name, Course_ID}
```

**Proof:**
- If X determines Y
- And we add Z to both sides
- Since X → Y, whenever two tuples have the same X, they have the same Y
- Adding Z maintains this property

---

#### **3. Transitivity**

**Rule:** If X → Y and Y → Z, then X → Z

**Explanation:** Functional dependencies are transitive, like mathematical implications.

**Examples:**
```
Given: Emp_ID → Dept_ID
And: Dept_ID → Dept_Name
Then: Emp_ID → Dept_Name

Given: Student_ID → Course_ID
And: Course_ID → Instructor
Then: Student_ID → Instructor
```

**Real-world Example:**
```
Employee Table:
Emp_ID → Dept_ID (Employee determines department)
Dept_ID → Manager (Department determines manager)
Therefore: Emp_ID → Manager (Employee determines manager)
```

---

#### **Secondary/Derived Rules:**

These can be proven using the primary axioms.

---

#### **4. Union**

**Rule:** If X → Y and X → Z, then X → YZ

**Example:**
```
Given: Emp_ID → Name
And: Emp_ID → Salary
Then: Emp_ID → {Name, Salary}
```

---

#### **5. Decomposition (Projectivity)**

**Rule:** If X → YZ, then X → Y and X → Z

**Example:**
```
Given: Emp_ID → {Name, Salary}
Then: Emp_ID → Name
And: Emp_ID → Salary
```

---

#### **6. Pseudo-transitivity**

**Rule:** If X → Y and WY → Z, then WX → Z

**Example:**
```
Given: Dept_ID → Budget
And: {Project_ID, Budget} → Allocation
Then: {Project_ID, Dept_ID} → Allocation
```

---

#### **Properties of Armstrong's Axioms:**

1. **Sound:** Only valid functional dependencies can be derived
2. **Complete:** All valid functional dependencies can be derived
3. **Minimal:** Cannot be reduced further while maintaining completeness

---

#### **Applications:**

1. **Finding Closure:** Compute X⁺ (all attributes determined by X)
2. **Finding Candidate Keys:** Determine minimal superkeys
3. **Normalization:** Identify dependencies causing anomalies
4. **Schema Design:** Validate relational schema correctness
5. **Dependency Preservation:** Check if decomposition preserves FDs

---

#### **Example: Finding Attribute Closure**

**Given:**
- Relation R(A, B, C, D, E)
- FDs: A → B, B → C, CD → E

**Find: A⁺ (closure of A)**

**Steps:**
1. Start with A⁺ = {A}
2. Using A → B: A⁺ = {A, B}
3. Using B → C: A⁺ = {A, B, C}
4. Cannot use CD → E (don't have D)
5. **Result: A⁺ = {A, B, C}**

**Conclusion:** A determines B and C but not D or E.

---

### 21b. E-R Diagram for AC Product Tracking System (10 marks)

#### **System Description:**

AC (shipping company) tracks shipped items through a company-wide information system. The system manages:
- **Shipped Items:** Characterized by item number, weight, dimensions, insurance, destination, delivery date
- **Retail Centers:** Characterized by type, uniqueID, address
- **Transportation Events:** Flights and truck deliveries with scheduleNumber, type, deliveryRoute

---

#### **Entity Identification:**

**1. SHIPPED_ITEM**
- **Attributes:**
  - Item_Number (PK) - Unique identifier
  - Weight
  - Dimensions (could be composite: Length, Width, Height)
  - Insurance_Amount
  - Destination
  - Final_Delivery_Date
  - Current_Location
- **Type:** Strong Entity

**2. RETAIL_CENTER**
- **Attributes:**
  - Unique_ID (PK)
  - Center_Type (e.g., warehouse, distribution center, store)
  - Address (composite: Street, City, State, ZIP)
- **Type:** Strong Entity

**3. TRANSPORTATION_EVENT**
- **Attributes:**
  - Schedule_Number (PK)
  - Event_Type (e.g., flight, truck)
  - Delivery_Route
  - Departure_Time
  - Arrival_Time
- **Type:** Strong Entity

---

#### **Relationship Identification:**

**1. RECEIVED_AT** (Shipped_Item ↔ Retail_Center)
- **Type:** Many-to-One (M:1)
- **Cardinality:** 
  - One item is received at one retail center
  - One retail center receives many items
- **Attributes:** Receipt_Date, Receipt_Time

**2. TRANSPORTED_BY** (Shipped_Item ↔ Transportation_Event)
- **Type:** Many-to-Many (M:N)
- **Cardinality:**
  - One item can be transported by multiple events (multi-leg journey)
  - One transportation event carries multiple items
- **Attributes:** Loading_Time, Sequence_Number (order in journey)

**3. DEPARTS_FROM / ARRIVES_AT** (Transportation_Event ↔ Retail_Center)
- **Type:** Many-to-One for each
- **Cardinality:**
  - One event departs from one center
  - One event arrives at one center
  - One center can have multiple departures/arrivals

---

#### **E-R Diagram:**

```
┌─────────────────────────┐
│    SHIPPED_ITEM         │
│─────────────────────────│
│ Item_Number (PK)        │
│ Weight                  │
│ Dimensions              │
│ Insurance_Amount        │
│ Destination             │
│ Final_Delivery_Date     │
└──────────┬──────────────┘
           │
           │ M (An item uses multiple transport events)
           │
     ╔═════╧══════════════════╗
     ║   TRANSPORTED_BY       ║  (M:N Relationship)
     ║────────────────────────║
     ║ Loading_Time           ║
     ║ Sequence_Number        ║
     ╚═════╤══════════════════╝
           │ N (A transport event carries many items)
           │
┌──────────┴─────────────────┐
│  TRANSPORTATION_EVENT      │
│────────────────────────────│
│ Schedule_Number (PK)       │
│ Event_Type                 │
│ Delivery_Route             │
│ Departure_Time             │
│ Arrival_Time               │
└──────┬──────────────┬──────┘
       │              │
       │ M            │ M
       │              │
  ╔════╧═══╗    ╔════╧═══════╗
  ║DEPARTS ║    ║ ARRIVES_AT ║
  ║  FROM  ║    ╚════╤═══════╝
  ╚════╤═══╝         │ 1
       │ 1           │
       │             │
┌──────┴─────────────┴────────┐
│     RETAIL_CENTER           │
│─────────────────────────────│
│ Unique_ID (PK)              │
│ Center_Type                 │
│ Address                     │
└──────────┬──────────────────┘
           │
           │ 1
           │
    ╔══════╧════════╗
    ║  RECEIVED_AT  ║  (M:1 Relationship)
    ║───────────────║
    ║ Receipt_Date  ║
    ║ Receipt_Time  ║
    ╚══════╤════════╝
           │ M
           │
┌──────────┴──────────────┐
│    SHIPPED_ITEM         │
└─────────────────────────┘
```

---

#### **Detailed Diagram with All Relationships:**

```
                   ┌─────────────────────┐
                   │   SHIPPED_ITEM      │
                   │─────────────────────│
                   │ Item_Number (PK)    │
                   │ Weight              │
                   │ Length              │
                   │ Width               │
                   │ Height              │
                   │ Insurance_Amount    │
                   │ Destination         │
                   │ Final_Delivery_Date │
                   └──────┬──────┬───────┘
                          │      │
                     (M)  │      │ (M)
                          │      │
        ╔═════════════════╧══╗   │
        ║  TRANSPORTED_BY    ║   │
        ║────────────────────║   │
        ║ Loading_Time       ║   │
        ║ Unloading_Time     ║   │
        ║ Sequence_Number    ║   │
        ╚═════════════════╤══╝   │
                     (N)  │      │
                          │      │
        ┌─────────────────┴──────┘
        │                 (M)
        │           ╔═════════════╗
        │           ║RECEIVED_AT  ║
        │           ║─────────────║
        │           ║Receipt_Date ║
        │           ╚═════╤═══════╝
        │                 │ (1)
        │                 │
┌───────┴──────────────┐  │
│TRANSPORTATION_EVENT  │  │
│──────────────────────│  │
│Schedule_Number (PK)  │  │
│Event_Type           │  │
│Delivery_Route        │  │
│Departure_Time        │  │
│Arrival_Time          │  │
└───┬──────────────┬───┘  │
    │              │      │
(M) │              │ (M)  │
    │              │      │
╔═══╧════╗    ╔════╧════╗ │
║DEPARTS ║    ║ARRIVES  ║ │
║  FROM  ║    ║   AT    ║ │
╚═══╤════╝    ╚════╤════╝ │
(1) │              │ (1)  │
    │              │      │
    └──────┬───────┘      │
           │              │
    ┌──────┴──────────────┴─┐
    │   RETAIL_CENTER       │
    │───────────────────────│
    │ Unique_ID (PK)        │
    │ Center_Type           │
    │ Street                │
    │ City                  │
    │ State                 │
    │ ZIP_Code              │
    └───────────────────────┘
```

---

#### **Cardinality Constraints:**

1. **SHIPPED_ITEM ─(M)─ TRANSPORTED_BY ─(N)─ TRANSPORTATION_EVENT**
   - One shipped item can use multiple transportation events (multi-leg journey)
   - One transportation event transports multiple shipped items

2. **SHIPPED_ITEM ─(M)─ RECEIVED_AT ─(1)─ RETAIL_CENTER**
   - Multiple items are received at one retail center
   - Each item is initially received at exactly one retail center

3. **TRANSPORTATION_EVENT ─(M)─ DEPARTS_FROM ─(1)─ RETAIL_CENTER**
   - Multiple events can depart from one retail center
   - Each event departs from exactly one retail center

4. **TRANSPORTATION_EVENT ─(M)─ ARRIVES_AT ─(1)─ RETAIL_CENTER**
   - Multiple events can arrive at one retail center
   - Each event arrives at exactly one retail center

---

#### **Relational Schema:**

```sql
SHIPPED_ITEM(
    Item_Number PRIMARY KEY,
    Weight DECIMAL(10,2),
    Length DECIMAL(10,2),
    Width DECIMAL(10,2),
    Height DECIMAL(10,2),
    Insurance_Amount DECIMAL(12,2),
    Destination VARCHAR(200),
    Final_Delivery_Date DATE,
    Received_Center_ID INT,
    FOREIGN KEY (Received_Center_ID) REFERENCES RETAIL_CENTER(Unique_ID)
)

RETAIL_CENTER(
    Unique_ID PRIMARY KEY,
    Center_Type VARCHAR(50),
    Street VARCHAR(100),
    City VARCHAR(50),
    State VARCHAR(50),
    ZIP_Code VARCHAR(10)
)

TRANSPORTATION_EVENT(
    Schedule_Number PRIMARY KEY,
    Event_Type VARCHAR(20),
    Delivery_Route VARCHAR(100),
    Departure_Time DATETIME,
    Arrival_Time DATETIME,
    Departure_Center_ID INT,
    Arrival_Center_ID INT,
    FOREIGN KEY (Departure_Center_ID) REFERENCES RETAIL_CENTER(Unique_ID),
    FOREIGN KEY (Arrival_Center_ID) REFERENCES RETAIL_CENTER(Unique_ID)
)

TRANSPORTED_BY(
    Item_Number INT,
    Schedule_Number INT,
    Loading_Time DATETIME,
    Unloading_Time DATETIME,
    Sequence_Number INT,
    PRIMARY KEY (Item_Number, Schedule_Number),
    FOREIGN KEY (Item_Number) REFERENCES SHIPPED_ITEM(Item_Number),
    FOREIGN KEY (Schedule_Number) REFERENCES TRANSPORTATION_EVENT(Schedule_Number)
)
```

---

### 22. Short Notes (15 marks)

---

#### a) Concurrent Execution (5 marks)

**Concurrent Execution** refers to the interleaved execution of multiple transactions simultaneously in a database system.

---

#### **Definition:**

When multiple transactions execute at the same time (or appear to execute simultaneously), the system must coordinate their operations to maintain database consistency.

---

#### **Why Concurrent Execution?**

1. **Improved Throughput:** More transactions completed per unit time
2. **Better Resource Utilization:** CPU works while I/O operations happen
3. **Reduced Waiting Time:** Short transactions don't wait for long ones
4. **Better Response Time:** System appears more responsive to users

---

#### **Execution Models:**

**1. Serial Execution:**
```
T1: ─────────────────────►
T2:                        ─────────────────────►
T3:                                              ─────────────────────►

Slow but always correct
```

**2. Concurrent Execution:**
```
T1: ─────────    ─────────    ─────────►
T2:       ─────────    ─────────    ─────────►
T3:             ─────────    ─────────►

Fast but needs concurrency control
```

---

#### **Advantages:**

1. **Increased Processor and Disk Utilization:**
   - While one transaction waits for disk I/O, another can use CPU
   
2. **Reduced Average Response Time:**
   - Short transactions complete quickly without waiting

3. **Better System Throughput:**
   - More transactions processed per second

---

#### **Problems with Concurrent Execution:**

**1. Lost Update Problem:**
```
T1: Read(X=100)
T2:              Read(X=100)
T1: X = X + 50 (X=150)
T2:                       X = X + 30 (X=130)
T1: Write(X=150)
T2:              Write(X=130)  ← T1's update lost!
```

**2. Dirty Read Problem (Temporary Update):**
```
T1: Read(X=100)
T1: X = X + 50 (X=150)
T1: Write(X=150)
T2:              Read(X=150)  ← Reading uncommitted data
T1: ROLLBACK (X=100)
T2: Using wrong value (150 instead of 100)
```

**3. Unrepeatable Read:**
```
T1: Read(X=100)
T2:              Read(X=100)
T2:              X = X + 50 (X=150)
T2:              Write(X=150)
T2:              COMMIT
T1: Read(X=150)  ← Different value in same transaction!
```

**4. Phantom Read:**
```
T1: SELECT COUNT(*) FROM Employee WHERE Salary > 50000
    → Result: 10 rows
T2:              INSERT INTO Employee VALUES (...)  -- Salary = 60000
T2:              COMMIT
T1: SELECT COUNT(*) FROM Employee WHERE Salary > 50000
    → Result: 11 rows (Phantom row appeared!)
```

---

#### **Concurrency Control Mechanisms:**

1. **Locking Protocols:**
   - Shared locks (read)
   - Exclusive locks (write)
   - Two-phase locking (2PL)

2. **Timestamp-Based Protocols:**
   - Each transaction gets a timestamp
   - Operations ordered by timestamp

3. **Optimistic Concurrency Control:**
   - Execute without locks
   - Validate before commit

4. **Multiversion Concurrency Control (MVCC):**
   - Multiple versions of data
   - Readers don't block writers

---

#### **Schedule in Concurrent Execution:**

**Schedule:** Chronological order of operations from concurrent transactions

**Example:**
```
Schedule S1:
T1: Read(A)
T2:         Read(A)
T1: A = A - 50
T1: Write(A)
T2:         A = A + 100
T2:         Write(A)
```

**Serial Schedule:** T1 → T2 or T2 → T1 (always correct)
**Serializable Schedule:** Equivalent to some serial schedule (correct)
**Non-serializable Schedule:** Not equivalent to any serial schedule (may be incorrect)

---

#### b) Two-Phase Locking Protocol (2PL) (5 marks)

**Two-Phase Locking (2PL)** is a concurrency control protocol that ensures serializability by dividing transaction execution into two distinct phases: Growing Phase and Shrinking Phase.

---

#### **Definition:**

A transaction must acquire all locks before releasing any lock. The protocol guarantees that the schedule is conflict-serializable.

---

#### **Two Phases:**

**Phase 1: Growing Phase (Expanding Phase)**
- Transaction can **acquire locks** (shared or exclusive)
- Transaction **cannot release** any lock
- Phase ends when transaction acquires its last lock
- Also called **lock acquisition phase**

**Phase 2: Shrinking Phase (Contracting Phase)**
- Transaction can **release locks**
- Transaction **cannot acquire** new locks
- Phase begins when first lock is released
- Also called **lock release phase**

---

#### **Graphical Representation:**

```
Number
of      │        Growing Phase    │    Shrinking Phase
Locks   │                         │
Held    │          ╱──────────╲   │
        │         ╱            ╲  │
        │        ╱              ╲ │
        │       ╱                ╲│
        │      ╱                  ╲
        │_____╱____________________╲_______________
                    │               │
                    │               │
              Lock Point      Release Begins
            (Maximum locks)
```

---

#### **Lock Point:**

The point where a transaction has acquired all its locks and is about to enter the shrinking phase. Transactions can be serialized in the order of their lock points.

---

#### **Example:**

**Transaction T1:**
```
1. Lock-S(A)      ← Growing Phase starts
2. Read(A)
3. Lock-X(B)      ← Still Growing Phase
4. Read(B)
5. B = B + A
6. Write(B)
7. Unlock(A)      ← Shrinking Phase starts (Lock Point reached)
8. Unlock(B)      ← Still Shrinking Phase
```

**Transaction T2:**
```
1. Lock-S(C)      ← Growing Phase
2. Read(C)
3. Lock-X(D)      ← Still Growing
4. Read(D)
5. D = D * C
6. Write(D)
7. Unlock(C)      ← Shrinking Phase (Lock Point)
8. Unlock(D)
```

---

#### **Types of Two-Phase Locking:**

---

**1. Basic 2PL:**
- Follows two-phase rule strictly
- **Problem:** Can lead to cascading rollbacks
- **Advantage:** Simple to implement

---

**2. Conservative (Static) 2PL:**
- Transaction acquires **all locks** at the beginning
- If any lock unavailable, waits for all
- No growing phase during execution
- **Advantage:** Deadlock-free
- **Disadvantage:** Difficult to predict all required locks

**Example:**
```
T1: Lock-All(A, B, C)  ← Acquire everything first
    Read(A)
    Read(B)
    Write(C)
    Unlock-All(A, B, C)
```

---

**3. Strict 2PL:**
- Transaction holds **all exclusive locks** until commit/abort
- Shared locks can be released during shrinking phase
- **Advantage:** Prevents cascading rollbacks
- **Most commonly used** in practice

**Example:**
```
T1: Lock-X(A)
    Write(A)
    Lock-X(B)
    Write(B)
    -- All X locks held until commit --
    COMMIT
    Unlock(A)
    Unlock(B)
```

---

**4. Rigorous 2PL:**
- Holds **all locks (both shared and exclusive)** until commit/abort
- Strictest form of 2PL
- **Advantage:** Maximum isolation, prevents all anomalies
- **Disadvantage:** Reduced concurrency

---

#### **Advantages of 2PL:**

1. **Guarantees Serializability:** Schedule is always conflict-serializable
2. **Widely Used:** Implemented in most commercial DBMS
3. **Simple to Implement:** Clear rules for lock acquisition/release
4. **Prevents Inconsistencies:** Protects from lost updates and dirty reads

---

#### **Disadvantages of 2PL:**

1. **Deadlocks Possible:**
```
T1: Lock-X(A)
T2:          Lock-X(B)
T1: Lock-X(B)  ← Waits for T2
T2:          Lock-X(A)  ← Waits for T1  [DEADLOCK!]
```

2. **Cascading Rollbacks (in Basic 2PL):**
```
T1: Lock-X(A), Write(A), Unlock(A)
T2:                      Lock-X(A), Read(A)
T1: ROLLBACK  ← T2 must also rollback (read dirty data)
```

3. **Reduced Concurrency:**
   - Locks held longer than necessary
   - Transactions wait for locks

4. **Starvation Possible:**
   - Long transactions may hold locks indefinitely
   - Short transactions wait forever

---

#### **Deadlock Handling in 2PL:**

**Prevention:**
- Lock all resources at once (Conservative 2PL)
- Order resources (always lock in same order)

**Detection:**
- Wait-for graph
- Timeout mechanisms

**Recovery:**
- Abort one transaction (victim selection)
- Rollback and restart

---

#### c) Serializability (5 marks)

**Serializability** is a property of a transaction schedule that ensures concurrent execution produces the same result as some serial execution of the same transactions.

---

#### **Definition:**

A schedule is **serializable** if it is equivalent to a serial schedule (where transactions execute one after another with no interleaving).

---

#### **Why Serializability?**

- **Goal:** Allow concurrent execution while maintaining correctness
- **Guarantee:** Database consistency despite interleaved operations
- **Standard:** Gold standard for concurrency control correctness

---

#### **Serial Schedule:**

Transactions execute completely one after another (no overlap).

**Example:**
```
Serial Schedule 1 (T1 → T2):
T1: Read(A), A=A-50, Write(A), Read(B), B=B+50, Write(B)
T2:                                                       Read(A), A=A*2, Write(A)

Serial Schedule 2 (T2 → T1):
T2: Read(A), A=A*2, Write(A)
T1:                          Read(A), A=A-50, Write(A), Read(B), B=B+50, Write(B)
```

**Properties:**
- Always correct (maintains consistency)
- Low concurrency (poor performance)
- Used as reference for correctness

---

#### **Types of Serializability:**

---

### **1. Conflict Serializability**

**Definition:** A schedule is conflict-serializable if it can be transformed into a serial schedule by swapping non-conflicting operations.

#### **Conflicting Operations:**

Two operations conflict if:
1. They belong to **different transactions**
2. They access the **same data item**
3. At least one is a **Write** operation

**Conflict Types:**
```
Read-Write (R-W) Conflict:  T1: Read(X)   T2: Write(X)
Write-Read (W-R) Conflict:  T1: Write(X)  T2: Read(X)
Write-Write (W-W) Conflict: T1: Write(X)  T2: Write(X)
```

**Non-Conflicting:**
```
Read-Read: T1: Read(X)  T2: Read(X)  ← Can be swapped
```

---

#### **Testing Conflict Serializability:**

**Method: Precedence Graph (Serialization Graph)**

**Steps:**
1. Create a node for each transaction
2. Draw edge Ti → Tj if Ti conflicts with Tj and Ti's operation comes first
3. If graph is **acyclic** → Conflict Serializable
4. If graph has **cycle** → Not Conflict Serializable

---

#### **Example 1: Conflict Serializable Schedule**

```
Schedule S:
T1: Read(A)
T2:         Read(A)
T1: Write(A)
T2:         Write(A)
T1: Read(B)
T2:         Read(B)
T1: Write(B)
T2:         Write(B)

Precedence Graph:
T1 → T2  (T1's Write(A) before T2's Write(A))
T1 → T2  (T1's Write(B) before T2's Write(B))

Graph: T1 ──→ T2  (No cycle)

Result: CONFLICT SERIALIZABLE
Equivalent to serial schedule: T1 → T2
```

---

#### **Example 2: Non-Conflict Serializable Schedule**

```
Schedule S:
T1: Read(A)
T2:         Read(A)
T2:         Write(A)
T1: Write(A)

Precedence Graph:
T1 → T2  (T1's Read(A) before T2's Write(A))
T2 → T1  (T2's Write(A) before T1's Write(A))

Graph: T1 ←→ T2  (CYCLE!)

Result: NOT CONFLICT SERIALIZABLE
```

---

### **2. View Serializability**

**Definition:** A schedule is view-serializable if it is view-equivalent to a serial schedule.

#### **View Equivalence Conditions:**

Two schedules S1 and S2 are view-equivalent if:

1. **Initial Read:** If Ti reads initial value of X in S1, it must also read initial value in S2

2. **Updated Read:** If Ti reads value of X written by Tj in S1, same must happen in S2

3. **Final Write:** If Ti performs final write on X in S1, it must perform final write in S2

---

#### **Relationship:**

```
Conflict Serializable ⊂ View Serializable ⊂ All Schedules

All conflict serializable schedules are view serializable
But NOT all view serializable schedules are conflict serializable
```

---

#### **Example: View Serializable but NOT Conflict Serializable**

```
Schedule S:
T1: Write(A)
T2:          Write(A)
T3:                   Write(A)

Analysis:
- No reads, only writes
- Final write by T3 determines result
- View equivalent to serial schedule: T1→T2→T3
- BUT has W-W conflicts that create cycles in precedence graph

Result: VIEW SERIALIZABLE but NOT CONFLICT SERIALIZABLE
```

---

#### **Comparison Table:**

| **Aspect** | **Conflict Serializability** | **View Serializability** |
|------------|------------------------------|--------------------------|
| **Strictness** | More restrictive | Less restrictive |
| **Testing** | Polynomial time (precedence graph) | NP-complete |
| **Usage** | Most commonly used | Theoretical importance |
| **Implementation** | Practical (2PL, timestamp) | Difficult to implement |

---

#### **Testing Methods:**

**1. For Conflict Serializability:**
```
Algorithm:
1. Build precedence graph
2. Check for cycles using DFS/BFS
3. If acyclic → Conflict serializable
4. Topological sort gives equivalent serial order
```

**2. For View Serializability:**
```
- Check all possible serial schedules
- Verify view equivalence
- NP-complete problem (exponential time)
```

---

#### **Practical Implications:**

**In Practice:**
- Most DBMS ensure **conflict serializability**
- Protocols like 2PL guarantee conflict serializability
- View serializability is theoretically important but rarely used

**Example Protocols:**
- **Two-Phase Locking (2PL):** Ensures conflict serializability
- **Timestamp Ordering:** Ensures conflict serializability
- **Optimistic CC:** Validates serializability at commit

---

#### **Non-Serializable Schedules:**

**Problems:**
1. **Inconsistent Results:** Different from any serial execution
2. **Lost Updates:** Writes overwritten incorrectly
3. **Dirty Reads:** Reading uncommitted data
4. **Unrepeatable Reads:** Same query gives different results

**Example:**
```
Initial: A=100, B=100
T1: Read(A), A=A+50, Write(A)  -- A=150
T2:                             Read(A), Read(B), Print(A+B)
T1:                                                           Read(B), B=B+50, Write(B)

T2 prints: 150+100=250 (inconsistent state!)
Serial T1→T2: 150+150=300
Serial T2→T1: 100+100=200
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
