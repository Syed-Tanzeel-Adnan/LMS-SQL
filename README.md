# LMS-SQL


## Project Overview

**Project Title**: Library Management System  
**Level**: Intermediate  
**Database**: `library_db`

This project demonstrates the implementation of a Library Management System using SQL. It includes creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation, and querying.

![Library_project]([library.png](https://github.com/Syed-Tanzeel-Adnan/LMS-SQL/blob/main/Screenshot%202024-10-16%20203708.png)

## Objectives

1. **Set up the Library Management System Database**: Create and populate the database with tables for branches, employees, members, books, issued status, and return status.
2. **CRUD Operations**: Perform Create, Read, Update, and Delete operations on the data.
3. **CTAS (Create Table As Select)**: Utilize CTAS to create new tables based on query results.
4. **Advanced SQL Queries**: Develop complex queries to analyze and retrieve specific data.

## Project Structure

### 1. Database Setup
![ERD](library_erd.png)

- **Database Creation**: Created a database named `library_db`.
- **Table Creation**: Created tables for branches, employees, members, books, issued status, and return status. Each table includes relevant columns and relationships.

```sql

CREATE DATABASE library_db;
DROP TABLE IF EXISTS members;
CREATE TABLE members
(
	member_id VARCHAR(15) PRIMARY KEY,
	member_name VARCHAR(30),
	member_address VARCHAR(30),
	reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);


-- STEP 2 - INSERT INTO "Members" TABLE
INSERT INTO members(member_id, member_name, member_address, reg_date) 
VALUES
('C101', 'Pranay Bollam', '123 Main City', '2021-05-15'),
('C102', 'Thoodi Rahul', '456 Vasanth Nagar', '2021-06-20'),
('C103', 'Shaikh Tausif Ahmed', '789 Madikonda St', '2021-07-10'),
('C104', 'Zoheb', '567 Raipura St', '2021-08-05'),
('C105', 'Shiva Kuttan', '890 Husnabad High', '2021-09-25'),
('C106', 'Kundarapu Karthik', '234 Markazi St', '2021-10-15'),
('C107', 'Syed Tanzeel Adnan', '1413 Gopalpur St', '2021-11-20'),
('C108', 'Freddy kajrekar', '456 Malinda St', '2021-12-10'),
('C109', 'Pallavi', '567 Oak St', '2022-01-05'),
('C110', 'Safa', '678 Pine St', '2022-02-25'),
('C111', 'Akhila', '673 High St', '2022-05-25'),
('C118', 'Kaneez Fatima', '133 City St', '2024-06-01'),    
('C119', 'Rana Tanveer', '143 KU First Gate', '2024-05-01');
SELECT * FROM members;



-- STEP 3 - CREATE BRANCH TABLE
DROP TABLE IF EXISTS branch;
CREATE TABLE branch
(
	branch_id VARCHAR(10) PRIMARY KEY,
	manager_id VARCHAR(10),
	branch_address VARCHAR(30),
	contact_no VARCHAR(15)
    -- ,FOREIGN KEY (manager_id) REFERENCES employees (emp_id) ON UPDATE CASCADE
);



-- STEP 4 - Create table "Employee"
DROP TABLE IF EXISTS employees;
CREATE TABLE employees
(
	emp_id VARCHAR(10) PRIMARY KEY,
	emp_name VARCHAR(30),
	position VARCHAR(30),
	salary DECIMAL(10,2),
	branch_id VARCHAR(10),
	FOREIGN KEY (branch_id) REFERENCES  branch(branch_id) ON UPDATE CASCADE
);


-- STEP 5 - ADD FOREIGN KEY CONSTRAINT TO "branch" TABLE
ALTER TABLE branch
ADD CONSTRAINT fk_branch
FOREIGN KEY (manager_id)
REFERENCES employees (emp_id)
ON UPDATE CASCADE;


-- Step 6 -  Drop the foreign key constraint on "branch" table
ALTER TABLE branch
DROP FOREIGN KEY fk_branch;


-- Step 7 - Insert Into "branch"
INSERT INTO branch(branch_id, manager_id, branch_address, contact_no) 
VALUES
('B001', 'E109', '123 Main St', '+919099988676'),
('B002', 'E109', '456 Elm St', '+919099988677'),
('B003', 'E109', '789 Oak St', '+919099988678'),
('B004', 'E110', '567 Pine St', '+919099988679'),
('B005', 'E110', '890 Maple St', '+919099988680');
SELECT * FROM branch;



-- Step 8 - Insert Into "Employees"
INSERT INTO employees(emp_id, emp_name, position, salary, branch_id) 
VALUES
('E101', 'John Doe', 'Clerk', 60000.00, 'B001'),
('E102', 'Jane Smith', 'Clerk', 45000.00, 'B002'),
('E103', 'Mike Johnson', 'Librarian', 55000.00, 'B001'),
('E104', 'Emily Davis', 'Assistant', 40000.00, 'B001'),
('E105', 'Sarah Brown', 'Assistant', 42000.00, 'B001'),
('E106', 'Michelle Ramirez', 'Assistant', 43000.00, 'B001'),
('E107', 'Michael Thompson', 'Clerk', 62000.00, 'B005'),
('E108', 'Jessica Taylor', 'Clerk', 46000.00, 'B004'),
('E109', 'Daniel Anderson', 'Manager', 57000.00, 'B003'),
('E110', 'Laura Martinez', 'Manager', 41000.00, 'B005'),
('E111', 'Christopher Lee', 'Assistant', 65000.00, 'B005');
SELECT * FROM employees;



-- Step 9 - Add constraint to the "branch" table
ALTER TABLE branch
ADD CONSTRAINT fk_branch
FOREIGN KEY (manager_id)
REFERENCES employees (emp_id)
ON UPDATE CASCADE;


-- Step 10 - Create table "Books"
DROP TABLE IF EXISTS books;
CREATE TABLE books
(
	isbn VARCHAR(50) PRIMARY KEY,
	book_title VARCHAR(80),
	category VARCHAR(30),
	rental_price DECIMAL(10,2),
	author VARCHAR(30),
	publisher VARCHAR(30),
    in_stock INT
);

-- Step 11- Insert Into "books"
INSERT INTO books(isbn, book_title, category, rental_price, author, publisher, in_stock) 
VALUES
('978-0-553-29698-2', 'The Catcher in the Rye', 'Classic', 7.00, 'J.D. Salinger', 'Little, Brown and Company', 5),
('978-0-330-25864-8', 'Animal Farm', 'Classic', 5.50, 'George Orwell', 'Penguin Books', 4),
('978-0-14-118776-1', 'One Hundred Years of Solitude', 'Literary Fiction', 6.50, 'Gabriel Garcia Marquez', 'Penguin Books', 2),
('978-0-525-47535-5', 'The Great Gatsby', 'Classic', 8.00, 'F. Scott Fitzgerald', 'Scribner', 5),
('978-0-141-44171-6', 'Jane Eyre', 'Classic', 4.00,'Charlotte Bronte', 'Penguin Classics', 3),
('978-0-307-37840-1', 'The Alchemist', 'Fiction', 2.50, 'Paulo Coelho', 'HarperOne', 8),
('978-0-679-76489-8', 'Harry Potter and the Sorcerers Stone', 'Fantasy', 7.00, 'J.K. Rowling', 'Scholastic', 5),
('978-0-7432-4722-4', 'The Da Vinci Code', 'Mystery', 8.00, 'Dan Brown', 'Doubleday', 5),
('978-0-09-957807-9', 'A Game of Thrones', 'Fantasy', 7.50, 'George R.R. Martin', 'Bantam', 4),
('978-0-393-05081-8', 'A Peoples History of the United States', 'History', 9.00, 'Howard Zinn', 'Harper Perennial', 6),
('978-0-19-280551-1', 'The Guns of August', 'History', 7.00, 'Barbara W. Tuchman', 'Oxford University Press', 7),
('978-0-307-58837-1', 'Sapiens: A Brief History of Humankind', 'History', 8.00, 'Yuval Noah Harari', 'Harper Perennial', 4),
('978-0-375-41398-8', 'The Diary of a Young Girl', 'History', 6.50, 'Anne Frank', 'Bantam', 3),
('978-0-14-044930-3', 'The Histories', 'History', 5.50, 'Herodotus', 'Penguin Classics', 9),
('978-0-393-91257-8', 'Guns, Germs, and Steel: The Fates of Human Societies', 'History', 7.00, 'Jared Diamond', 'W. W. Norton & Company', 4),
('978-0-7432-7357-1', '1491: New Revelations of the Americas Before Columbus', 'History', 6.50,'Charles C. Mann', 'Vintage Books', 4),
('978-0-679-64115-3', '1984', 'Dystopian', 6.50, 'George Orwell', 'Penguin Books', 6),
('978-0-14-143951-8', 'Pride and Prejudice', 'Classic', 5.00, 'Jane Austen', 'Penguin Classics', 5),
('978-0-452-28240-7', 'Brave New World', 'Dystopian', 6.50, 'Aldous Huxley', 'Harper Perennial', 4),
('978-0-670-81302-4', 'The Road', 'Dystopian', 7.00, 'Cormac McCarthy', 'Knopf', 7),
('978-0-385-33312-0', 'The Shining', 'Horror', 6.00, 'Stephen King', 'Doubleday', 5),
('978-0-451-52993-5', 'Fahrenheit 451', 'Dystopian', 5.50, 'Ray Bradbury', 'Ballantine Books', 10),
('978-0-345-39180-3', 'Dune', 'Science Fiction', 8.50, 'Frank Herbert', 'Ace', 12),
('978-0-375-50167-0', 'The Road', 'Dystopian', 7.00, 'Cormac McCarthy', 'Vintage', 8),
('978-0-06-025492-6', 'Where the Wild Things Are', 'Children', 3.50, 'Maurice Sendak', 'HarperCollins', 13),
('978-0-06-112241-5', 'The Kite Runner', 'Fiction', 5.50, 'Khaled Hosseini', 'Riverhead Books', 7),
('978-0-06-440055-8', 'Charlotte''s Web', 'Children', 4.00,'E.B. White', 'Harper & Row', 6),
('978-0-679-77644-3', 'Beloved', 'Fiction', 6.50, 'Toni Morrison', 'Knopf', 4),
('978-0-14-027526-3', 'A Tale of Two Cities', 'Classic', 4.50, 'Charles Dickens', 'Penguin Books', 6),
('978-0-7434-7679-3', 'The Stand', 'Horror', 7.00, 'Stephen King', 'Doubleday', 3),
('978-0-451-52994-2', 'Moby Dick', 'Classic', 6.50, 'Herman Melville', 'Penguin Books', 5),
('978-0-06-112008-4', 'To Kill a Mockingbird', 'Classic', 5.00, 'Harper Lee', 'J.B. Lippincott & Co.', 9),
('978-0-553-57340-1', '1984', 'Dystopian', 6.50, 'George Orwell', 'Penguin Books', 6),
('978-0-7432-4722-5', 'Angels & Demons', 'Mystery', 7.50, 'Dan Brown', 'Doubleday', 4),
('978-0-7432-7356-4', 'The Hobbit', 'Fantasy', 7.00, 'J.R.R. Tolkien', 'Houghton Mifflin Harcourt', 6);
select * from books;


-- Step 12 - Create table "IssueStatus"
DROP TABLE IF EXISTS issued_status;
CREATE TABLE issued_status
(
	issued_id VARCHAR(10) PRIMARY KEY,
	issued_member_id VARCHAR(30),
	issued_book_name VARCHAR(80),
	issued_date TIMESTAMP DEFAULT NOW(),
	issued_book_isbn VARCHAR(50),
	issued_emp_id VARCHAR(10),
	FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
	FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
	FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn) 
);



-- Step 13 - Insert Into "issued_status"
INSERT INTO issued_status(issued_id, issued_member_id, issued_book_name, issued_date, issued_book_isbn, issued_emp_id) 
VALUES
('IS106', 'C106', 'Animal Farm', '2024-03-10', '978-0-330-25864-8', 'E104'),
('IS107', 'C107', 'One Hundred Years of Solitude', '2024-03-11', '978-0-14-118776-1', 'E104'),
('IS108', 'C108', 'The Great Gatsby', '2024-03-12', '978-0-525-47535-5', 'E104'),
('IS109', 'C109', 'Jane Eyre', '2024-03-13', '978-0-141-44171-6', 'E105'),
('IS110', 'C110', 'The Alchemist', '2024-03-14', '978-0-307-37840-1', 'E105'),
('IS111', 'C109', 'Harry Potter and the Sorcerers Stone', '2024-03-15', '978-0-679-76489-8', 'E105'),
('IS112', 'C109', 'A Game of Thrones', '2024-03-16', '978-0-09-957807-9', 'E106'),
('IS113', 'C109', 'A Peoples History of the United States', '2024-03-17', '978-0-393-05081-8', 'E106'),
('IS114', 'C109', 'The Guns of August', '2024-03-18', '978-0-19-280551-1', 'E106'),
('IS115', 'C109', 'The Histories', '2024-03-19', '978-0-14-044930-3', 'E107'),
('IS116', 'C110', 'Guns, Germs, and Steel: The Fates of Human Societies', '2024-03-20', '978-0-393-91257-8', 'E107'),
('IS117', 'C110', '1984', '2024-03-21', '978-0-679-64115-3', 'E107'),
('IS118', 'C101', 'Pride and Prejudice', '2024-03-22', '978-0-14-143951-8', 'E108'),
('IS119', 'C110', 'Brave New World', '2024-03-23', '978-0-452-28240-7', 'E108'),
('IS120', 'C110', 'The Road', '2024-03-24', '978-0-670-81302-4', 'E108'),
('IS121', 'C102', 'The Shining', '2024-03-25', '978-0-385-33312-0', 'E109'),
('IS122', 'C102', 'Fahrenheit 451', '2024-03-26', '978-0-451-52993-5', 'E109'),
('IS123', 'C103', 'Dune', '2024-03-27', '978-0-345-39180-3', 'E109'),
('IS124', 'C104', 'Where the Wild Things Are', '2024-03-28', '978-0-06-025492-6', 'E110'),
('IS125', 'C105', 'The Kite Runner', '2024-03-29', '978-0-06-112241-5', 'E110'),
('IS126', 'C105', 'Charlotte''s Web', '2024-03-30', '978-0-06-440055-8', 'E110'),
('IS127', 'C105', 'Beloved', '2024-03-31', '978-0-679-77644-3', 'E110'),
('IS128', 'C105', 'A Tale of Two Cities', '2024-04-01', '978-0-14-027526-3', 'E110'),
('IS129', 'C105', 'The Stand', '2024-04-02', '978-0-7434-7679-3', 'E110'),
('IS130', 'C106', 'Moby Dick', '2024-04-03', '978-0-451-52994-2', 'E101'),
('IS131', 'C106', 'To Kill a Mockingbird', '2024-04-04', '978-0-06-112008-4', 'E101'),
('IS132', 'C106', 'The Hobbit', '2024-04-05', '978-0-7432-7356-4', 'E106'),
('IS133', 'C107', 'Angels & Demons', '2024-04-06', '978-0-7432-4722-5', 'E106'),
('IS134', 'C107', 'The Diary of a Young Girl', '2024-04-07', '978-0-375-41398-8', 'E106'),
('IS135', 'C107', 'Sapiens: A Brief History of Humankind', '2024-04-08', '978-0-307-58837-1', 'E108'),
('IS136', 'C107', '1491: New Revelations of the Americas Before Columbus', '2024-04-09', '978-0-7432-7357-1', 'E102'),
('IS137', 'C107', 'The Catcher in the Rye', '2024-04-10', '978-0-553-29698-2', 'E103'),
('IS138', 'C108', 'The Great Gatsby', '2024-04-11', '978-0-525-47535-5', 'E104'),
('IS139', 'C109', 'Harry Potter and the Sorcerers Stone', '2024-04-12', '978-0-679-76489-8', 'E105'),
('IS140', 'C110', 'Animal Farm', '2024-04-13', '978-0-330-25864-8', 'E102');
select * from issued_status;
-- ALTER TABLE issued_status ADD status_return ENUM('yes', 'no') DEFAULT 'no';



-- Step 14 - Create Table "return_status"
DROP TABLE IF EXISTS return_status;
CREATE TABLE return_status
(
	return_id VARCHAR(10) PRIMARY KEY,
	issued_id VARCHAR(30),
	return_book_name VARCHAR(80),
	return_date TIMESTAMP DEFAULT NOW(),
	return_book_isbn VARCHAR(50),
    book_quality VARCHAR(15) DEFAULT('Good'),
    paid DECIMAL(10,2),
	FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);



-- Step 15 - Insert Into "return_status"
INSERT INTO return_status(return_id, issued_id, return_date, paid) 
VALUES
('RS101', 'IS101', '2023-06-06', 29),
('RS102', 'IS105', '2023-06-07', 14.5),
('RS103', 'IS103', '2023-08-07', 41),
('RS104', 'IS106', '2024-05-01', 30),
('RS105', 'IS107', '2024-05-03', 25),
('RS106', 'IS108', '2024-05-05', 56),
('RS107', 'IS109', '2024-05-07', 42.5),
('RS108', 'IS110', '2024-05-09', 36),
('RS109', 'IS111', '2024-05-11', 18),
('RS110', 'IS112', '2024-05-13', 23.35),
('RS111', 'IS113', '2024-05-15', 35),
('RS112', 'IS114', '2024-05-17', 33),
('RS113', 'IS115', '2024-05-19', 40),
('RS114', 'IS116', '2024-05-21', 50),
('RS115', 'IS117', '2024-05-23', 24),
('RS116', 'IS118', '2024-05-25', 44),
('RS117', 'IS119', '2024-05-27', 22),
('RS118', 'IS120', '2024-05-29', 30);

```

### 2. CRUD Operations

- **Create**: Inserted sample records into the `books` table.
- **Read**: Retrieved and displayed data from various tables.
- **Update**: Updated records in the `employees` table.
- **Delete**: Removed records from the `members` table as needed.

**Task 1. Create a New Book Record**
-- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"

```sql
INSERT INTO books(isbn, book_title, category, rental_price, status, author, publisher)
VALUES('978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.');
SELECT * FROM books;
```
**Task 2: Update an Existing Member's Address**

```sql
UPDATE members
SET member_address = '125 Oak St'
WHERE member_id = 'C103';
```

**Task 3: Delete a Record from the Issued Status Table**
-- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.

```sql
DELETE FROM issued_status
WHERE   issued_id =   'IS121';
```

**Task 4: Retrieve All Books Issued by a Specific Employee**
-- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
SELECT * FROM issued_status
WHERE issued_emp_id = 'E101'
```


**Task 5: List Members Who Have Issued More Than One Book**
-- Objective: Use GROUP BY to find members who have issued more than one book.

```sql
SELECT
    issued_emp_id,
    COUNT(*)
FROM issued_status
GROUP BY 1
HAVING COUNT(*) > 1
```

### 3. CTAS (Create Table As Select)

- **Task 6: Create Summary Tables**: Used CTAS to generate new tables based on query results - each book and total book_issued_cnt**

```sql
CREATE TABLE book_issued_cnt AS
SELECT b.isbn, b.book_title, COUNT(ist.issued_id) AS issue_count
FROM issued_status as ist
JOIN books as b
ON ist.issued_book_isbn = b.isbn
GROUP BY b.isbn, b.book_title;
```


### 4. Data Analysis & Findings

The following SQL queries were used to address specific questions:

Task 7. **Retrieve All Books in a Specific Category**:

```sql
SELECT * FROM books
WHERE category = 'Classic';
```

8. **Task 8: Find Total Rental Income by Category**:

```sql
SELECT 
    b.category,
    SUM(b.rental_price),
    COUNT(*)
FROM 
issued_status as ist
JOIN
books as b
ON b.isbn = ist.issued_book_isbn
GROUP BY 1
```

9. **List Members Who Registered in the Last 180 Days**:
```sql
SELECT * FROM members
WHERE reg_date >= CURRENT_DATE - INTERVAL '180 days';
```

10. **List Employees with Their Branch Manager's Name and their branch details**:

```sql
SELECT 
    e1.emp_id,
    e1.emp_name,
    e1.position,
    e1.salary,
    b.*,
    e2.emp_name as manager
FROM employees as e1
JOIN 
branch as b
ON e1.branch_id = b.branch_id    
JOIN
employees as e2
ON e2.emp_id = b.manager_id
```

Task 11. **Create a Table of Books with Rental Price Above a Certain Threshold**:
```sql
CREATE TABLE expensive_books AS
SELECT * FROM books
WHERE rental_price > 7.00;
```

Task 12: **Retrieve the List of Books Not Yet Returned**
```sql
SELECT * FROM issued_status as ist
LEFT JOIN
return_status as rs
ON rs.issued_id = ist.issued_id
WHERE rs.return_id IS NULL;
```

## Advanced SQL Operations

**Task 13: Identify Members with Overdue Books**  
Write a query to identify members who have overdue books (assume a 30-day return period). Display the member's_id, member's name, book title, issue date, and days overdue.

```sql
SELECT i.issued_id, issued_member_id, i.issued_book_isbn, DATE(i.issued_date) AS issued_date, DATE(NOW()) - DATE(i.issued_date) AS days_overdue
FROM issued_status as i
LEFT JOIN return_status as r
ON i.issued_id = r.issued_id
WHERE return_id IS NULL AND DATE(NOW()) - DATE(i.issued_date) > 30
ORDER BY i.issued_id;


```


**Task 14: Update Book Status on Return**  
Write a query to update the status of books in the books table to "Yes" when they are returned (based on entries in the return_status table).


```sql

DROP PROCEDURE return_book;
DELIMITER $$
CREATE PROCEDURE return_book(
    IN p_return_id VARCHAR(10),
    IN p_issued_id VARCHAR(30),
    IN p_book_quality VARCHAR(15)
)
BEGIN
    DECLARE p_isbn VARCHAR(50);    
    DECLARE p_issued_date DATE;    -- Changed to DATE for simplicity
    DECLARE p_price DECIMAL(10,2); -- price of borrowing
    DECLARE days_used INT;         -- days difference
    DECLARE p_paid DECIMAL(10,2);  -- amount to be paid by the reader
    DECLARE v_message VARCHAR(255);

    -- Fetch the required details from issued_status
    SELECT issued_book_isbn, issued_date INTO p_isbn, p_issued_date
    FROM issued_status
    WHERE issued_id = p_issued_id;

    -- Fetch rental price from books 
    SELECT rental_price INTO p_price
    FROM books
    WHERE isbn = p_isbn;

    -- Calculate the number of days used
    SET days_used = DATEDIFF(DATE(NOW()), p_issued_date);

    -- Determine payment and update records
    IF days_used <= 30 THEN
        SET p_paid = days_used * p_price;
    ELSE
        SET p_paid = (30 * p_price) + ((days_used - 30) * p_price * 1.5); -- fine
    END IF;

    -- Insert into return_status table
    INSERT INTO return_status (return_id, issued_id, book_quality, paid)
    VALUES (p_return_id, p_issued_id, p_book_quality, p_paid);

    -- Update book status in books table
    UPDATE books SET in_stock = in_stock + 1 WHERE isbn = p_isbn;
	UPDATE issued_status SET status_return = 'yes' WHERE issued_id = p_issued_id;

    -- Set success message
    SET v_message = 'Book return successful. Thank you!!';
    SELECT v_message AS message;
END $$
DELIMITER ;

-- Returning a book with given 'return_id', 'issued_id' and 'book_quality'
CALL return_book('RS119', 'IS121', 'good');

```


**Task 15: CTAS: Create a Table of Active Members**  
Use the CREATE TABLE AS (CTAS) statement to create a new table active_members containing members who have issued at least one book in the last 2 months.

```sql

CREATE TABLE active_members
AS
SELECT * FROM members
WHERE member_id IN (SELECT 
                        DISTINCT issued_member_id   
                    FROM issued_status
                    WHERE 
                        issued_date >= CURRENT_DATE - INTERVAL '2 month'
                    )
;

SELECT * FROM active_members;

```


**Task 16: Find Employees with the Most Book Issues Processed**  
Write a query to find the top 3 employees who have processed the most book issues. Display the employee name, number of books processed, and their branch.

```sql
SELECT 
    e.emp_name,
    b.*,
    COUNT(ist.issued_id) as no_book_issued
FROM issued_status as ist
JOIN
employees as e
ON e.emp_id = ist.issued_emp_id
JOIN
branch as b
ON e.branch_id = b.branch_id
GROUP BY 1, 2
```

**Task 17: Stored Procedure**
Objective:
Create a stored procedure to manage the status of books in a library system.
Description:
Write a stored procedure that updates the status of a book in the library based on its issuance. The procedure should function as follows:
The stored procedure should take the book_id as an input parameter.
The procedure should first check if the book is available (status = 'yes').
If the book is available, it should be issued, and the status in the books table should be updated to 'no'.
If the book is not available (status = 'no'), the procedure should return an error message indicating that the book is currently not available.

```sql

DROP PROCEDURE IF EXISTS issue_book;
DELIMITER $$ 
CREATE PROCEDURE issue_book (
  IN v_issued_id         VARCHAR(10), 
  IN v_issued_member_id  VARCHAR(15),
  IN v_issued_book_isbn  VARCHAR(50),
  IN v_issued_emp_id     VARCHAR(10)
)
BEGIN
	DECLARE available INT;
    DECLARE v_issued_book_name VARCHAR(80);
    DECLARE fail_msg VARCHAR(100);
    DECLARE succ_msg VARCHAR(100);
    
    -- Setting Messages
    SET fail_msg = CONCAT('Sorry !!  your requested book with isbn no ', v_issued_book_isbn, ' is out of stock');
    SET succ_msg = CONCAT('Thank You for Borrowing our Book !!  Return Date : ', DATE_ADD(CURDATE(), INTERVAL 30 DAY));
    
    -- Fetching available books for the requested isbn
     SELECT in_stock INTO available FROM books WHERE isbn = v_issued_book_isbn;
     
     IF available <= 0 THEN                    -- checking the availaibility of Requested Book
	    SELECT fail_msg AS failure;
	 ELSE                                     --  There are enough books to Issue
        BEGIN
          SELECT book_title INTO v_issued_book_name FROM books WHERE isbn = v_issued_book_isbn;
          UPDATE books SET in_stock = in_stock - 1 WHERE isbn = v_issued_book_isbn;
          INSERT INTO issued_status (issued_id, issued_member_id, issued_book_name, issued_book_isbn, issued_emp_id)
          VALUES (v_issued_id, v_issued_member_id, v_issued_book_name, v_issued_book_isbn, v_issued_emp_id);
          SELECT succ_msg AS Successful;
        END;
     END IF;
END $$ 
DELIMITER ;

-- Issuing a book to the memeber
CALL issue_book('IS140', 'C101', '978-0-06-025492-6', 'E101');
-- Issuing same book to another member but there are no available Books 
CALL issue_book('IS141', 'C102', '978-0-06-025492-6', 'E101');

```


## Reports

- **Database Schema**: Detailed table structures and relationships.
- **Data Analysis**: Insights into book categories, employee salaries, member registration trends, and issued books.
- **Summary Reports**: Aggregated data on high-demand books and employee performance.

## Conclusion

This project demonstrates the application of SQL skills in creating and managing a library management system. It includes database setup, data manipulation, and advanced querying, providing a solid foundation for data management and analysis.


## Author - Syed Tanzeel Adnan

This project showcases SQL skills essential for database management and analysis.

Thank you for your interest in this project!
