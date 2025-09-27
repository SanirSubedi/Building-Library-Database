# Library Management System – Entities & Relationships

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
| **Vendor**       | Vendor_ID (PK), Vendor_Name, Contact_email *(added for Library–Vendor relation)* ,moobile_no|

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


#  Library Management System – Sample Data

## Library
| Lib_Number | Lib_Name          | Lib_Location |
|------------|------------------|--------------|
| 1          | Central Library  | Helsinki     |
| 2          | Westside Library | Espoo        |

---

## Books
| Book_id | Title                          | Language | Publish_year |
|---------|--------------------------------|----------|--------------|
| 101     | Data Structures in C++         | English  | 2018         |
| 102     | Introduction to Databases      | English  | 2020         |
| 103     | Artificial Intelligence Basics | English  | 2021         |

---

## Author
| Author_id | F_name       | Birth_date | Sex |
|-----------|-------------|------------|-----|
| 201       | John Smith  | 1975-06-15 | M   |
| 202       | Emma Johnson| 1980-11-22 | F   |

---

## Employee
| Emp_Id | Name        | Email                      | BOD        | Sec     | Lib_no | Manager_id |
|--------|------------|----------------------------|------------|---------|--------|------------|
| 301    | Alice Brown| alice.brown@library.fi     | 1985-03-10 | HR      | 1      | NULL       |
| 302    | Bob Martin | bob.martin@library.fi      | 1990-07-25 | Finance | 1      | 301        |
| 303    | Carol White| carol.white@library.fi     | 1988-01-18 | IT      | 2      | 301        |

---

## Member
| M_Id | F_name  | L_name   | Email                    | Loan_id | Lib_id |
|------|---------|----------|--------------------------|---------|--------|
| 401  | David   | Green    | david.green@mail.com     | 501     | 1      |
| 402  | Sophia  | Black    | sophia.black@mail.com    | 502     | 1      |
| 403  | James   | Wilson   | james.wilson@mail.com    | 503     | 2      |

---

## Loan
| Loan_Id | Loan_date  | Return_date | Member_id |
|---------|------------|-------------|-----------|
| 501     | 2024-01-12 | 2024-02-12  | 401       |
| 502     | 2024-01-15 | 2024-02-15  | 402       |
| 503     | 2024-01-20 | 2024-02-20  | 403       |

---

## Emp_Dependent (Weak Entity)
| Name   | Sex | Emp_id |
|--------|-----|--------|
| Lily   | F   | 301    |
| Tom    | M   | 302    |

---

## Donations
| Donator_ID | Donator_Name | Donator_email        | Lib_no |
|------------|--------------|----------------------|--------|
| 601        | Anna Clark   | anna.clark@mail.com  | 1      |
| 602        | Mike Lee     | mike.lee@mail.com    | 2      |

---

## Buys (M:N)
| Lib_no | Vendor_id |
|--------|-----------|
| 1      | 701       |
| 1      | 702       |
| 2      | 701       |

---

## Offer (M:N)
| Emp_id | Loan_no |
|--------|---------|
| 301    | 501     |
| 302    | 502     |

---

## Emp-Mobile (Multivalued)
| Emp_id | Mob_no     |
|--------|------------|
| 301    | 0401234567 |
| 301    | 0407654321 |
| 302    | 0419876543 |

---

## Vendor_Mobile (Multivalued)
| Vendor_id | Mobile_no |
|-----------|-----------|
| 701       | 050111222 |
| 702       | 050333444 |

---

## Vendor
| Vendor_id | Name         | Mobile    | Email                 |
|-----------|--------------|-----------|-----------------------|
| 701       | BookWorld Oy | 050111222 | sales@bookworld.fi    |
| 702       | EduBooks Ltd | 050333444 | contact@edubooks.com  |

