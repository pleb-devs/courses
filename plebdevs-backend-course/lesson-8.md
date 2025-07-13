## Lesson Slides
- [Lesson 8 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-8.pdf)
- [Lesson 8 link to slides](https://docs.google.com/presentation/d/1-6XhWPhO65TSTeL9O_O3CZ1f3Pvufs5hj6OGmJvFQJU/edit?usp=sharing)

# Lesson 8: Introduction to Databases

## Overview

Welcome to Lesson 8! This lesson marks a crucial transition in our backend development journey. We're moving from building APIs and connecting to Lightning nodes to understanding how to properly store and manage data. Databases are the backbone of any serious backend application, and this lesson will give you the foundation you need to design and work with them effectively.

**What We'll Cover:**
- Understanding what databases are and why they matter
- Exploring different types of databases and their use cases
- Deep dive into relational databases and SQL
- Understanding database schemas and their importance
- Designing our first database schema for the Pleb Wallet
- Learning about primary keys, foreign keys, and table relationships
- Using tools to visualize and plan database structure

**Note:** This is a foundational lesson that will prepare us for hands-on database development in the next lessons!

## What is a Database?

A database is a collection of data that is organized in a specific way to make it easily accessible and manageable. Think of it as a sophisticated filing system for your application's data.

### Key Characteristics

**Organized Structure:** Data is stored in a structured manner that allows for efficient searching, sorting, and querying. This organization is what makes databases powerful compared to storing data in simple files.

**Accessibility:** Databases provide standardized ways to access data, whether you're searching for a specific user, sorting transactions by date, or querying for complex relationships between data points.

**Scalability:** As your application grows from hundreds to thousands to millions of users, databases are designed to handle this growth efficiently.

### Types of Data

Databases can store many different types of data:
- **Text:** User names, descriptions, messages
- **Numbers:** Amounts, IDs, timestamps
- **Binary Data:** Images, videos, files
- **Structured Data:** JSON objects, arrays

For our Lightning wallet application, we'll primarily work with text and numbers, but understanding the full scope helps you make informed decisions for future projects.

## Types of Databases

Understanding different database types is crucial for choosing the right tool for your project. Each type is optimized for different use cases and comes with its own trade-offs.

### 1. Relational Databases

**Most Common Choice:** When people say "database," they often mean relational databases. These are the industry standard for most applications.

#### How They Work:
- **Tables:** Data is stored in tables with rows and columns
- **Rows:** Each row represents a single record (like one user)
- **Columns:** Each column represents an attribute (like username or email)
- **Relationships:** Tables can be connected through shared keys

#### Key Benefits:
- **Data Integrity:** Strong constraints ensure data consistency
- **ACID Properties:** Atomicity, Consistency, Isolation, Durability
- **Mature Ecosystem:** Decades of development and optimization
- **SQL Standard:** Universal query language

#### Common Examples:
- **MySQL:** Most popular open-source database
- **PostgreSQL:** Advanced features, great for complex queries
- **SQLite:** Lightweight, perfect for development
- **Oracle:** Enterprise-grade, feature-rich

### 2. Document-Oriented Databases

**Flexible Structure:** These databases store data as documents, usually in JSON or XML format.

#### How They Work:
- **Documents:** Each record is a document with nested data
- **Collections:** Groups of similar documents
- **Flexible Schema:** Structure can vary between documents

#### Key Benefits:
- **Rapid Development:** Easy to get started and iterate
- **Flexible Schema:** No need to define structure upfront
- **Natural JSON:** Works seamlessly with JavaScript applications

#### Trade-offs:
- **Scaling Challenges:** Can become problematic at scale
- **Loose Constraints:** Easier to introduce data inconsistencies
- **Query Limitations:** Complex relationships are harder to manage

#### Common Examples:
- **MongoDB:** Most popular document database
- **CouchDB:** Built for distributed systems

### 3. Key-Value Stores

**Simple and Fast:** These databases store data as simple key-value pairs.

#### How They Work:
- **Keys:** Unique identifiers for each piece of data
- **Values:** The actual data, which can be any format
- **High Performance:** Optimized for speed and scale

#### Common Use Cases:
- **Caching:** Storing frequently accessed data
- **Session Storage:** User session information
- **Queue Systems:** Processing background tasks

#### Examples:
- **Redis:** In-memory key-value store
- **Amazon DynamoDB:** Managed NoSQL service

### 4. Graph Databases

**Relationship-Focused:** These databases excel at storing and querying complex relationships between data points.

#### How They Work:
- **Nodes:** Individual data points
- **Edges:** Relationships between nodes
- **Graph Traversal:** Following connections between related data

#### Common Use Cases:
- **Social Networks:** Friend connections and recommendations
- **Fraud Detection:** Identifying suspicious relationship patterns
- **Knowledge Graphs:** Storing interconnected information

#### Examples:
- **Neo4j:** Leading graph database
- **Amazon Neptune:** Managed graph database service

## Deep Dive: Relational Databases

For our Lightning wallet application, we'll use a relational database. Let's understand why and how they work.

### The Relational Model

The relational model is based on mathematical set theory and provides a solid foundation for organizing data:

#### Core Concepts:
- **Relations:** Tables that represent entities (users, invoices, payments)
- **Attributes:** Columns that describe properties of entities
- **Tuples:** Rows that represent individual instances
- **Keys:** Special attributes that uniquely identify or connect records

### Primary and Foreign Keys

Understanding keys is crucial for designing effective databases:

#### Primary Keys:
- **Unique Identifier:** Every table has a primary key that uniquely identifies each row
- **Never Changes:** Primary keys should be stable and never change
- **Usually Integers:** Auto-incrementing integers are common choices
- **Required:** Every table must have a primary key

#### Foreign Keys:
- **References:** Point to primary keys in other tables
- **Relationships:** Create connections between tables
- **Referential Integrity:** Ensure connected data remains consistent
- **Constraints:** Database enforces that foreign keys point to valid records

### Example: Customers and Orders

Let's look at a classic example:

```sql
-- Customers table
CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    first_name TEXT NOT NULL,
    last_name TEXT NOT NULL,
    email TEXT UNIQUE
);

-- Orders table
CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    customer_id INTEGER,
    order_date DATE,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

In this example:
- Each customer has a unique `id` (primary key)
- Each order references a customer through `customer_id` (foreign key)
- The database ensures every order belongs to a valid customer

## What is SQL?

**Structured Query Language (SQL)** is the standard language for working with relational databases. It's been around since the 1970s and is used across virtually all relational database systems.

### Why SQL Matters

#### Universal Language:
- **Standardized:** Works across different database systems
- **Declarative:** You describe what you want, not how to get it
- **Powerful:** Can handle simple queries to complex analytics
- **Transferable:** Skills apply across many database systems

#### Beyond Developers:
- **Business Analysts:** Query data for insights
- **Data Scientists:** Extract and analyze data
- **Database Administrators:** Manage and optimize databases
- **Many Non-Technical Roles:** Libraries, government, research

### Basic SQL Commands

The foundation of SQL consists of four main operations:

#### SELECT - Reading Data
```sql
-- Get all users
SELECT * FROM users;

-- Get specific columns
SELECT username, email FROM users;

-- Filter results
SELECT * FROM users WHERE created_at > '2023-01-01';
```

#### INSERT - Adding Data
```sql
-- Add a new user
INSERT INTO users (username, email, password) 
VALUES ('alice', 'alice@example.com', 'hashed_password');
```

#### UPDATE - Modifying Data
```sql
-- Update a user's email
UPDATE users 
SET email = 'newemail@example.com' 
WHERE username = 'alice';
```

#### DELETE - Removing Data
```sql
-- Remove a user
DELETE FROM users WHERE username = 'alice';
```

### Advanced SQL Features

As you grow more comfortable with SQL, you'll discover powerful features:

- **JOINs:** Combining data from multiple tables
- **Aggregations:** Calculating sums, averages, counts
- **Subqueries:** Nested queries for complex logic
- **Indexes:** Optimizing query performance
- **Transactions:** Ensuring data consistency

## SQL vs NoSQL

Understanding the trade-offs between SQL and NoSQL databases helps you choose the right tool for each project.

### SQL Databases (Relational)

#### Strengths:
- **ACID Compliance:** Guaranteed data consistency
- **Complex Queries:** Powerful JOIN operations
- **Mature Tools:** Decades of tooling and optimization
- **Standardized:** SQL skills transfer between systems
- **Data Integrity:** Strong constraints prevent bad data

#### Best For:
- **Financial Applications:** Where data consistency is critical
- **Complex Relationships:** When data is highly interconnected
- **Reporting Systems:** Complex analytical queries
- **Traditional Business Applications:** ERP, CRM systems

### NoSQL Databases (Non-Relational)

#### Strengths:
- **Flexible Schema:** Easy to change data structure
- **Horizontal Scaling:** Can spread across many servers
- **Rapid Development:** Quick to prototype and iterate
- **JSON-Native:** Natural fit for JavaScript applications

#### Best For:
- **Content Management:** Varying document structures
- **Real-time Applications:** High-speed reads and writes
- **Microservices:** Independent, loosely coupled systems
- **Prototyping:** When requirements are still evolving

### Making the Choice

For our Lightning wallet application, we're choosing SQL because:
- **Data Integrity:** Financial applications need strong consistency
- **Clear Relationships:** Users and invoices have well-defined connections
- **Industry Standard:** SQL skills are valuable in the job market
- **Learning Value:** Understanding SQL provides a solid foundation

## Understanding Database Schemas

A database schema is like a blueprint for your database. It defines the structure, relationships, and constraints that govern how data is organized and accessed.

### What is a Schema?

#### Definition:
A schema describes:
- **Tables:** What entities exist in your system
- **Columns:** What attributes each entity has
- **Data Types:** What kind of data each column stores
- **Constraints:** Rules about valid data
- **Relationships:** How tables connect to each other

#### Why Schemas Matter:
- **Data Consistency:** Ensures all data follows the same rules
- **Performance:** Optimized structure improves query speed
- **Documentation:** Serves as a blueprint for developers
- **Validation:** Prevents invalid data from entering the system

### Schema Design Process

Creating a good schema requires careful planning:

#### 1. Identify Entities
What "things" does your application manage?
- Users
- Invoices
- Payments
- Transactions

#### 2. Define Attributes
What properties does each entity have?
- User: username, password, email, created_at
- Invoice: amount, memo, payment_request, settled

#### 3. Establish Relationships
How do entities connect to each other?
- Users create invoices
- Invoices belong to users
- Payments settle invoices

#### 4. Set Constraints
What rules should the data follow?
- Usernames must be unique
- Amounts must be positive
- Foreign keys must reference valid records

## Designing Our Pleb Wallet Schema

Let's design the database schema for our Lightning wallet application. We'll use a visual tool to help us plan and understand the relationships.

### Our Requirements

Based on our application design, we need to store:
- **Users:** People who use our wallet
- **Invoices:** Both invoices we create and invoices we pay
- **Relationships:** Which user owns which invoices

### Using QuickDB Diagrams

We'll use [QuickDB Diagrams](https://app.quickdatabasediagrams.com/) to visualize our schema. This tool helps us:
- **Visualize Relationships:** See how tables connect
- **Define Constraints:** Specify data rules
- **Generate Code:** Export to actual SQL
- **Collaborate:** Share designs with team members

### Our Final Schema

Here's our complete database schema for the Pleb Wallet:

```sql
Users
-
id PK int
username string UNIQUE
password string
adminKey string default=null

Invoices
-
id PK int
payment_request string UNIQUE
value int
memo string
fees int
send bool
settled bool
settle_date timestamp
created_at timestamp default=GETUTCDATE()
user_id int FK >- Users.id
```

### Users Table Breakdown

#### Field Definitions:
- **id:** Primary key, auto-incrementing integer
- **username:** Unique identifier for login
- **password:** Hashed password (never store plain text!)
- **adminKey:** Optional admin key for privileged operations

### Invoices Table Breakdown

#### Field Definitions:
- **id:** Primary key for the invoice
- **payment_request:** The actual Lightning invoice string (unique)
- **value:** Amount in satoshis
- **memo:** Optional description/message
- **fees:** Routing fees paid (for outgoing payments)
- **send:** Boolean indicating if this is outgoing (true) or incoming (false)
- **settled:** Boolean indicating if the invoice has been paid
- **settle_date:** When the invoice was paid
- **created_at:** When the invoice was created
- **user_id:** Foreign key linking to the user who owns this invoice

### Understanding the Relationships

The relationship between Users and Invoices is **one-to-many**:
- One user can have many invoices
- Each invoice belongs to exactly one user

This is enforced by the foreign key constraint on `user_id` in the Invoices table.

### Schema Visualization

When you input this schema into QuickDB, you'll see:
- **Two connected tables** with a line showing the relationship
- **Key icons** indicating primary keys
- **Relationship arrows** showing foreign key connections

## Key Database Concepts

### Data Types

Understanding data types is crucial for effective schema design:

#### Common Types:
- **INTEGER:** Whole numbers (user IDs, amounts in satoshis)
- **STRING/TEXT:** Text data (usernames, memos)
- **BOOLEAN:** True/false values (settled, send)
- **TIMESTAMP:** Date and time information
- **DECIMAL:** Precise decimal numbers (for fiat amounts)

#### Choosing the Right Type:
- **Storage Efficiency:** Smaller types use less space
- **Query Performance:** Appropriate types enable faster queries
- **Data Integrity:** Types prevent invalid data

### Constraints

Constraints ensure data quality and consistency:

#### Types of Constraints:
- **PRIMARY KEY:** Unique identifier for each row
- **FOREIGN KEY:** References to other tables
- **UNIQUE:** No duplicate values allowed
- **NOT NULL:** Field must have a value
- **CHECK:** Custom validation rules

#### Example Constraints:
```sql
-- Username must be unique and not null
username string UNIQUE NOT NULL

-- Amount must be positive
value int CHECK (value > 0)

-- Foreign key relationship
user_id int REFERENCES users(id)
```

## Lightning-Specific Considerations

### Invoice Management

Our schema needs to handle both:
- **Incoming Invoices:** Created by users to receive payments
- **Outgoing Payments:** Invoices we pay to other Lightning nodes

The `send` field helps us distinguish between these two types.

### Payment States

Lightning payments have several states:
- **Created:** Invoice generated but not yet paid
- **Pending:** Payment in progress
- **Settled:** Payment completed successfully
- **Failed:** Payment attempt failed

Our schema tracks these states using `settled` and `settle_date` fields.

### Fee Tracking

Lightning payments include routing fees. Our schema tracks:
- **value:** The base amount of the invoice
- **fees:** Additional fees paid for routing
- **Total cost:** value + fees (calculated in application)

## Security Considerations

Database security is critical, especially for financial applications:

### Password Security

**Never store plain text passwords:**
```javascript
// WRONG - Never do this
const user = {
  username: 'alice',
  password: 'mypassword123'
};

// CORRECT - Always hash passwords
const bcrypt = require('bcrypt');
const hashedPassword = await bcrypt.hash(password, 10);
```

### Data Validation

**Validate data at multiple levels:**
- **Application Level:** Check data before saving
- **Database Level:** Use constraints and triggers
- **API Level:** Validate incoming requests

### Access Control

**Principle of least privilege:**
- **Database Users:** Create specific users for your application
- **Permissions:** Grant only necessary permissions
- **Network Security:** Restrict database access by IP/network

## Best Practices

### Naming Conventions

Consistent naming makes your database easier to understand:

#### Table Names:
- **Plural:** `users`, `invoices`, `payments`
- **Lowercase:** Avoid mixed case
- **Descriptive:** Clear about what the table contains

#### Column Names:
- **Lowercase with underscores:** `user_id`, `created_at`
- **Descriptive:** `payment_request` not `pr`
- **Consistent:** Use same patterns throughout

### Planning for Growth

Design your schema with future growth in mind:

#### Considerations:
- **Scalability:** Will this work with millions of records?
- **Flexibility:** Can we add new features easily?
- **Performance:** Will queries remain fast as data grows?
- **Maintenance:** Is the schema easy to understand and modify?

## Key Takeaways

1. **Databases are Essential:** Every serious backend application needs persistent data storage
2. **Relational Databases are Standard:** SQL databases are the industry standard for most applications
3. **Schema Design Matters:** Good planning upfront saves time and prevents problems later
4. **Relationships are Powerful:** Foreign keys enable complex data relationships
5. **Constraints Ensure Quality:** Use database constraints to prevent invalid data
6. **Security is Critical:** Never store sensitive data in plain text
7. **Planning Prevents Problems:** Design your schema before writing code

## What's Next?

In the next lesson, we'll:
- Set up a real database for our application
- Learn hands-on SQL commands
- Implement our schema using database migration tools
- Connect our Express server to the database
- Write queries to create, read, update, and delete data

## Practice Exercises

1. **Explore QuickDB:** Create different schema designs for various applications (blog, e-commerce, social media)
2. **Identify Relationships:** Practice identifying one-to-many, many-to-many, and one-to-one relationships
3. **Design Constraints:** Think about what constraints would be appropriate for different types of data
4. **Research Database Types:** Look into when you might choose MongoDB vs PostgreSQL vs Redis
5. **Study Existing Schemas:** Look at open-source projects to see how they design their databases

## Resources

### Essential Reading
- [Database Design Fundamentals](https://www.lucidchart.com/pages/database-diagram/database-design) - Comprehensive guide to database design
- [SQL Tutorial](https://www.w3schools.com/sql/) - Interactive SQL learning
- [Database Normalization](https://www.studytonight.com/dbms/database-normalization.php) - Understanding normal forms

### Video Resources
- [Database Design Course](https://www.youtube.com/watch?v=ztHopE5Wnpc) - Complete database design tutorial
- [SQL Explained](https://www.youtube.com/watch?v=HXV3zeQKqGY) - SQL fundamentals
- [Database Relationships](https://www.youtube.com/watch?v=V5DyvUfsboA) - Understanding table relationships

### Tools and References
- [QuickDB Diagrams](https://app.quickdatabasediagrams.com/) - Visual schema design tool
- [SQLite Browser](https://sqlitebrowser.org/) - Great for exploring databases
- [PostgreSQL Documentation](https://www.postgresql.org/docs/) - Comprehensive database documentation

### Lightning Development Context
- [Database Design for Bitcoin Apps](https://bitcoin.design/guide/daily-spending-wallet/database-design/) - Specific considerations for Bitcoin applications
- [Lightning Network Database Patterns](https://docs.lightning.engineering/lightning-network-tools/lnd/database) - How Lightning nodes store data

Remember: Database design is both an art and a science. Start with solid fundamentals, but don't be afraid to iterate and improve your designs as you learn more about your application's needs. The schema we've designed for our Lightning wallet is intentionally simple, but it provides a solid foundation that we can build upon as we add more features.

In the next lesson, we'll bring this schema to life by setting up a real database and implementing our design with actual SQL commands! 