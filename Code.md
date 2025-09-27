```sql
CREATE DATABASE library_db;

CREATE TABLE library (
    lib_no       INT PRIMARY KEY,
    lib_name     VARCHAR(150) NOT NULL,
    lib_location VARCHAR(150) NOT NULL
);

CREATE TABLE vendor (
    vendor_id    INT PRIMARY KEY,
    name         VARCHAR(150) NOT NULL,
    email        VARCHAR(150) UNIQUE
);

CREATE TABLE books (
    book_id      INT PRIMARY KEY,
    title        VARCHAR(250) NOT NULL,
    language     VARCHAR(80),
    publish_year INT
);

CREATE TABLE author (
    author_id    INT PRIMARY KEY,
    f_name       VARCHAR(150) NOT NULL,
    birth_date   DATE,
    sex          CHAR(1) CHECK (sex IN ('M','F'))
);

CREATE TABLE employee (
    emp_id       INT PRIMARY KEY,
    name         VARCHAR(150) NOT NULL,
    email        VARCHAR(150) UNIQUE,
    bod          DATE,
    sec          VARCHAR(80),
    lib_no       INT REFERENCES library(lib_no),
    manager_id   INT REFERENCES employee(emp_id) ON DELETE SET NULL
);

CREATE TABLE member (
    m_id         INT PRIMARY KEY,
    f_name       VARCHAR(120),
    l_name       VARCHAR(120),
    email        VARCHAR(150) UNIQUE,
    lib_no       INT REFERENCES library(lib_no),
    emp_id       INT UNIQUE REFERENCES employee(emp_id)    
);

CREATE TABLE loan (
    loan_id      INT PRIMARY KEY,
    loan_date    DATE NOT NULL,
    return_date  DATE,
    member_id    INT REFERENCES member(m_id) ON DELETE SET NULL
);

CREATE TABLE donations (
    donator_id    INT PRIMARY KEY,
    donator_name  VARCHAR(150),
    donator_email VARCHAR(150),
    lib_no        INT REFERENCES library(lib_no)
);

-- Weak entity: Emp_Dependent (identified by emp_id + name)
CREATE TABLE emp_dependent (
    name      VARCHAR(150),
    sex       CHAR(1) CHECK (sex IN ('M','F')),
    emp_id    INT,
    PRIMARY KEY (name, emp_id),
    FOREIGN KEY (emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE
);

-- Multivalued: Emp_Mobile
CREATE TABLE emp_mobile (
    emp_id   INT,
    mob_no   VARCHAR(30),
    PRIMARY KEY (emp_id, mob_no),
    FOREIGN KEY (emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE
);

-- Multivalued vendor_mobile
CREATE TABLE vendor_mobile (
    vendor_id INT,
    mobile_no VARCHAR(30),
    PRIMARY KEY (vendor_id, mobile_no),
    FOREIGN KEY (vendor_id) REFERENCES vendor(vendor_id) ON DELETE CASCADE
);

-- Junction table for Books <-> Author (M:N)
CREATE TABLE book_author (
    book_id   INT,
    author_id INT,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE,
    FOREIGN KEY (author_id) REFERENCES author(author_id) ON DELETE CASCADE
);

-- Buys: Library <-> Vendor (M:N)
CREATE TABLE buys (
    lib_no    INT,
    vendor_id INT,
    PRIMARY KEY (lib_no, vendor_id),
    FOREIGN KEY (lib_no) REFERENCES library(lib_no) ON DELETE CASCADE,
    FOREIGN KEY (vendor_id) REFERENCES vendor(vendor_id) ON DELETE CASCADE
);

-- Offer: Employee <-> Loan (M:N)
CREATE TABLE offer (
    emp_id   INT,
    loan_id  INT,
    PRIMARY KEY (emp_id, loan_id),
    FOREIGN KEY (emp_id) REFERENCES employee(emp_id) ON DELETE CASCADE,
    FOREIGN KEY (loan_id) REFERENCES loan(loan_id) ON DELETE CASCADE
);


-- Libraries
INSERT INTO library (lib_no, lib_name, lib_location) VALUES
(1, 'Central Library', 'Helsinki'),
(2, 'Westside Library', 'Espoo');

-- Vendors
INSERT INTO vendor (vendor_id, name, email) VALUES
(701, 'BookWorld Oy', 'sales@bookworld.fi'),
(702, 'EduBooks Ltd', 'contact@edubooks.com');

-- Books
INSERT INTO books (book_id, title, language, publish_year) VALUES
(101, 'Data Structures in C++', 'English', 2018),
(102, 'Introduction to Databases', 'English', 2020),
(103, 'Artificial Intelligence Basics', 'English', 2021),
(104, 'Introduction to Algorithms', 'English', 2009);

-- Authors
INSERT INTO author (author_id, f_name, birth_date, sex) VALUES
(201, 'John Smith', '1975-06-15', 'M'),
(202, 'Emma Johnson', '1980-11-22', 'F'),
(203, 'Robert Brown', '1968-05-30', 'M'),
(204, 'Alice Green', '1992-02-14', 'F');

-- Employees
INSERT INTO employee (emp_id, name, email, bod, sec, lib_no, manager_id) VALUES
(301, 'Alice Brown', 'alice.brown@library.fi', '1985-03-10', 'HR', 1, NULL),
(302, 'Bob Martin', 'bob.martin@library.fi', '1990-07-25', 'Finance', 1, 301),
(303, 'Carol White', 'carol.white@library.fi', '1988-01-18', 'IT', 2, 301),
(304, 'David Lee', 'david.lee@library.fi', '1992-09-05', 'Assistant', 2, 303);

-- Members
-- Note: m_id 405 is linked to emp_id 301 to illustrate Employee-Member 1:1 relation
INSERT INTO member (m_id, f_name, l_name, email, lib_no, emp_id) VALUES
(401, 'David', 'Green', 'david.green@mail.com', 1, NULL),
(402, 'Sophia', 'Black', 'sophia.black@mail.com', 1, NULL),
(403, 'James', 'Wilson', 'james.wilson@mail.com', 2, NULL),
(404, 'Emily', 'Davis', 'emily.davis@mail.com', 1, NULL),
(405, 'Michael', 'Scott', 'michael.scott@mail.com', 2, 301);

-- Loans
INSERT INTO loan (loan_id, loan_date, return_date, member_id) VALUES
(501, '2025-09-01', '2025-09-15', 401),
(502, '2025-09-05', NULL, 402),
(503, '2025-09-10', NULL, 403);

-- Donations
INSERT INTO donations (donator_id, donator_name, donator_email, lib_no) VALUES
(601, 'Anna Clark', 'anna.clark@mail.com', 1),
(602, 'Mike Lee', 'mike.lee@mail.com', 2),
(603, 'Katherine Mills', 'katherine.mills@mail.com', 1);

-- Emp_Dependent (weak entity)
INSERT INTO emp_dependent (name, sex, emp_id) VALUES
('Lily Brown', 'F', 301),
('Tom Martin', 'M', 302),
('Sam Lee', 'M', 304);

-- Emp_Mobile (multivalued)
INSERT INTO emp_mobile (emp_id, mob_no) VALUES
(301, '+358401234567'),
(301, '+358401111222'),
(302, '+358409876543'),
(303, '+358407777888');

-- Vendor_Mobile (multivalued)
INSERT INTO vendor_mobile (vendor_id, mobile_no) VALUES
(701, '+358401112233'),
(701, '+358401998877'),
(702, '+358409998877');

-- Book_Author (M:N)
INSERT INTO book_author (book_id, author_id) VALUES
(101, 201),
(102, 202),
(103, 201),
(103, 204),
(104, 203);

-- Buys (Library <-> Vendor)
INSERT INTO buys (lib_no, vendor_id) VALUES
(1, 701),
(1, 702),
(2, 701);

-- Offer (Employee <-> Loan)
INSERT INTO offer (emp_id, loan_id) VALUES
(301, 501),
(302, 502),
(303, 503);

-- =====================================================
-- Quick verification SELECTs 
-- =====================================================
SELECT '--- LIBRARIES ---' AS info;       SELECT * FROM library ORDER BY lib_no;
SELECT '--- VENDORS ---' AS info;         SELECT * FROM vendor ORDER BY vendor_id;
SELECT '--- BOOKS ---' AS info;           SELECT * FROM books ORDER BY book_id;
SELECT '--- AUTHORS ---' AS info;         SELECT * FROM author ORDER BY author_id;
SELECT '--- EMPLOYEES ---' AS info;       SELECT * FROM employee ORDER BY emp_id;
SELECT '--- MEMBERS ---' AS info;         SELECT * FROM member ORDER BY m_id;
SELECT '--- LOANS ---' AS info;           SELECT * FROM loan ORDER BY loan_id;
SELECT '--- DONATIONS ---' AS info;       SELECT * FROM donations ORDER BY donator_id;
SELECT '--- EMP_DEPENDENTS ---' AS info;  SELECT * FROM emp_dependent ORDER BY emp_id, name;
SELECT '--- EMP_MOBILES ---' AS info;     SELECT * FROM emp_mobile ORDER BY emp_id;
SELECT '--- VENDOR_MOBILES ---' AS info;  SELECT * FROM vendor_mobile ORDER BY vendor_id;
SELECT '--- BOOK_AUTHOR ---' AS info;     SELECT * FROM book_author ORDER BY book_id;
SELECT '--- BUYS ---' AS info;            SELECT * FROM buys ORDER BY lib_no, vendor_id;
SELECT '--- OFFER ---' AS info;           SELECT * FROM offer ORDER BY emp_id, loan_id;
