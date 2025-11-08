# SQL Schema Definition for Property Booking System

This repository contains the SQL schema definition for a property booking system, developed as part of a course assignment. The schema models all necessary entities, their attributes, and the relationships between them, using MySQL syntax.

## Objective

The goal of this assignment was to:

-  Define a normalised relational database schema
-  Implement constraints (e.g. `NOT NULL`, `UNIQUE`, `CHECK`, `FOREIGN KEY`)
-  Ensure data integrity using appropriate data types and indexing
-  Optimise for performance by creating indexes on frequently queried fields

## Entities

The database consists of the following tables:

-  `user`
-  `property`
-  `booking`
-  `payment`
-  `review`
-  `message`

Each table is defined with relevant fields, primary keys (UUIDs stored as `CHAR(36)`), and foreign key relationships to enforce referential integrity.

## Key Features

-  **UUIDs** used as primary keys for scalability and uniqueness
-  **ENUMs** to constrain fields like `role`, `status`, and `payment_method`
-  **CHECK constraint** on review ratings (must be between 1 and 5)
-  **Automatic timestamping** with `DEFAULT CURRENT_TIMESTAMP` and `ON UPDATE CURRENT_TIMESTAMP`
-  **Indexes** on frequently accessed fields (e.g. `email`, `property_id`, `booking_id`) for faster lookups

## Tech Stack

-  **Database**: MySQL 8.x
-  **Tools Used**: MySQL Workbench, SQLTools (VS Code)

## Notes

-  Foreign key constraints enforce cascading relationships (e.g., a `booking` must link to an existing `property` and `user`)
-  The schema was manually written and reviewed to match assignment specifications without relying on ORM auto-generation

---
