# Database Requirements Specification

This document describes the entity structure and relationships for the Booking System database.

---

## 🧍‍♂️ USER

**Table:** `users`

| Column | Type | Constraints | Description |
|--------|------|-------------|--------------|
| `user_id` | UUID | **Primary Key**, Indexed | Unique identifier for each user |
| `first_name` | VARCHAR | **NOT NULL** | User's first name |
| `last_name` | VARCHAR | **NOT NULL** | User's last name |
| `email` | VARCHAR | **UNIQUE, NOT NULL** | User email (used for login) |
| `password_hash` | VARCHAR | **NOT NULL** | Secure hashed password |
| `phone_number` | VARCHAR | NULL | Optional contact number |
| `role` | ENUM('guest', 'host', 'admin') | **NOT NULL** | Defines user type |
| `created_at` | TIMESTAMP | **DEFAULT CURRENT_TIMESTAMP** | Record creation time |

---

## 🏠 PROPERTY

**Table:** `property`

| Column | Type | Constraints | Description |
|--------|------|-------------|--------------|
| `property_id` | UUID | **Primary Key**, Indexed | Unique property identifier |
| `host_id` | UUID | **Foreign Key** → `users(user_id)` | The host who owns the property |
| `name` | VARCHAR | **NOT NULL** | Property name |
| `description` | TEXT | **NOT NULL** | Description of the property |
| `location` | VARCHAR | **NOT NULL** | Property address or area |
| `pricepernight` | DECIMAL | **NOT NULL** | Price per night |
| `created_at` | TIMESTAMP | **DEFAULT CURRENT_TIMESTAMP** | Record creation time |
| `updated_at` | TIMESTAMP | **ON UPDATE CURRENT_TIMESTAMP** | Last update time |

---

## 📅 BOOKING

**Table:** `booking`

| Column | Type | Constraints | Description |
|--------|------|-------------|--------------|
| `booking_id` | UUID | **Primary Key**, Indexed | Unique booking identifier |
| `property_id` | UUID | **Foreign Key** → `property(property_id)` | The property being booked |
| `user_id` | UUID | **Foreign Key** → `users(user_id)` | The guest making the booking |
| `start_date` | DATE | **NOT NULL** | Check-in date |
| `end_date` | DATE | **NOT NULL** | Check-out date |
| `total_price` | DECIMAL | **NOT NULL** | Total cost of the booking |
| `status` | ENUM('pending', 'confirmed', 'canceled') | **NOT NULL** | Booking state |
| `created_at` | TIMESTAMP | **DEFAULT CURRENT_TIMESTAMP** | Booking creation timestamp |

---

## 💳 PAYMENT

**Table:** `payment`

| Column | Type | Constraints | Description |
|--------|------|-------------|--------------|
| `payment_id` | UUID | **Primary Key**, Indexed | Unique payment identifier |
| `booking_id` | UUID | **Foreign Key** → `booking(booking_id)` | Associated booking |
| `amount` | DECIMAL | **NOT NULL** | Payment amount |
| `payment_date` | TIMESTAMP | **DEFAULT CURRENT_TIMESTAMP** | Date/time of payment |
| `payment_method` | ENUM('credit_card', 'paypal', 'stripe') | **NOT NULL** | Payment method used |

---

## ⭐ REVIEW

**Table:** `review`

| Column | Type | Constraints | Description |
|--------|------|-------------|--------------|
| `review_id` | UUID | **Primary Key**, Indexed | Unique review identifier |
| `property_id` | UUID | **Foreign Key** → `property(property_id)` | Reviewed property |
| `user_id` | UUID | **Foreign Key** → `users(user_id)` | Reviewer (guest) |
| `rating` | INTEGER | **CHECK (rating ≥ 1 AND rating ≤ 5), NOT NULL** | Review score |
| `comment` | TEXT | **NOT NULL** | User comment |
| `created_at` | TIMESTAMP | **DEFAULT CURRENT_TIMESTAMP** | Review creation time |

---

## 💬 MESSAGE

**Table:** `message`

| Column | Type | Constraints | Description |
|--------|------|-------------|--------------|
| `message_id` | UUID | **Primary Key**, Indexed | Unique message identifier |
| `sender_id` | UUID | **Foreign Key** → `users(user_id)` | Message sender |
| `recipient_id` | UUID | **Foreign Key** → `users(user_id)` | Message recipient |
| `message_body` | TEXT | **NOT NULL** | Content of the message |
| `sent_at` | TIMESTAMP | **DEFAULT CURRENT_TIMESTAMP** | When the message was sent |

---

## 🔗 Relationships Summary

| Relationship | Type | Description |
|---------------|------|--------------|
| `User (host)` → `Property` | **1 : N** | A host can list many properties |
| `User (guest)` → `Booking` | **1 : N** | A guest can make multiple bookings |
| `Property` → `Booking` | **1 : N** | A property can have multiple bookings |
| `Booking` → `Payment` | **1 : 1** | Each booking has one payment |
| `User` → `Review` | **1 : N** | A user can post multiple reviews |
| `Property` → `Review` | **1 : N** | A property can receive many reviews |
| `User` → `Message` | **1 : N** | A user can send or receive multiple messages |

---

### ✅ Notes
- All UUIDs should be generated using `gen_random_uuid()` (`pgcrypto` extension).
- ENUM types are recommended for PostgreSQL.
- Use **foreign keys with cascading deletes or updates** as appropriate.
- All timestamps default to `CURRENT_TIMESTAMP`.
