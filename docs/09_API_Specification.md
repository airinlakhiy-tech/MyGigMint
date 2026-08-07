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
