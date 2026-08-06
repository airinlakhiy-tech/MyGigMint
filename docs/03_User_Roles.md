# MyGigMint – User Roles & Access Control

**Document Version:** 1.0  
**Document Type:** User Roles & Access Control (RBAC)  
**Project:** MyGigMint

---

# 1. Purpose

This document defines every user role available in the MyGigMint platform and specifies the permissions, responsibilities, and access levels associated with each role.

The platform follows a **Role-Based Access Control (RBAC)** model to ensure security, scalability, and maintainability.

---

# 2. User Role Hierarchy

```

Super Admin
│
├── Admin
│ ├── Moderator
│ │
│ ├── Employer
│ └── Worker
│
└── Guest

```

Higher roles inherit permissions from lower roles where applicable.

---

# 3. Guest

A guest is any visitor who has not created an account.

## Permissions

- View homepage
- Browse public jobs
- View pricing
- View FAQ
- Contact support
- Register account
- Login

## Restrictions

- Cannot apply for jobs
- Cannot post jobs
- Cannot access dashboard
- Cannot use wallet
- Cannot submit work

---

# 4. Worker

Workers complete tasks and earn money.

## Dashboard

- View dashboard
- View profile
- Update profile
- Upload profile picture
- Verify email
- Verify phone

---

## Job Permissions

- Browse available jobs
- Apply for jobs
- View assigned jobs
- Submit completed work
- Cancel application (before approval)
- Track job history

---

## Wallet

- View balance
- View transaction history
- Request withdrawal
- Save payment methods
- Download payment receipts

---

## Referral

- Generate referral link
- Invite users
- View referral earnings
- Track referral tree

---

## Notifications

- Receive system notifications
- Receive employer messages
- Receive payment notifications

---

## Restrictions

Workers cannot:

- Approve jobs
- Delete jobs
- Manage users
- Access admin settings

---

# 5. Employer

Employers create jobs and pay workers.

## Dashboard

- Employer dashboard
- Business profile
- Company verification
- Team members (future)

---

## Job Management

- Create jobs
- Edit jobs
- Pause jobs
- Delete draft jobs
- Approve submissions
- Reject submissions
- Extend deadlines
- Duplicate jobs

---

## Budget Management

- Deposit funds
- View spending
- View invoices
- Manage campaigns

---

## Analytics

- Completion rate
- Worker performance
- Campaign reports
- Budget reports

---

## Restrictions

Employers cannot:

- Modify platform settings
- Access admin dashboard
- View other employers' private data

---

# 6. Moderator

Moderators help maintain platform quality.

## Permissions

- Review reported jobs
- Review reported users
- Moderate comments
- Moderate disputes
- Suspend fraudulent content
- Review withdrawals (optional)

---

## Restrictions

Moderators cannot:

- Delete database
- Modify system settings
- Manage billing
- Access server configuration

---

# 7. Admin

Administrators manage the platform.

## User Management

- Create users
- Edit users
- Suspend users
- Ban users
- Verify users
- Reset passwords

---

## Job Management

- Delete jobs
- Approve jobs
- Suspend campaigns
- Review reports

---

## Wallet

- Approve withdrawals
- Reject withdrawals
- View transactions
- Export reports

---

## Referral

- Configure referral rewards
- View referral analytics

---

## Platform Management

- Manage categories
- Manage notifications
- Manage banners
- Manage CMS pages
- Manage FAQs
- Manage announcements

---

## Reports

- Revenue reports
- User reports
- Employer reports
- Financial reports
- Fraud reports

---

# 8. Super Admin

The highest authority within the platform.

## Permissions

Everything available to Admin plus:

- Manage Admin accounts
- Manage Moderator accounts
- Configure platform settings
- Manage payment gateways
- Configure email service
- Configure SMS service
- Configure AI services
- Configure storage
- Configure security policies
- Configure API keys
- Manage backups
- Restore backups
- View audit logs
- Delete platform data (restricted)
- Manage feature flags
- Configure maintenance mode

---

# 9. Permission Matrix

| Permission | Guest | Worker | Employer | Moderator | Admin | Super Admin |
|------------|:-----:|:------:|:--------:|:---------:|:-----:|:-----------:|
| Register | ✅ | | | | | |
| Login | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| View Jobs | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Apply Job | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| Post Job | ❌ | ❌ | ✅ | ❌ | ✅ | ✅ |
| Wallet | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ |
| Withdraw | ❌ | ✅ | ✅ | ❌ | ✅ | ✅ |
| Approve Jobs | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| Manage Users | ❌ | ❌ | ❌ | Limited | ✅ | ✅ |
| Platform Settings | ❌ | ❌ | ❌ | ❌ | Limited | ✅ |
| Manage Admins | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |

---

# 10. Role Upgrade Flow

Guest

↓

Worker

↓

Verified Worker

↓

Premium Worker (Future)

---

Guest

↓

Employer

↓

Verified Employer

↓

Enterprise Employer (Future)

---

Moderator

↓

Admin

↓

Super Admin

---

# 11. Security Rules

Every role must follow these principles:

- Least privilege access
- Secure authentication
- Audit logging
- Rate limiting
- Session expiration
- Two-factor authentication (optional)
- Email verification
- Phone verification (optional)
- Device tracking

---

# 12. Audit Logging

The system records:

- Login history
- Failed login attempts
- Password changes
- Wallet activity
- Withdrawal requests
- Job approvals
- Role changes
- Admin actions

---

# 13. Future Roles

Future platform versions may introduce:

- AI Assistant
- Customer Support Agent
- Financial Reviewer
- Compliance Officer
- Enterprise Manager
- Affiliate Manager

---

# 14. Design Principles

The RBAC system should be:

- Secure
- Flexible
- Scalable
- Easy to maintain
- Auditable
- Extensible

---

# Conclusion

The Role-Based Access Control system ensures that every user interacts only with the features necessary for their responsibilities. This improves platform security, reduces operational risks, and supports future scalability as MyGigMint grows.
