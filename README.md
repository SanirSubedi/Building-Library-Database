# 📚 Library Management System – Entities & Relationships

## 1. Entities and Attributes

| Entity           | Attributes                                                                 |
|------------------|----------------------------------------------------------------------------|
| **Library**      | Lib_Name, Lib_Number, Lib_Location                                         |
| **Books**        | Book_id (PK), Title, Language, Publish_year                                |
| **Author**       | Author_id (PK), F_name, Birth_date, Sex                                    |
| **Employee**     | Emp_Id (PK), Name, Email, BOD, Sec                                         |
| **Member**       | M_Id (PK), F_name, L_name, Email                                           |
| **Loan**         | Loan_Id (PK), Loan_date, Return_date                                       |
| **Emp_Dependent** *(Weak)* | Name, Sex (Identified by Emp_Id)                                |
| **Donations**    | Donator_ID (PK), Donator_Name, Donator_email                               |
| **Vendor**       | Vendor_ID (PK), Vendor_Name, Contact_email *(added for Library–Vendor relation)* |

---

## 2. Relationships

| Relationship            | Type   | Cardinality | Participation                          | Description |
|--------------------------|--------|-------------|----------------------------------------|-------------|
| Library – Books          | Strong | M:N         | Library **total**, Book partial         | A library offers many books; a book may exist in multiple libraries. |
| Library – Vendors        | Strong | M:N         | Both partial                           | A library can order from many vendors; a vendor supplies many libraries. |
| Employee – Library       | Strong | N:1         | Employee **total**, Library partial     | Every employee works in one library. |
| Employee – Emp_Dependent | Weak   | 1:N         | Employee **total**, Dependent **total** | A dependent cannot exist without an employee. |
| Employee – Member        | Strong | 1:1         | Both partial                            | An employee may also be a member, and vice versa. |
| Member – Loan            | Strong | N:1         | Loan **total**, Member partial          | Every loan must belong to a member. |
| Book – Author            | Strong | M:N         | Book **total**, Author partial          | A book can have multiple authors; an author can write many books. |
| Library – Donations      | Strong | 1:N         | Donation **total**, Library partial     | Each donation belongs to one library; a library may have many donations. |
| Library – Member         | Strong | 1:N         | Member **total**, Library partial       | A library has many members; each member must belong to one library. |

---

## 3. Participation Summary

| Relationship            | Entity with **Total** Participation |
|--------------------------|--------------------------------------|
| Library – Books          | Library                             |
| Library – Vendors        | None (both partial)                 |
| Employee – Library       | Employee                            |
| Employee – Emp_Dependent | Employee, Dependent                 |
| Employee – Member        | None (both partial)                 |
| Member – Loan            | Loan                                |
| Book – Author            | Book                                |
| Library – Donations      | Donation                            |
| Library – Member         | Member                              |
