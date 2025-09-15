# Library Management 




#  Library Management System – Entity Attributes 

---

## Authors
| Attribute     | Type    | Notes          |
|---------------|---------|----------------|
| **author_id** | PK      | Primary Key    |
| full_name     | Simple  |                |
| birth_year    | Simple  |                |
| country       | Simple  |                |

---

## Books
| Attribute        | Type            | Notes                          |
|------------------|-----------------|--------------------------------|
| **book_id**      | PK              | Primary Key                    |
| isbn             | Simple, Unique  | Each book has unique ISBN       |
| title            | Simple          |                                |
| publication_year | Simple          |                                |
| language         | Simple          |                                |
| edition          | Simple          |                                |

---

## BookCopies (Weak Entity of Books)
| Attribute        | Type            | Notes                                      |
|------------------|-----------------|--------------------------------------------|
| **copy_id**      | Partial Key     | Not unique without book_id                 |
| condition        | Simple          | e.g., good, damaged                        |
| acquisition_date | Simple          |                                            |
| **book_id**      | FK, Owner Key   | Part of composite PK with copy_id          |

---

## Members
| Attribute     | Type            | Notes                          |
|---------------|-----------------|--------------------------------|
| **member_id** | PK              | Primary Key                    |
| full_name     | Simple          |                                |
| email         | Simple, Unique  | Must be unique per member       |
| phone         | Multi-valued    | Member may have multiple phones |
| joined_on     | Simple          |                                |
| status        | Simple          | e.g., active, suspended, closed |

---

## Addresses (Weak Entity of Members)
| Attribute     | Type            | Notes                                 |
|---------------|-----------------|---------------------------------------|
| **address_id**| Partial Key     | Not unique without member_id          |
| street        | Simple          |                                       |
| city          | Simple          |                                       |
| postal_code   | Simple          |                                       |
| country       | Simple          |                                       |
| **member_id** | FK, Owner Key   | Part of composite PK with address_id  |

---

## Staff
| Attribute     | Type            | Notes                          |
|---------------|-----------------|--------------------------------|
| **staff_id**  | PK              | Primary Key                    |
| full_name     | Simple          |                                |
| email         | Simple, Unique  | Must be unique per staff        |
| role          | Simple          | e.g., librarian, assistant      |
| hired_on      | Simple          |                                |

---

## Dependents (Weak Entity of Staff)
| Attribute       | Type          | Notes                                 |
|-----------------|---------------|---------------------------------------|
| **dependent_id**| Partial Key   | Not unique without staff_id           |
| name            | Simple        |                                       |
| relationship    | Simple        | e.g., child, spouse                   |
| birthdate       | Simple        |                                       |
| **staff_id**    | FK, Owner Key | Part of composite PK with dependent_id|

---

## Loans
| Attribute     | Type            | Notes                                         |
|---------------|-----------------|-----------------------------------------------|
| **loan_id**   | PK              | Primary Key                                   |
| loan_date     | Simple          |                                               |
| due_date      | Simple          |                                               |
| return_date   | Derived         | Based on loan_date + rules                    |
| late_fee      | Derived         | Calculated based on due_date vs return_date   |
| **book_id**   | FK              | References Books                              |
| **member_id** | FK              | References Members                            |
| **staff_id**  | FK              | References Staff                              |

---

## Reservations
| Attribute        | Type    | Notes                                |
|------------------|---------|---------------------------------------|
| **reservation_id**| PK     | Primary Key                          |
| reservation_date | Simple  |                                      |
| status           | Simple  | e.g., active, cancelled, fulfilled    |
| **member_id**    | FK      | References Members                   |
| **book_id**      | FK      | References Books                     |

---

## Fines
| Attribute     | Type    | Notes                                |
|---------------|---------|---------------------------------------|
| **fine_id**   | PK      | Primary Key                          |
| amount        | Simple  |                                      |
| status        | Simple  | e.g., paid, unpaid                   |
| issued_date   | Simple  |                                      |
| paid_date     | Simple  |                                      |
| **loan_id**   | FK      | References Loans                     |
| **member_id** | FK      | References Members                   |

---

## Payments
| Attribute     | Type    | Notes                                |
|---------------|---------|---------------------------------------|
| **payment_id**| PK      | Primary Key                          |
| amount        | Simple  |                                      |
| method        | Simple  | e.g., cash, card, online             |
| payment_date  | Simple  |                                      |
| **fine_id**   | FK      | References Fines                     |

---

## Publishers
| Attribute       | Type    | Notes          |
|-----------------|---------|----------------|
| **publisher_id**| PK      | Primary Key    |
| name            | Simple  |                |
| address         | Simple  |                |
| contact_email   | Simple  |                |

---

## Categories
| Attribute       | Type    | Notes          |
|-----------------|---------|----------------|
| **category_id** | PK      | Primary Key    |
| name            | Simple  | e.g., Fiction, Science |
| description     | Simple  |                |

---

## Branch
| Attribute     | Type    | Notes          |
|---------------|---------|----------------|
| **branch_id** | PK      | Primary Key    |
| branch_name   | Simple  |                |
| address       | Simple  |                |
| phone         | Simple  |                |
