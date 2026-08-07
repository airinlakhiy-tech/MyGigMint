# MyGigMint – Database Design

**Document Version:** 1.0

**Document Type:** Database Design Specification

**Project:** MyGigMint

---

# 1. Purpose

This document defines the database architecture, table structure, relationships, indexes, constraints, and data integrity rules for the MyGigMint platform.

Primary Database:

- PostgreSQL

ORM:

- Laravel Eloquent ORM

---

# 2. Database Design Principles

The database shall follow:

- Third Normal Form (3NF)
- ACID Compliance
- Foreign Key Constraints
- Proper Indexing
- Soft Deletes
- UUID Support (optional)
- Timestamp Tracking

---

# 3. Core Tables

## Users

Stores all registered users.

Columns

- id
- full_name
- username
- email
- phone
- password
- role_id
- status
- profile_photo
- referral_code
- referred_by
- email_verified_at
- created_at
- updated_at

Indexes

- email
- username
- referral_code

---

## Roles

Stores RBAC roles.

Columns

- id
- name
- description

Examples

- User
- Employer
- Moderator
- Admin
- Super Admin

---

## Permissions

Stores system permissions.

Columns

- id
- permission_name
- module

---

## Role_Permissions

Many-to-many relationship.

Columns

- role_id
- permission_id

---

## User_Sessions

Stores login sessions.

Columns

- id
- user_id
- ip_address
- device
- browser
- login_at
- logout_at

---

## Password_Resets

Stores password reset requests.

Columns

- email
- token
- expires_at

---

## Notifications

Columns

- id
- user_id
- title
- message
- type
- is_read
- created_at

---

## Activity_Logs

Stores all user activities.

Columns

- id
- user_id
- action
- ip
- device
- created_at

---

# Relationships

Users

↓

Roles

↓

Permissions

Users

↓

Notifications

Users

↓

Activity Logs

Users

↓

Sessions

---

# End of Part 1
