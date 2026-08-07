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
---

# Part 2 – Job Management Database

## 4. Job Categories

Stores all job categories.

### Columns

- id
- name
- slug
- description
- icon
- status
- created_at
- updated_at

Indexes

- name
- slug

---

## 5. Jobs

Stores all posted jobs.

### Columns

- id
- employer_id
- category_id
- title
- slug
- description
- instructions
- reward_amount
- available_slots
- completed_slots
- start_date
- end_date
- status
- created_at
- updated_at

Indexes

- employer_id
- category_id
- status

---

## 6. Job Applications

Stores worker applications.

### Columns

- id
- job_id
- user_id
- proof
- status
- submitted_at
- reviewed_at
- created_at
- updated_at

Status

- Pending
- Approved
- Rejected

Indexes

- job_id
- user_id
- status

---

## 7. Job Proofs

Stores uploaded proof for completed jobs.

### Columns

- id
- application_id
- file_url
- proof_text
- reviewed_by
- review_note
- created_at

---

## 8. Skills

Stores platform skills.

### Columns

- id
- name
- slug
- description

---

## 9. User Skills

Many-to-many relationship.

### Columns

- id
- user_id
- skill_id
- experience_level

Experience Level

- Beginner
- Intermediate
- Advanced
- Expert

---

## 10. Favorites

Stores bookmarked jobs.

### Columns

- id
- user_id
- job_id
- created_at

---

## 11. Reviews

Stores employer and worker reviews.

### Columns

- id
- reviewer_id
- target_user_id
- job_id
- rating
- review
- created_at

Rating

- 1
- 2
- 3
- 4
- 5

---

# Relationships

Users
↓
Jobs

Jobs
↓
Applications

Applications
↓
Proofs

Users
↓
Skills

Users
↓
Favorites

Users
↓
Reviews

Jobs
↓
Reviews

---

# End of Part 2
---

# Part 3 – Wallet & Financial Database

## 12. Wallets

Stores each user's wallet.

### Columns

- id
- user_id
- available_balance
- pending_balance
- total_earned
- total_withdrawn
- currency
- status
- created_at
- updated_at

Indexes

- user_id

---

## 13. Wallet Transactions

Stores all wallet transactions.

### Columns

- id
- wallet_id
- user_id
- transaction_type
- amount
- balance_before
- balance_after
- reference_number
- description
- status
- created_at

Transaction Types

- Deposit
- Withdrawal
- Job Reward
- Referral Bonus
- Admin Adjustment
- Refund

Indexes

- wallet_id
- user_id
- transaction_type

---

## 14. Deposits

Stores user deposits.

### Columns

- id
- user_id
- payment_method_id
- amount
- transaction_id
- payment_proof
- status
- verified_by
- verified_at
- created_at

Status

- Pending
- Approved
- Rejected

---

## 15. Withdrawals

Stores withdrawal requests.

### Columns

- id
- user_id
- payment_method_id
- amount
- fee
- payable_amount
- account_number
- transaction_id
- status
- approved_by
- approved_at
- created_at

Indexes

- user_id
- status

---

## 16. Payment Methods

Stores supported payment methods.

### Columns

- id
- name
- type
- minimum_amount
- maximum_amount
- processing_fee
- status

Examples

- bKash
- Nagad
- Rocket
- Bank Transfer
- USDT

---

## 17. Referral System

Stores referral relationships.

### Columns

- id
- referrer_id
- referred_user_id
- referral_level
- referral_bonus
- created_at

---

## 18. Referral Earnings

Stores referral commission history.

### Columns

- id
- referral_id
- earner_id
- amount
- source
- created_at

---

## 19. Bonus History

Stores bonus rewards.

### Columns

- id
- user_id
- bonus_type
- amount
- description
- created_at

Bonus Types

- Daily Bonus
- Weekly Bonus
- Monthly Bonus
- Promotional Bonus
- Achievement Bonus

---

# Relationships

Users
↓
Wallets

Wallets
↓
Wallet Transactions

Users
↓
Deposits

Users
↓
Withdrawals

Users
↓
Referrals

Referrals
↓
Referral Earnings

Users
↓
Bonus History

---

# End of Part 3
---

# Part 4 – Administration & System Database

## 20. Admin Users

Stores administrator accounts.

### Columns

- id
- full_name
- email
- password
- role
- status
- last_login
- created_at
- updated_at

---

## 21. Support Tickets

Stores customer support requests.

### Columns

- id
- user_id
- subject
- category
- priority
- message
- status
- assigned_to
- resolved_at
- created_at
- updated_at

Priority

- Low
- Medium
- High
- Critical

Status

- Open
- In Progress
- Resolved
- Closed

---

## 22. Ticket Replies

Stores replies for support tickets.

### Columns

- id
- ticket_id
- sender_id
- message
- attachment
- created_at

---

## 23. Reports

Stores reports against users or jobs.

### Columns

- id
- reporter_id
- reported_user_id
- job_id
- reason
- description
- status
- reviewed_by
- reviewed_at
- created_at

---

## 24. Notifications

Stores system notifications.

### Columns

- id
- user_id
- title
- message
- notification_type
- is_read
- created_at

---

## 25. Premium Plans

Stores premium membership plans.

### Columns

- id
- name
- price
- duration_days
- job_limit
- featured_profile
- priority_support
- status
- created_at

---

## 26. User Premium Subscriptions

Stores purchased premium plans.

### Columns

- id
- user_id
- premium_plan_id
- start_date
- end_date
- payment_status
- status
- created_at

---

## 27. Coupons

Stores promotional coupons.

### Columns

- id
- code
- discount_type
- discount_value
- usage_limit
- used_count
- expires_at
- status
- created_at

---

## 28. Coupon Usage

Tracks coupon redemption.

### Columns

- id
- coupon_id
- user_id
- used_at

---

## 29. AI Activity Logs

Stores AI-generated activities.

### Columns

- id
- user_id
- feature
- request
- response
- processing_time
- status
- created_at

---

## 30. Audit Logs

Stores security and administrative actions.

### Columns

- id
- user_id
- action
- module
- ip_address
- device
- browser
- created_at

---

## 31. System Settings

Stores platform configuration.

### Columns

- id
- setting_key
- setting_value
- description
- updated_at

Examples

- Site Name
- Currency
- Maintenance Mode
- Registration Enabled
- Minimum Withdrawal
- Referral Bonus
- AI Configuration

---

## Database Relationships Summary

Users
├── Wallet
├── Jobs
├── Job Applications
├── Deposits
├── Withdrawals
├── Referrals
├── Notifications
├── Reports
├── Support Tickets
├── Premium Subscriptions
└── Audit Logs

Jobs
├── Category
├── Applications
├── Reviews
└── Reports

Wallet
├── Transactions
├── Deposits
└── Withdrawals

Support Tickets
└── Ticket Replies

Premium Plans
└── User Premium Subscriptions

Coupons
└── Coupon Usage

---

# Database Indexing Strategy

The following columns should be indexed:

- email
- username
- referral_code
- job_id
- user_id
- wallet_id
- payment_status
- transaction_id
- created_at
- status

Composite indexes should be used where necessary to improve query performance.

---

# Data Retention Policy

- Audit Logs: 2 Years
- Payment Records: 7 Years
- User Activity Logs: 1 Year
- Notifications: 180 Days
- AI Logs: 90 Days
- Support Tickets: 2 Years

---

# Conclusion

The MyGigMint database is designed using enterprise-grade principles with normalization, indexing, security, scalability, and maintainability in mind. The schema supports future expansion while ensuring high performance and data integrity.

---

# End of Database Design
