# 📚 Library Management System

The Library Management System (LMS) is a database designed to efficiently manage library operations such as maintaining books, authors, employees, members, vendors, and donations.

The goal of this project is to create a structured relational database that supports library operations like book borrowing, vendor purchases, and donor tracking, ensuring accuracy, scalability, and integrity of data.

This project implements a **Library Management System** using SQL, designed to manage books, members, employees, vendors, and their relationships efficiently.  
The database follows principles of **Entity–Relationship modeling**, **Normalization**, and **Referential Integrity**.


## 🧩 1. Entities and Attributes

| Entity | Attributes |
|--------|-------------|
| **Library** | Lib_No (PK), Lib_Name, Lib_Location |
| **Books** | Book_ID (PK), Title, Language, Publish_Year |
| **Author** | Author_ID (PK), F_Name, Birth_Date, Sex |
| **Employee** | Emp_ID (PK), Name, Email, BOD, Sec, Lib_No (FK), Manager_ID (Self-FK) |
| **Member** | M_ID (PK), F_Name, L_Name, Email, Lib_No (FK) |
| **Loan** | Loan_ID (PK), Loan_Date, Return_Date, Member_ID (FK) |
| **Emp_Dependent** *(Weak Entity)* | Name, Sex, Emp_ID (FK) |
| **Donations** | Donator_ID (PK), Donator_Name, Donator_Email, Lib_No (FK) |
| **Vendor** | Vendor_ID (PK), Name, Email |
| **Emp_Mobile** *(Multivalued)* | Emp_ID (FK), Mob_No |
| **Vendor_Mobile** *(Multivalued)* | Vendor_ID (FK), Mobile_No |
| **Book_Author** *(M:N)* | Book_ID (FK), Author_ID (FK) |
| **Buys** *(M:N)* | Lib_No (FK), Vendor_ID (FK) |
| **Offer** *(M:N)* | Emp_ID (FK), Loan_ID (FK) |

---

## 🔗 2. Relationships

| Relationship | Type | Cardinality | Participation | Description |
|---------------|------|-------------|---------------|--------------|
| Library – Books | Strong | M:N | Library **total**, Book partial | A library can store many books; a book can belong to multiple libraries. |
| Library – Vendor | Strong | M:N | Both partial | A library can order from multiple vendors. |
| Employee – Library | Strong | N:1 | Employee **total**, Library partial | Each employee works in one library. |
| Employee – Emp_Dependent | Weak | 1:N | Employee **total**, Dependent **total** | A dependent cannot exist without an employee. |
| Member – Loan | Strong | N:1 | Loan **total**, Member partial | Each loan is issued to one member. |
| Book – Author | Strong | M:N | Book **total**, Author partial | A book can have multiple authors; an author can write multiple books. |
| Library – Donations | Strong | 1:N | Donation **total**, Library partial | Each donation is linked to one library. |
| Library – Member | Strong | 1:N | Member **total**, Library partial | Each member belongs to one library. |

---

## DDL 
	 DDL stands for Data Definition Language, a subset of SQL used to create, modify, and remove database objects like tables, schemas, and indexes. Its main purpose is to define the structure and schema of a database, rather than manipulating the data within it.


## DML 
	A data manipulation language is a computer programming language used for adding, deleting, and modifying data in a database. A DML is often a sublanguage of a broader database language such as SQL, with the DML comprising some of the operators in the language.


For Code: [DDL and DML (Sql Code)](code.md)



## Queries 
	A query is a request for information, or an action to be performed on data. It can be a simple question a person asks to get information, or a technical command sent to a database or search engine to retrieve specific data.

For Code: [10 SQL Queries](Queries.md) 

---



## 📘 3. Normalization

To ensure **data consistency** and **minimal redundancy**, the database is normalized up to **Third Normal Form (3NF)**.

### **1NF – First Normal Form**
- All attributes contain **atomic values**.
- Multivalued attributes (e.g., employee phone numbers, vendor contact numbers) are moved to separate tables:
  - `emp_mobile(emp_id, mob_no)`
  - `vendor_mobile(vendor_id, mobile_no)`
- Composite and repeating groups are eliminated.

### **2NF – Second Normal Form**
- All **non-key attributes** are fully dependent on the **primary key**.
- Example: In `books`, `title`, `language`, and `publish_year` depend entirely on `book_id`.
- Junction tables (`book_author`, `buys`, `offer`) remove partial dependencies in M:N relationships.

### **3NF – Third Normal Form**
- No **transitive dependencies**.
- Example: Employee’s library information is stored in `library`, not duplicated in `employee`.
- Vendor contact info is in `vendor_mobile`, preventing dependency chains.
- Each table focuses on one **logical concept** (Library, Employee, Vendor, etc.).

### ✅ **Normalization Summary**

| Normal Form | Achieved | Purpose |
|--------------|-----------|----------|
| 1NF | ✅ | Remove repeating groups and multivalued attributes |
| 2NF | ✅ | Remove partial dependencies |
| 3NF | ✅ | Remove transitive dependencies and ensure referential integrity |

The design ensures **data accuracy**, **integrity**, and **efficient updates** across all relationships.

---

## 🧠 4. Participation Summary

| Relationship | Entity with **Total Participation** |
|---------------|--------------------------------------|
| Library – Books | Library |
| Library – Vendor | None (both partial) |
| Employee – Library | Employee |
| Employee – Emp_Dependent | Employee, Dependent |
| Member – Loan | Loan |
| Book – Author | Book |
| Library – Donations | Donation |
| Library – Member | Member |

---

## 📊 5. Sample Data Overview

### **Libraries**
| Lib_No | Lib_Name | Lib_Location |
|--------|-----------|--------------|
| 1 | Kathmandu Valley Public Library | Kathmandu |
| 2 | Westside Library | Espoo |

### **Books**
| Book_ID | Title | Language | Publish_Year |
|----------|--------|----------|---------------|
| 101 | Data Structures in C++ | English | 2018 |
| 102 | Introduction to Databases | English | 2020 |
| 103 | Artificial Intelligence Basics | English | 2021 |
| 104 | Introduction to Algorithms | English | 2009 |

### **Authors**
| Author_ID | F_Name | Birth_Date | Sex |
|------------|--------|------------|-----|
| 201 | John Smith | 1975-06-15 | M |
| 202 | Emma Johnson | 1980-11-22 | F |
| 203 | Robert Brown | 1968-05-30 | M |
| 204 | Alice Green | 1992-02-14 | F |

### **Employees**
| Emp_ID | Name | Email | BOD | Sec | Lib_No | Manager_ID |
|--------|------|-------|-----|-----|--------|-------------|
| 301 | Alice Brown | alice.brown@library.fi | 1985-03-10 | HR | 1 | NULL |
| 302 | Bob Martin | bob.martin@library.fi | 1990-07-25 | Finance | 1 | 301 |
| 303 | Carol White | carol.white@library.fi | 1988-01-18 | IT | 2 | 301 |
| 304 | David Lee | david.lee@library.fi | 1992-09-05 | Assistant | 2 | 303 |

### **Members**
| M_ID | F_Name | L_Name | Email | Lib_No |
|------|---------|--------|-------|--------|
| 401 | David | Green | david.green@mail.com | 1 |
| 402 | Sophia | Black | sophia.black@mail.com | 1 |
| 403 | James | Wilson | james.wilson@mail.com | 2 |
| 404 | Emily | Davis | emily.davis@mail.com | 1 |
| 405 | Michael | Scott | michael.scott@mail.com | 2 |

### **Loans**
| Loan_ID | Loan_Date | Return_Date | Member_ID |
|----------|------------|-------------|------------|
| 501 | 2025-09-01 | 2025-09-15 | 401 |
| 502 | 2025-09-05 | NULL | 402 |
| 503 | 2025-09-10 | NULL | 403 |

### **Donations**
| Donator_ID | Donator_Name | Donator_Email | Lib_No |
|-------------|---------------|----------------|--------|
| 601 | Anna Clark | anna.clark@mail.com | 1 |
| 602 | Mike Lee | mike.lee@mail.com | 2 |
| 603 | Katherine Mills | katherine.mills@mail.com | 1 |

### **Relationships & Multivalued Data**
- Employees may have multiple phone numbers.
- Vendors may serve multiple libraries.
- Books can have multiple authors.
- Loans are managed by employees through the *Offer* relationship.

### Outcomes 

| **Concept**        | **Learning Outcome**                                                |
| ------------------ | ------------------------------------------------------------------- |
| ER Modeling        | Modeled logical structure of data using entities and relationships. |
| Relational Mapping | Converted ER model to relational schema with constraints.           |
| Normalization      | Achieved up to 3NF, ensuring efficiency and consistency.            |
| SQL DDL/DML        | Designed and populated the database using MySQL.                    |
| Query Writing      | Developed complex joins and aggregate queries for analysis.         |
| Documentation      | Structured Markdown documentation for professional reporting.       |


### 12. Conclusion

The Library Management System project demonstrates the complete lifecycle of database design, from concept to implementation.

By applying normalization principles and building relational models, the system ensures data integrity, reduces redundancy, and supports reliable operations for library workflows.

This project showcases strong practical understanding of database design, SQL implementation, and documentation aligning with the learning goals of the Database Design and Programming course.
---

📘 *Developed as part of the Database Management coursework for practicing SQL normalization, ER modeling, and relational schema design.*
