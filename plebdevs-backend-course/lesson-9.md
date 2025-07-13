## Lesson Slides
- [Lesson 9 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-9.pdf)
- [Lesson 9 link to slides](https://docs.google.com/presentation/d/1u0ylPhnyvmKhyNICjKEG-MJYoTnr_nvP0sttKIdlmZU/edit?usp=sharing)

# Lesson 9: Introduction to SQL

## Overview

Welcome to Lesson 9! This lesson dives deep into SQL (Structured Query Language), the foundation of database interaction. After understanding database concepts and schema design in the previous lesson, we're now ready to learn the language that powers our data operations.

**What We'll Cover:**
- Understanding what SQL is and why it's essential
- Exploring Relational Database Management Systems (RDBMS)
- Learning fundamental SQL commands for data manipulation
- Understanding SQL joins and table relationships
- Hands-on practice with SQLite Online IDE
- Creating our actual Users and Invoices tables
- Building queries for our Lightning wallet application

**Note:** This lesson provides the SQL foundation you'll need for the next lesson where we'll integrate a real database into our Pleb Wallet backend!

## What is SQL?

**SQL stands for Structured Query Language.** It's essentially a language that allows us to "communicate" with databases. Let's break down what makes SQL special:

### Breaking Down the Acronym

**Structured:** SQL is structured, meaning it has a defined format and syntax. The rules and structure of SQL allow us to describe exactly what we want from a database with precision and clarity.

**Query:** A query is a request for data. When you want to retrieve, insert, update, or delete data from a database, you make a query. Think of it as asking questions to your database and getting answers back.

**Language:** SQL is a language designed for a specific purpose - to interact with relational databases. It's been around since the mid-1970s and has become the universal standard for database operations.

### Why SQL Matters

SQL is probably one of the most widely known programming languages in the world, alongside HTML. Here's why it's so important:

#### Universal Application
Whether your data is stored in:
- A small SQLite database on an IoT device
- A MySQL database powering a web application
- A massive Oracle database running a multinational corporation

SQL provides the means to work with the data consistently across all these environments.

#### Industry Standard
SQL is recognized by the American National Standards Institute (ANSI) and the International Organization for Standardization (ISO). This standardization means:
- Skills transfer between different database systems
- Consistent syntax across platforms
- Long-term career value

#### Core Capabilities
With SQL, you can perform essential database tasks:
- **Retrieving data:** Find specific information that meets your criteria
- **Inserting new data:** Add new records to your tables
- **Updating existing data:** Modify information that's already stored
- **Deleting data:** Remove records that are no longer needed
- **Creating databases and tables:** Build your data structure from scratch
- **Maintaining database structures:** Modify and optimize your database design

## Understanding RDBMS

**RDBMS stands for Relational Database Management System.** This is the foundation that SQL operates on.

### What Makes a Database "Relational"

The "relational" part refers to how data is organized and connected:

#### Table Structure
- **Tables:** Data is stored in tables, much like spreadsheets
- **Rows:** Each row represents a single record (like one user)
- **Columns:** Each column represents a data field (like username or email)
- **Relationships:** Tables connect to each other through shared keys

#### Mental Model: Multiple Spreadsheets
Imagine you're building a Python program that needs to work with multiple Google Sheets:
- One spreadsheet for customers
- Another spreadsheet for orders
- You need to connect them together with certainty

You'd quickly realize you need:
- Unique identifiers (primary keys)
- Ways to link spreadsheets (foreign keys)
- Rules about data consistency (constraints)

This is exactly what RDBMS provides, but with much more sophistication and reliability.

### Popular RDBMS Options

The most common relational database management systems include:

#### The Big Three for Developers
- **PostgreSQL:** Advanced features, great for complex applications, popular in production
- **MySQL:** Most popular open-source database, widely supported
- **SQLite:** Lightweight, perfect for development and learning

#### Enterprise Options
- **Oracle:** Enterprise-grade with advanced features
- **SQL Server:** Microsoft's database solution

### SQL Dialects

While SQL is standardized, each RDBMS has its own "dialect" with slight variations:

#### Common Differences
- **Functions:** Each system has unique built-in functions
- **Data Types:** Slightly different ways to handle data
- **Syntax Extensions:** Proprietary features and optimizations

#### For Beginners
Don't worry about these differences initially. Focus on learning standard SQL - the skills transfer between systems, and you can learn the nuances as needed.

## Fundamental SQL Commands

Let's explore the essential SQL commands you'll use daily. For each command, we'll provide examples and link to additional practice resources.

### SELECT - Reading Data

**The SELECT command is the most important SQL command.** It's used to retrieve data from your database.

#### Basic Syntax
```sql
SELECT column1, column2 FROM table_name;
SELECT * FROM table_name;  -- Select everything
```

#### Examples
```sql
-- Get all users
SELECT * FROM users;

-- Get specific columns
SELECT username, email FROM users;

-- Filter results with WHERE
SELECT * FROM users WHERE created_at > '2023-01-01';
```

#### Key Concepts
- **Result Set:** The data returned is stored in a result table
- **Wildcard (*):** Selects all columns
- **Column Selection:** Choose specific columns for better performance

**Practice Resource:** [W3Schools SELECT Tutorial](https://www.w3schools.com/sql/sql_select.asp)

### INSERT - Adding Data

**The INSERT INTO statement adds new rows to your tables.**

#### Basic Syntax
```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

#### Examples
```sql
-- Add a new user
INSERT INTO users (username, email, password)
VALUES ('alice', 'alice@example.com', 'hashed_password');

-- Insert multiple rows at once
INSERT INTO users (username, email, password)
VALUES 
  ('bob', 'bob@example.com', 'hashed_password1'),
  ('carol', 'carol@example.com', 'hashed_password2');
```

**Practice Resource:** [W3Schools INSERT Tutorial](https://www.w3schools.com/sql/sql_insert.asp)

### UPDATE - Modifying Data

**The UPDATE statement modifies existing records in a table.**

#### Basic Syntax
```sql
UPDATE table_name 
SET column1 = value1, column2 = value2
WHERE condition;
```

#### Examples
```sql
-- Update a user's email
UPDATE users 
SET email = 'newemail@example.com' 
WHERE username = 'alice';

-- Update multiple columns
UPDATE users 
SET email = 'alice@newdomain.com', last_login = '2023-12-01'
WHERE id = 1;
```

#### Important Note
Always use a WHERE clause with UPDATE! Without it, you'll update ALL rows in the table.

**Practice Resource:** [W3Schools UPDATE Tutorial](https://www.w3schools.com/sql/sql_update.asp)

### DELETE - Removing Data

**The DELETE statement removes existing records from a table.**

#### Basic Syntax
```sql
DELETE FROM table_name WHERE condition;
```

#### Examples
```sql
-- Remove a specific user
DELETE FROM users WHERE username = 'alice';

-- Remove users created before a certain date
DELETE FROM users WHERE created_at < '2023-01-01';
```

#### Security Warning
Like UPDATE, always use a WHERE clause! Without it, you'll delete ALL rows.

**Practice Resource:** [W3Schools DELETE Tutorial](https://www.w3schools.com/sql/sql_delete.asp)

### WHERE - Filtering Data

**The WHERE clause is used to filter records based on specific conditions.**

#### Basic Syntax
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```

#### Examples
```sql
-- Simple equality
SELECT * FROM users WHERE country = 'USA';

-- Numeric comparison
SELECT * FROM invoices WHERE amount > 1000;

-- Multiple conditions
SELECT * FROM users 
WHERE country = 'USA' AND age >= 18;

-- Pattern matching
SELECT * FROM users 
WHERE username LIKE 'admin%';
```

#### Common Operators
- **=** Equal to
- **>** Greater than
- **<** Less than
- **>=** Greater than or equal
- **<=** Less than or equal
- **<>** or **!=** Not equal
- **LIKE** Pattern matching
- **IN** Match any value in a list
- **BETWEEN** Range of values

**Practice Resource:** [W3Schools WHERE Tutorial](https://www.w3schools.com/sql/sql_where.asp)

### CREATE - Building Structure

**The CREATE command builds databases and tables.**

#### Creating a Database
```sql
CREATE DATABASE my_database;
```

#### Creating a Table
```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,
    email TEXT NOT NULL,
    password TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Data Types
Common data types you'll use:
- **INTEGER:** Whole numbers
- **TEXT:** String data
- **BOOLEAN:** True/false values
- **TIMESTAMP:** Date and time
- **DECIMAL:** Precise decimal numbers

### ALTER - Modifying Structure

**The ALTER command modifies existing table structures.**

#### Adding a Column
```sql
ALTER TABLE users 
ADD COLUMN lightning_address TEXT;
```

#### Modifying a Column
```sql
ALTER TABLE users 
MODIFY COLUMN email VARCHAR(255);
```

#### Example Use Case
If you update your Lightning wallet to support Lightning addresses, you might add:
```sql
ALTER TABLE users 
ADD COLUMN lightning_address TEXT DEFAULT NULL;
```

### DROP - Removing Structure

**The DROP command permanently deletes databases or tables.**

#### Dropping a Table
```sql
DROP TABLE table_name;
```

#### Dropping a Database
```sql
DROP DATABASE database_name;
```

#### ⚠️ Security Warning
The DROP command is permanent! This is the source of the famous "Little Bobby Tables" attack where malicious users try to inject `DROP TABLE` commands through input fields.

**Never allow user input to directly execute DROP commands!**

## SQL Joins: Connecting Tables

Joins are where SQL gets sophisticated. They allow you to combine data from multiple tables based on relationships between them.

### Why Joins Matter

Remember our mental model of multiple spreadsheets? Joins are how you combine those spreadsheets intelligently. You need different strategies depending on what data you want to see.

### Types of Joins

There are four main types of joins, each serving different purposes:

#### 1. INNER JOIN (Simple Join)
**Returns only records that have matching values in both tables.**

```sql
SELECT customers.customer_name, orders.product
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id;
```

**Use Case:** "Show me customers and their orders, but only for customers who have actually placed orders."

#### 2. LEFT JOIN (Left Outer Join)
**Returns all records from the left table, and matched records from the right table.**

```sql
SELECT customers.customer_name, orders.product
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id;
```

**Use Case:** "Show me all customers, including those who haven't placed any orders yet."

#### 3. RIGHT JOIN (Right Outer Join)
**Returns all records from the right table, and matched records from the left table.**

```sql
SELECT customers.customer_name, orders.product
FROM customers
RIGHT JOIN orders ON customers.customer_id = orders.customer_id;
```

**Use Case:** "Show me all orders, even if the customer data is missing."

#### 4. FULL OUTER JOIN
**Returns all records when there's a match in either table.**

```sql
SELECT customers.customer_name, orders.product
FROM customers
FULL OUTER JOIN orders ON customers.customer_id = orders.customer_id;
```

**Use Case:** "Show me everything - all customers and all orders, whether they match or not."

### Practical Example

Let's imagine we have two tables:

**Customers Table:**
| customer_id | customer_name |
|-------------|---------------|
| 1           | John          |
| 2           | Jane          |
| 3           | Alice         |

**Orders Table:**
| order_id | product | customer_id |
|----------|---------|-------------|
| 1        | Apples  | 1           |
| 2        | Bananas | 2           |
| 3        | Grapes  | 2           |
| 4        | Oranges | 4           |

Notice that:
- Alice (customer_id 3) has no orders
- There's an order for customer_id 4, but no customer with that ID

#### INNER JOIN Result
```sql
SELECT customers.customer_name, orders.product
FROM customers
INNER JOIN orders ON customers.customer_id = orders.customer_id;
```

| customer_name | product |
|---------------|---------|
| John          | Apples  |
| Jane          | Bananas |
| Jane          | Grapes  |

Only customers with orders appear.

#### LEFT JOIN Result
```sql
SELECT customers.customer_name, orders.product
FROM customers
LEFT JOIN orders ON customers.customer_id = orders.customer_id;
```

| customer_name | product |
|---------------|---------|
| John          | Apples  |
| Jane          | Bananas |
| Jane          | Grapes  |
| Alice         | NULL    |

All customers appear, even Alice with no orders.

#### RIGHT JOIN Result
```sql
SELECT customers.customer_name, orders.product
FROM customers
RIGHT JOIN orders ON customers.customer_id = orders.customer_id;
```

| customer_name | product |
|---------------|---------|
| John          | Apples  |
| Jane          | Bananas |
| Jane          | Grapes  |
| NULL          | Oranges |

All orders appear, even the orphaned "Oranges" order.

## Building Our Pleb Wallet Database

Now let's apply what we've learned to create the actual database tables for our Lightning wallet application.

### Our Database Schema

Based on our design from Lesson 8, we need two tables:

#### Users Table
Stores information about wallet users:
- **id:** Primary key
- **username:** Unique identifier for login
- **password:** Hashed password (never plain text!)
- **adminKey:** Optional admin privileges

#### Invoices Table
Stores Lightning invoices (both incoming and outgoing):
- **id:** Primary key
- **payment_request:** The actual Lightning invoice string
- **value:** Amount in satoshis
- **memo:** Optional description
- **fees:** Routing fees paid
- **send:** Boolean (true = outgoing, false = incoming)
- **settled:** Boolean (true = paid, false = unpaid)
- **settle_date:** When the invoice was paid
- **created_at:** When the invoice was created
- **user_id:** Foreign key linking to Users table

### Creating the Users Table

```sql
CREATE TABLE Users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,
    adminKey TEXT DEFAULT NULL
);
```

#### Breakdown:
- **id INTEGER PRIMARY KEY:** Auto-incrementing unique identifier
- **username TEXT UNIQUE NOT NULL:** Required unique username
- **password TEXT NOT NULL:** Required password field
- **adminKey TEXT DEFAULT NULL:** Optional admin key

### Creating the Invoices Table

```sql
CREATE TABLE Invoices (
    id INTEGER PRIMARY KEY,
    payment_request TEXT UNIQUE NOT NULL,
    value INTEGER NOT NULL,
    memo TEXT,
    fees INTEGER NOT NULL,
    send BOOLEAN NOT NULL,
    settled BOOLEAN NOT NULL,
    settle_date DATETIME,
    created_at DATETIME DEFAULT (datetime('now')),
    user_id INTEGER NOT NULL,
    FOREIGN KEY(user_id) REFERENCES Users(id)
);
```

#### Breakdown:
- **payment_request TEXT UNIQUE NOT NULL:** Lightning invoice string (unique)
- **value INTEGER NOT NULL:** Amount in satoshis
- **memo TEXT:** Optional message (can be NULL)
- **fees INTEGER NOT NULL:** Routing fees (can be 0)
- **send BOOLEAN NOT NULL:** True for outgoing, false for incoming
- **settled BOOLEAN NOT NULL:** Payment status
- **settle_date DATETIME:** When payment completed (NULL if unsettled)
- **created_at DATETIME DEFAULT (datetime('now')):** Auto-timestamp
- **user_id INTEGER NOT NULL:** Foreign key to Users table
- **FOREIGN KEY(user_id) REFERENCES Users(id):** Enforces relationship

### Understanding the Relationship

The relationship between Users and Invoices is **one-to-many**:
- One user can have many invoices
- Each invoice belongs to exactly one user
- The foreign key constraint ensures data integrity

## Hands-On Practice with SQLite Online

Let's put our knowledge into practice using a browser-based SQL environment.

### Setting Up SQLite Online

1. Visit [SQLite Online IDE](https://sqliteonline.com/)
2. This tool lets you practice SQL without installing anything
3. You can create, modify, and query databases directly in your browser

### Practice Exercise: Building Our Schema

Follow these steps to create your Pleb Wallet database:

#### Step 1: Create the Users Table
```sql
CREATE TABLE Users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,
    adminKey TEXT DEFAULT NULL
);
```

#### Step 2: Insert a Test User
```sql
INSERT INTO Users (username, password)
VALUES ('example_user', 'hashed_password_123');
```

#### Step 3: Query the Users Table
```sql
SELECT * FROM Users;
```

#### Step 4: Create the Invoices Table
```sql
CREATE TABLE Invoices (
    id INTEGER PRIMARY KEY,
    payment_request TEXT UNIQUE NOT NULL,
    value INTEGER NOT NULL,
    memo TEXT,
    fees INTEGER NOT NULL,
    send BOOLEAN NOT NULL,
    settled BOOLEAN NOT NULL,
    settle_date DATETIME,
    created_at DATETIME DEFAULT (datetime('now')),
    user_id INTEGER NOT NULL,
    FOREIGN KEY(user_id) REFERENCES Users(id)
);
```

#### Step 5: Insert a Test Invoice
```sql
INSERT INTO Invoices (payment_request, value, memo, fees, send, settled, settle_date, user_id)
VALUES ('lnbc1000n1...', 1000, 'Payment for services', 2, 1, 1, '2023-12-01 10:30:00', 1);
```

#### Step 6: Query with a Join
```sql
SELECT Users.username, Invoices.value, Invoices.memo
FROM Users
INNER JOIN Invoices ON Users.id = Invoices.user_id;
```

### Advanced Practice Exercises

Try these additional exercises to solidify your understanding:

#### Exercise 1: User Management
```sql
-- Create multiple users
INSERT INTO Users (username, password) VALUES 
('alice', 'pass123'),
('bob', 'pass456'),
('charlie', 'pass789');

-- Find all users
SELECT * FROM Users;

-- Update a user's password
UPDATE Users SET password = 'new_password' WHERE username = 'alice';

-- Find users with admin keys
SELECT * FROM Users WHERE adminKey IS NOT NULL;
```

#### Exercise 2: Invoice Queries
```sql
-- Find all settled invoices
SELECT * FROM Invoices WHERE settled = 1;

-- Find invoices over 500 sats
SELECT * FROM Invoices WHERE value > 500;

-- Find outgoing payments
SELECT * FROM Invoices WHERE send = 1;

-- Calculate total fees paid
SELECT SUM(fees) as total_fees FROM Invoices WHERE send = 1;
```

#### Exercise 3: Complex Joins
```sql
-- Show all users and their invoice count
SELECT Users.username, COUNT(Invoices.id) as invoice_count
FROM Users
LEFT JOIN Invoices ON Users.id = Invoices.user_id
GROUP BY Users.id;

-- Show users who have sent payments
SELECT DISTINCT Users.username
FROM Users
INNER JOIN Invoices ON Users.id = Invoices.user_id
WHERE Invoices.send = 1;
```

## Lightning-Specific SQL Considerations

Our Lightning wallet has some unique requirements that affect our SQL design:

### Payment States
Lightning payments have multiple states:
- **Created:** Invoice generated but not paid
- **Pending:** Payment in progress
- **Settled:** Payment completed
- **Failed:** Payment failed

Our schema tracks these with the `settled` boolean and `settle_date` timestamp.

### Invoice Types
We handle both:
- **Incoming Invoices:** Created by users to receive payments
- **Outgoing Payments:** Invoices we pay to other Lightning nodes

The `send` boolean distinguishes between these types.

### Fee Tracking
Lightning payments include routing fees:
- **value:** Base amount of the invoice
- **fees:** Additional routing fees
- **Total cost:** value + fees (calculated in application)

### Security Considerations

#### Password Security
```sql
-- WRONG - Never store plain text passwords
INSERT INTO Users (username, password) VALUES ('user', 'plaintext123');

-- CORRECT - Always hash passwords in your application
-- The hashed password gets stored, not the plain text
INSERT INTO Users (username, password) VALUES ('user', '$2b$10$...');
```

#### SQL Injection Prevention
Never directly concatenate user input into SQL queries:
```sql
-- DANGEROUS - Don't do this
query = "SELECT * FROM Users WHERE username = '" + userInput + "'";

-- SAFE - Use parameterized queries
query = "SELECT * FROM Users WHERE username = ?";
```

## Key Takeaways

1. **SQL is Universal:** Learning SQL provides a foundation that works across many database systems
2. **Start with Basics:** Master SELECT, INSERT, UPDATE, DELETE, and WHERE before moving to advanced topics
3. **Joins Are Powerful:** Understanding joins lets you work with related data across multiple tables
4. **Practice is Essential:** Use tools like SQLite Online to practice and experiment
5. **Security First:** Never store passwords in plain text, always prevent SQL injection
6. **Schema Design Matters:** Good table design makes queries easier and more efficient
7. **Lightning Context:** Consider the specific needs of Lightning applications in your database design

## Common Pitfalls to Avoid

1. **Forgetting WHERE clauses:** Always double-check UPDATE and DELETE statements
2. **Not using constraints:** PRIMARY KEY, FOREIGN KEY, and UNIQUE constraints prevent data corruption
3. **Ignoring data types:** Choose appropriate data types for better performance and storage
4. **Complex joins too early:** Master simple queries before attempting complex multi-table joins
5. **Security oversights:** Always validate input and use parameterized queries

## What's Next?

In the next lesson, we'll:
- Set up a real database for our Pleb Wallet application
- Learn about database migration tools (specifically Knex.js)
- Connect our Express server to the database
- Implement our Users and Invoices tables in code
- Create API endpoints that interact with our database
- Handle database connections and error management

## Practice Exercises

1. **SQLite Online Exploration:** 
   - Create different database schemas (blog, e-commerce, social media)
   - Practice all four types of joins with sample data
   - Write queries with multiple WHERE conditions

2. **Lightning Wallet Queries:**
   - Design queries to find all unpaid invoices
   - Calculate total fees paid by each user
   - Find the most recent transactions for each user

3. **Schema Design:**
   - Design a database for a Lightning-powered marketplace
   - Consider what tables and relationships you'd need
   - Write the CREATE TABLE statements

4. **Join Practice:**
   - Create sample data with some mismatched foreign keys
   - Practice each type of join to see the different results
   - Write queries that combine data from multiple tables

5. **Security Research:**
   - Research SQL injection attacks and prevention
   - Learn about database user permissions
   - Study password hashing best practices

## Resources

### Essential Learning
- [SQL Cheat Sheet](https://cheatography.com/fetttobse/cheat-sheets/sqlite/) - Visual reference for SQL syntax
- [W3Schools SQL Tutorial](https://www.w3schools.com/sql/) - Interactive SQL learning with exercises
- [SQLite Online IDE](https://sqliteonline.com/) - Browser-based SQL practice environment

### Video Resources
- [Learn SQL in 15 Minutes](https://www.youtube.com/watch?v=kbKty5ZVKMY) - Quick SQL fundamentals
- [SQL Joins Explained](https://www.youtube.com/watch?v=9yeOJ0ZMUYw) - Visual explanation of joins
- [Database Design Course](https://www.youtube.com/watch?v=ztHopE5Wnpc) - Complete database design tutorial

### Articles and References
- [SQL Joins Explained Visually](https://dataschool.com/how-to-teach-people-sql/sql-join-types-explained-visually/) - Great visual guide to joins
- [SQLite vs MySQL vs PostgreSQL](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems) - Database comparison
- [SQL Injection Prevention](https://owasp.org/www-community/attacks/SQL_Injection) - Security best practices

### Lightning Development Context
- [Database Design for Bitcoin Apps](https://bitcoin.design/guide/daily-spending-wallet/database-design/) - Bitcoin-specific database considerations
- [Lightning Network Database Patterns](https://docs.lightning.engineering/lightning-network-tools/lnd/database) - How Lightning nodes store data
- [Bolt 11 Invoice Format](https://github.com/lightning/bolts/blob/master/11-payment-encoding.md) - Understanding Lightning invoice structure

Remember: SQL is both powerful and potentially dangerous. Start with simple queries, always test in a safe environment, and never run queries you don't understand on production data. The fundamentals you learn here will serve you throughout your development career, as SQL skills are transferable across many technologies and industries.

In the next lesson, we'll bring these SQL concepts to life by integrating a real database into our Pleb Wallet backend! 