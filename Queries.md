```sql
-- 1. List all books and their authors
SELECT b.title, a.f_name
FROM books b
JOIN book_author ba ON b.book_id = ba.book_id
JOIN author a ON ba.author_id = a.author_id;

-- 2. Show employees working in Kathmandu Valley Public Library
SELECT e.name, e.email, l.lib_name
FROM employee e
JOIN library l ON e.lib_no = l.lib_no
WHERE l.lib_name = 'Kathmandu Valley Public Library';

-- 3. Find all loans not yet returned
SELECT loan_id, member_id, loan_date
FROM loan
WHERE return_date IS NULL;

-- 4. Count books published after 2015
SELECT COUNT(*) AS total_recent_books
FROM books
WHERE publish_year > 2015;

-- 5. Each library and number of members
SELECT l.lib_name, COUNT(m.m_id) AS total_members
FROM library l
LEFT JOIN member m ON l.lib_no = m.lib_no
GROUP BY l.lib_name;

-- 6. List all managers
SELECT DISTINCT m.name AS manager_name
FROM employee e
JOIN employee m ON e.manager_id = m.emp_id;

-- 7. Vendors and libraries they supply
SELECT v.name AS vendor_name, l.lib_name
FROM vendor v
JOIN buys b ON v.vendor_id = b.vendor_id
JOIN library l ON b.lib_no = l.lib_no;

-- 8. Members who borrowed before 2025-09-05
SELECT f_name, l_name
FROM member
WHERE m_id IN (SELECT member_id FROM loan WHERE loan_date < '2025-09-05');

-- 9. Total donations per library
SELECT l.lib_name, COUNT(d.donator_id) AS total_donations
FROM library l
LEFT JOIN donations d ON l.lib_no = d.lib_no
GROUP BY l.lib_name;

-- 10. Employees with multiple phone numbers
SELECT e.name, COUNT(em.mob_no) AS phone_count
FROM employee e
JOIN emp_mobile em ON e.emp_id = em.emp_id
GROUP BY e.name
HAVING COUNT(em.mob_no) > 1;
