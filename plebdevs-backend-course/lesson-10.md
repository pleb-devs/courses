## Lesson Slides
- [Lesson 10 slides](https://github.com/pleb-devs/plebdevs-course-2/blob/main/lesson-10.pdf)
- [Lesson 10 link to slides](https://docs.google.com/presentation/d/1vzXhmrolSIPXqvSHETIoOBS6o2tfoSvuJXpYcN0AwMk/edit?usp=sharing)

# Lesson 10: Database Development with Knex.js

## Overview

Welcome to Lesson 10! This is where the theoretical knowledge from our previous lessons gets put into practice. We're diving deep into database development with Knex.js, a powerful JavaScript library that will serve as our bridge between JavaScript and SQL.

**What We'll Cover:**
- Understanding Knex.js and its role in database development
- Setting up database configuration for development and production
- Learning migrations - the version control system for databases
- Understanding seeds - populating databases with test data
- Creating our Users and Invoices tables using Knex schema builder
- Running migrations and seeds to build our database
- Viewing and verifying our database structure

**Note:** This lesson represents a major milestone in our Lightning wallet backend! We're moving from mock data to real database integration, setting the foundation for our production-ready application.

## What is Knex.js?

**Knex.js is like a translator between JavaScript and SQL.** It allows you to write database requests in JavaScript, which is easier for many developers to work with, and then automatically translates these commands into proper SQL.

### Breaking Down the Benefits

**JavaScript-First Approach:** Instead of writing raw SQL, you can use JavaScript methods and objects to interact with your database. This means:
- Familiar syntax for JavaScript developers
- Better integration with your Node.js application
- Enhanced readability and maintainability

**Database Agnostic:** Knex supports multiple database systems:
- SQLite (perfect for development)
- PostgreSQL (excellent for production)
- MySQL, MariaDB, and others

**Built-in Tools:** Knex comes with powerful features:
- Schema builder for creating tables
- Migration system for version control
- Seeding system for test data
- Query builder for complex operations

### Knex vs Raw SQL

Here's a comparison to show the difference:

**Raw SQL:**
```sql
CREATE TABLE users (
   id INTEGER PRIMARY KEY AUTOINCREMENT,
   name TEXT NOT NULL
);
```

**Knex.js:**
```javascript
knex.schema.createTable('users', function(table) {
  table.increments('id');
  table.string('name').notNullable();
});
```

The Knex version is more readable and integrates seamlessly with your JavaScript code.

## Why Use Knex.js?

### Unified Query Syntax
Knex provides a consistent syntax across different SQL databases. This means you can:
- Write your schema once
- Switch databases with minimal code changes
- Use the same development patterns for local and production environments

### Schema Builder
Knex's schema builder makes it easy to:
- Create and modify tables
- Define relationships between tables
- Set up constraints and indexes
- Handle complex database structures

### Migration System
Migrations help you:
- Track database changes over time
- Version control your database structure
- Collaborate with team members effectively
- Roll back changes if needed

### Seeding Capabilities
Seeds allow you to:
- Populate databases with test data
- Create consistent development environments
- Test your application with known data sets
- Set up reference data

### Production Features
Knex also provides:
- **Transaction Support:** Ensure data integrity with all-or-nothing operations
- **Connection Pooling:** Manage database connections efficiently
- **Query Optimization:** Built-in performance enhancements

## Setting Up Our Database Configuration

Let's start by installing the necessary packages and setting up our database configuration.

### Installing Required Packages

First, we need to install three essential packages:

```bash
npm i knex sqlite3 pg
```

**Package Breakdown:**
- **knex:** The main library for database operations
- **sqlite3:** SQLite driver for local development
- **pg:** PostgreSQL driver for production deployment

### Creating the Knex Configuration File

Knex requires a configuration file called `knexfile.js` in your project root. This file defines settings for different environments.

**Create `knexfile.js`:**

```javascript
module.exports = {
  development: {
    // SQLite configuration for local development
    client: "sqlite3",
    connection: {
      filename: "./db/dev.sqlite3",
    },
    useNullAsDefault: true,
    migrations: {
      directory: "./db/migrations",
    },
    seeds: {
      directory: "./db/seeds",
    },
  },

  production: {
    // PostgreSQL configuration for production
    client: "pg",
    connection: process.env.DATABASE_URL,
    migrations: {
      directory: "./db/migrations",
    },
    seeds: {
      directory: "./db/seeds",
    },
  },
};
```

#### Configuration Breakdown

**Development Environment:**
- **client:** Specifies SQLite3 as the database
- **connection.filename:** Points to our local database file
- **useNullAsDefault:** Ensures compatibility with PostgreSQL patterns
- **migrations.directory:** Where migration files are stored
- **seeds.directory:** Where seed files are stored

**Production Environment:**
- **client:** Specifies PostgreSQL as the database
- **connection:** Uses environment variable for database URL
- **migrations/seeds:** Same directory structure as development

### Creating the Database Configuration Module

Now we need to create a database configuration module that our application can use.

**Create `db/dbConfig.js`:**

```javascript
const knex = require("knex");
const config = require("../knexfile");

// Determine environment (development or production)
const env = process.env.NODE_ENV || "development";

// Initialize Knex with the appropriate configuration
const db = knex(config[env]);

module.exports = db;
```

This module:
1. Imports the Knex library
2. Imports our configuration
3. Determines the current environment
4. Initializes Knex with the appropriate configuration
5. Exports the database connection for use throughout our application

## Understanding Migrations

**Migrations are like version control for your database structure.** They provide a systematic way to create, modify, and track changes to your database schema over time.

### What Are Migrations?

Think of migrations as a to-do list for your database. Each migration file contains:
- **Up function:** Instructions for applying changes
- **Down function:** Instructions for reversing changes

### Why Use Migrations?

1. **Version Control:** Track every change to your database structure
2. **Team Collaboration:** Everyone can see what changes were made and why
3. **Deployment Safety:** Apply changes systematically across environments
4. **Rollback Capability:** Undo changes if something goes wrong

### How Migrations Work

Each migration file follows this pattern:

```javascript
exports.up = function(knex) {
  // Instructions for making changes
  return knex.schema.createTable('users', function(table) {
    table.increments('id');
    table.string('name');
    table.string('email');
  });
};

exports.down = function(knex) {
  // Instructions for undoing changes
  return knex.schema.dropTable('users');
};
```

### Creating Our First Migration

Let's create a migration for our Users table:

```bash
npx knex migrate:make create_users_table
```

This command:
- Creates a new migration file with a timestamp prefix
- Generates boilerplate up and down functions
- Stores the file in the migrations directory

### Users Table Migration

Open the newly created migration file and add this code:

```javascript
exports.up = function (knex) {
  return knex.schema.createTable("users", function (table) {
    // Primary key that auto-increments
    table.increments("id");

    // Username: unique string, max 128 characters, required
    table.string("username", 128).notNullable().unique();

    // Password: string, max 128 characters, required
    table.string("password", 128).notNullable();

    // Admin key: optional string for admin privileges
    table.string("adminKey").defaultTo(null);
  });
};

exports.down = function (knex) {
  return knex.schema.dropTableIfExists("users");
};
```

#### Schema Breakdown

**table.increments("id"):**
- Creates an auto-incrementing primary key
- Automatically indexed for performance

**table.string("username", 128).notNullable().unique():**
- Creates a string column with maximum 128 characters
- Cannot be null (required field)
- Must be unique across all records

**table.string("adminKey").defaultTo(null):**
- Optional field for admin privileges
- Defaults to null if not provided
- Secret field for accessing protected endpoints

### Running the Migration

Execute the migration with:

```bash
npx knex migrate:latest
```

This command:
- Runs all pending migrations
- Creates the database file if it doesn't exist
- Sets up the table structure according to your schema

## Understanding Seeds

**Seeds are files that populate your database with test data.** They're essential for development and testing because they provide consistent, predictable data to work with.

### What Are Seeds?

Seeds are JavaScript files that:
- Add initial data to your database tables
- Create consistent development environments
- Provide test data for application testing
- Set up reference data (like user roles or categories)

### Why Use Seeds?

1. **Consistent Testing:** Always have the same data to test against
2. **Development Efficiency:** Don't manually create test data every time
3. **Team Collaboration:** Everyone works with the same data set
4. **Automated Setup:** Quickly set up new development environments

### Creating Our First Seed

Create a seed file for users:

```bash
npx knex seed:make 01_users
```

The naming convention `01_users` ensures seeds run in order.

### Users Seed Implementation

Add this code to your users seed file:

```javascript
const bcrypt = require("bcryptjs");

exports.seed = async function (knex) {
  // Delete all existing entries first
  await knex("users").del();
  
  // Insert test users
  await knex("users").insert([
    {
      id: 1,
      username: "Alice",
      password: bcrypt.hashSync("pass1", 14),
      adminKey: "1234",
    },
    {
      id: 2,
      username: "Bob",
      password: bcrypt.hashSync("pass2", 14),
      adminKey: null,
    },
  ]);
};
```

#### Seed Breakdown

**Password Hashing:**
- Uses bcrypt to hash passwords before storing
- The number `14` is the salt rounds (security parameter)
- Never store plain text passwords in any environment

**Test Users:**
- **Alice:** Admin user with adminKey "1234"
- **Bob:** Regular user without admin privileges

### Running Seeds

Execute the seed with:

```bash
npx knex seed:run
```

This command:
- Runs all seed files in order
- Clears existing data first
- Inserts fresh test data

## Creating the Invoices Table

Now let's create our second table for Lightning invoices. This table will store both incoming and outgoing Lightning payments.

### Invoices Migration

Create a new migration:

```bash
npx knex migrate:make create_invoices_table
```

### Invoices Schema Implementation

Add this comprehensive schema to your invoices migration:

```javascript
exports.up = function (knex) {
  return knex.schema.createTable("invoices", function (table) {
    // Primary key
    table.increments("id").primary();

    // Lightning invoice string (unique identifier)
    table.string("payment_request").notNullable().unique();

    // Amount in satoshis
    table.integer("value").notNullable();

    // Optional memo/description
    table.string("memo");

    // Routing fees (null until paid)
    table.integer("fees");

    // Direction: true = outgoing, false = incoming
    table.boolean("send").notNullable();

    // Payment status: true = paid, false = unpaid
    table.boolean("settled").notNullable();

    // When payment was completed (null if unpaid)
    table.timestamp("settle_date").defaultTo(null);

    // When invoice was created
    table.timestamp("created_at").defaultTo(knex.fn.now());

    // Foreign key to users table
    table.integer("user_id").unsigned().notNullable();
    table.foreign("user_id").references("id").inTable("users");
  });
};

exports.down = function (knex) {
  return knex.schema.dropTableIfExists("invoices");
};
```

#### Schema Breakdown

**Lightning-Specific Fields:**
- **payment_request:** The actual Lightning invoice string
- **value:** Amount in satoshis (Lightning's base unit)
- **memo:** Optional description or note
- **fees:** Routing fees (only known after payment)

**State Tracking:**
- **send:** Distinguishes between incoming and outgoing payments
- **settled:** Tracks whether payment has been completed
- **settle_date:** Timestamp of when payment completed

**Relationships:**
- **user_id:** Foreign key linking to the users table
- **foreign():** Ensures referential integrity

### Running the Invoices Migration

Execute the migration:

```bash
npx knex migrate:latest
```

### Creating Invoice Seeds

Create a seed file for invoices:

```bash
npx knex seed:make 02_invoices
```

### Invoice Seed Implementation

Add test invoice data:

```javascript
exports.seed = async function (knex) {
  // Delete all existing entries
  await knex("invoices").del();
  
  // Insert test invoices
  await knex("invoices").insert([
    {
      // Incoming invoice (not yet paid)
      payment_request: "lnbcrt1u1p...",
      memo: "Test incoming payment",
      value: 1000,
      fees: null,
      send: false,
      settled: false,
      settle_date: null,
      user_id: 1, // Alice's invoice
    },
    {
      // Outgoing payment (completed)
      payment_request: "lnbcrt2u1p...",
      memo: "Test outgoing payment",
      value: 1100,
      fees: 10,
      send: true,
      settled: true,
      settle_date: knex.fn.now(),
      user_id: 2, // Bob's payment
    },
  ]);
};
```

#### Seed Data Explanation

**Incoming Invoice (First Entry):**
- Created by Alice to receive payment
- Not yet paid (settled = false)
- No fees yet (fees = null)
- No settle_date yet

**Outgoing Payment (Second Entry):**
- Payment made by Bob
- Completed (settled = true)
- Includes routing fees (fees = 10)
- Has settlement timestamp

### Running Invoice Seeds

Execute the seeds:

```bash
npx knex seed:run
```

## Viewing Our Database

Now let's examine our database to verify everything was created correctly.

### Installing SQLite Browser

For easy database viewing, download SQLite Browser:
- Visit [https://sqlitebrowser.org/dl/](https://sqlitebrowser.org/dl/)
- Download the appropriate version for your operating system
- Install and launch the application

### Opening Our Database

1. Launch SQLite Browser
2. Click "Open Database"
3. Navigate to your project's `db/` directory
4. Select `dev.sqlite3`
5. Click "Open"

### Exploring the Data

**View Users Table:**
1. Click the "Browse Data" tab
2. Select "users" from the table dropdown
3. You should see Alice and Bob with their hashed passwords

**View Invoices Table:**
1. Select "invoices" from the table dropdown
2. You should see both test invoices with all their properties
3. Notice the foreign key relationships (user_id values)

### Verifying Relationships

You can verify the relationship between users and invoices by noting:
- Alice (user_id: 1) has the incoming invoice
- Bob (user_id: 2) has the outgoing payment
- The foreign key constraint ensures data integrity

## Database Development Workflow

Now that we've completed our initial setup, let's understand the typical workflow for database development with Knex.

### Development Process

1. **Plan Changes:** Determine what database changes are needed
2. **Create Migration:** Generate a new migration file
3. **Define Schema:** Write the up and down functions
4. **Test Migration:** Run the migration and verify results
5. **Create Seeds:** Add test data for the new structure
6. **Verify Results:** Use SQLite Browser to confirm everything works

### Best Practices

**Migration Guidelines:**
- Always include both up and down functions
- Test migrations thoroughly before deploying
- Use descriptive migration names
- Keep migrations focused on single changes

**Seed Guidelines:**
- Always clear existing data first
- Use realistic test data
- Include edge cases in your test data
- Keep seeds consistent across team members

**Security Considerations:**
- Never store plain text passwords
- Use proper foreign key constraints
- Validate data types and constraints
- Consider data privacy in seed files

## Lightning Wallet Database Design

Our database design specifically supports Lightning wallet functionality:

### Payment Flow Support

**Incoming Payments:**
- User creates an invoice (settled = false)
- Invoice gets paid by external party
- We update the invoice (settled = true, settle_date = now)

**Outgoing Payments:**
- User initiates payment (send = true)
- Payment routes through Lightning network
- We record the result (settled = true/false, fees)

### Data Integrity

**Foreign Key Constraints:**
- Ensures every invoice belongs to a valid user
- Prevents orphaned invoice records
- Maintains referential integrity

**Unique Constraints:**
- Prevents duplicate usernames
- Prevents duplicate payment_request strings
- Ensures data consistency

### Performance Considerations

**Primary Keys:**
- Auto-incrementing IDs for fast lookups
- Indexed by default for query performance

**Timestamp Tracking:**
- created_at for audit trails
- settle_date for payment history
- Enables time-based queries and reports

## The Database Development Flowchart

Looking back at our complete setup, here's how all the pieces fit together:

```
┌─────────────────────┐
│   Database          │
│   (SQLite/Postgres) │
└─────────────────────┘
           ↑
           │
┌─────────────────────┐
│   Knex.js           │
│   (Query Builder)   │
└─────────────────────┘
           ↑
    ┌──────┴──────┐
    │             │
┌───────────┐ ┌───────────┐
│ Migrations│ │   Seeds   │
│ (Schema)  │ │ (Test Data)│
└───────────┘ └───────────┘
           ↑
┌─────────────────────┐
│   DB Config         │
│   (Connection)      │
└─────────────────────┘
           ↑
┌─────────────────────┐
│   Application       │
│   (Express Routes)  │
└─────────────────────┘
```

This architecture provides:
- **Separation of Concerns:** Each layer has a specific responsibility
- **Environment Flexibility:** Easy switching between development and production
- **Version Control:** Migrations track all schema changes
- **Testing Support:** Seeds provide consistent test data

## Key Takeaways

1. **Knex.js Advantages:** JavaScript-first database development with SQL power
2. **Migration System:** Version control for database structure changes
3. **Seeding Strategy:** Consistent test data for development and testing
4. **Environment Configuration:** Different setups for development and production
5. **Lightning Integration:** Database designed specifically for Lightning wallet functionality
6. **Data Integrity:** Foreign keys and constraints ensure data consistency
7. **Development Workflow:** Systematic approach to database changes

## Common Pitfalls to Avoid

1. **Missing Down Functions:** Always implement rollback functionality
2. **Forgetting Dependencies:** Run migrations in correct order
3. **Seed Data Conflicts:** Always clear existing data before seeding
4. **Configuration Errors:** Double-check database paths and settings
5. **Security Oversights:** Never store plain text passwords
6. **Schema Mismatches:** Ensure seeds match your schema exactly

## What's Next?

In the next lesson, we'll:
- Create database models for clean data access
- Integrate our database with Express routes
- Build API endpoints that interact with our database
- Implement user authentication using our database
- Connect Lightning operations to database storage
- Handle database errors and edge cases

## Practice Exercises

1. **Schema Design Practice:**
   - Design a migration for a "transactions" table
   - Create relationships between users, invoices, and transactions
   - Write seeds for the new table structure

2. **Migration Scenarios:**
   - Practice creating and rolling back migrations
   - Add new columns to existing tables
   - Modify existing column constraints

3. **Seed Data Creation:**
   - Create realistic Lightning invoice data
   - Build seeds for different user scenarios
   - Test edge cases with seed data

4. **Database Exploration:**
   - Use SQLite Browser to write custom queries
   - Explore relationships between tables
   - Verify data integrity constraints

5. **Environment Setup:**
   - Practice switching between development and production configs
   - Set up a PostgreSQL instance for testing
   - Configure environment variables properly

## Resources

### Essential Documentation
- [Knex.js Official Documentation](https://knexjs.org/) - Complete reference guide
- [Knex.js Cheat Sheet](https://devhints.io/knex) - Quick reference for common commands
- [SQLite Browser](https://sqlitebrowser.org/) - Tool for viewing SQLite databases

### Tutorial Resources
- [Node Backend Walkthrough](https://github.com/AustinKelsay/node-backend-walkthrough) - Complete example implementation
- [Learn Knex.js with PostgreSQL](https://www.youtube.com/watch?v=wfrn21E2NaU) - Video tutorial
- [Knex.js Tutorial for Beginners](https://blog.shahednasser.com/knex-js-tutorial-for-beginners/) - Comprehensive written guide

### Database Concepts
- [What are Database Migrations](https://www.prisma.io/dataguide/types/relational/what-are-database-migrations) - Understanding migration patterns
- [Database Seeding with Knex](https://dev.to/cesareferrari/database-seeding-with-knex-51gf) - Seeding best practices
- [SQL Relationships and Foreign Keys](https://www.sqlitetutorial.net/sqlite-foreign-key/) - Database relationship concepts

### Lightning Development Resources
- [Lightning Network Database Patterns](https://docs.lightning.engineering/lightning-network-tools/lnd/database) - How Lightning nodes store data
- [Bitcoin Database Design](https://bitcoin.design/guide/daily-spending-wallet/database-design/) - Bitcoin-specific database considerations
- [Lightning Invoice Format](https://github.com/lightning/bolts/blob/master/11-payment-encoding.md) - Understanding Lightning invoice structure

Remember: Database development is a process, not a single event. Each step builds upon the previous one, and the systematic approach we've learned here will serve you well as your applications grow in complexity. The foundation we've built with Knex.js will make it easy to add new features and maintain your Lightning wallet backend as it evolves.

In our next lesson, we'll bring this database to life by connecting it to our Express server and building the API endpoints that will power our Lightning wallet application! 