# MyGigMint – API Specification

**Document Version:** 1.0

**Document Type:** REST API Specification

**Project:** MyGigMint

---

# 1. Overview

The MyGigMint platform exposes RESTful APIs for frontend, mobile applications, and third-party integrations.

## Base URL

Production

https://api.mygigmint.com/v1

Development

http://localhost:8000/api/v1

---

# 2. Authentication

Authentication Method

- JWT Token
- Bearer Authentication

Example Header

Authorization: Bearer <access_token>

---

# 3. Authentication APIs

## Register

POST /auth/register

Request

```json
{
  "full_name": "John Doe",
  "email": "john@example.com",
  "password": "********"
}
```

Response

```json
{
  "success": true,
  "message": "Registration successful"
}
```

---

## Login

POST /auth/login

Request

```json
{
  "email": "john@example.com",
  "password": "********"
}
```

Response

```json
{
  "token":"JWT_TOKEN",
  "user":{}
}
```

---

## Logout

POST /auth/logout

---

## Forgot Password

POST /auth/forgot-password

---

## Reset Password

POST /auth/reset-password

---

## Verify Email

POST /auth/verify-email

---

## Refresh Token

POST /auth/refresh-token

---

# 4. User APIs

## Get Profile

GET /user/profile

---

## Update Profile

PUT /user/profile

---

## Upload Profile Photo

POST /user/profile/photo

---

## Change Password

PUT /user/change-password

---

## Notification List

GET /user/notifications

---

## Mark Notification Read

PUT /user/notifications/{id}

---

# HTTP Status Codes

200 OK

201 Created

400 Bad Request

401 Unauthorized

403 Forbidden

404 Not Found

422 Validation Error

500 Internal Server Error

---

# End of Part 1
---

# Part 2 – Job & Marketplace APIs

# 5. Job APIs

## Create Job

POST /jobs

Description:

Create a new micro job.

Authentication:

Required

Request

```json
{
  "title": "Visit Website",
  "category_id": 1,
  "description": "Visit the website and stay for 2 minutes.",
  "reward_amount": 5,
  "available_slots": 100
}
```

Response

```json
{
  "success": true,
  "job_id": 101
}
```

---

## Get All Jobs

GET /jobs

Query Parameters

- page
- limit
- category
- keyword
- min_reward
- max_reward
- status

---

## Get Job Details

GET /jobs/{id}

---

## Update Job

PUT /jobs/{id}

Authentication

Employer Only

---

## Delete Job

DELETE /jobs/{id}

Authentication

Employer Only

---

# 6. Job Application APIs

## Apply for Job

POST /jobs/{id}/apply

Request

```json
{
  "proof": "Screenshot uploaded."
}
```

---

## My Applications

GET /applications

---

## Application Details

GET /applications/{id}

---

## Approve Application

PUT /applications/{id}/approve

Authentication

Employer Only

---

## Reject Application

PUT /applications/{id}/reject

Authentication

Employer Only

---

# 7. Category APIs

## Get Categories

GET /categories

---

## Category Details

GET /categories/{id}

---

## Create Category

POST /categories

Authentication

Admin Only

---

## Update Category

PUT /categories/{id}

---

## Delete Category

DELETE /categories/{id}

---

# 8. Search APIs

## Search Jobs

GET /search/jobs

Parameters

- keyword
- category
- reward
- country
- page

---

## Search Users

GET /search/users

Parameters

- keyword
- role
- country

---

# 9. Favorite APIs

## Add Favorite

POST /favorites

---

## Remove Favorite

DELETE /favorites/{id}

---

## My Favorites

GET /favorites

---

# 10. Review APIs

## Submit Review

POST /reviews

Request

```json
{
  "rating": 5,
  "review": "Excellent employer."
}
```

---

## Get Reviews

GET /reviews

---

## Report Review

POST /reviews/report

---

# API Response Format

Success

```json
{
  "success": true,
  "message": "Request completed successfully.",
  "data": {}
}
```

Validation Error

```json
{
  "success": false,
  "message": "Validation failed.",
  "errors": {}
}
```

---

# End of Part 2
---

# Part 3 – Wallet, Payment & Referral APIs

# 11. Wallet APIs

## Get Wallet Balance

GET /wallet

Authentication

User Required

Response

```json
{
  "success": true,
  "data": {
    "available_balance": 250.00,
    "pending_balance": 40.00,
    "total_earned": 850.00,
    "total_withdrawn": 560.00
  }
}
```

---

## Wallet Transaction History

GET /wallet/transactions

Query Parameters

- page
- limit
- type
- start_date
- end_date

---

## Wallet Summary

GET /wallet/summary

---

# 12. Deposit APIs

## Create Deposit

POST /wallet/deposit

Request

```json
{
  "payment_method": "bkash",
  "amount": 500
}
```

---

## Upload Payment Proof

POST /wallet/deposit/{id}/proof

---

## Deposit History

GET /wallet/deposits

---

# 13. Withdrawal APIs

## Request Withdrawal

POST /wallet/withdraw

Request

```json
{
  "payment_method": "Nagad",
  "account_number": "017XXXXXXXX",
  "amount": 300
}
```

---

## Withdrawal History

GET /wallet/withdrawals

---

## Cancel Withdrawal

DELETE /wallet/withdrawals/{id}

---

# 14. Payment Methods API

## List Payment Methods

GET /payment-methods

---

## Payment Method Details

GET /payment-methods/{id}

---

# 15. Referral APIs

## Get Referral Information

GET /referrals

---

## Referral Statistics

GET /referrals/statistics

---

## Referral Earnings

GET /referrals/earnings

---

## Referral Tree

GET /referrals/tree

---

# 16. Notification APIs

## Get Notifications

GET /notifications

---

## Mark Notification as Read

PUT /notifications/{id}/read

---

## Mark All Notifications as Read

PUT /notifications/read-all

---

## Delete Notification

DELETE /notifications/{id}

---

# 17. Premium Membership APIs

## Premium Plans

GET /premium/plans

---

## Purchase Premium

POST /premium/purchase

Request

```json
{
  "plan_id": 1,
  "payment_method": "wallet"
}
```

---

## My Subscription

GET /premium/subscription

---

## Cancel Subscription

DELETE /premium/subscription

---

# Standard API Response

Success

```json
{
  "success": true,
  "message": "Request completed successfully.",
  "data": {}
}
```

Error

```json
{
  "success": false,
  "message": "An error occurred.",
  "errors": {}
}
```

---

# End of Part 3
