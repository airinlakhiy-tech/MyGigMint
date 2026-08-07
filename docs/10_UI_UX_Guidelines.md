# MyGigMint – UI/UX Guidelines

**Document Version:** 1.0

**Document Type:** UI/UX Design Guidelines

**Project:** MyGigMint

---

# 1. Purpose

This document defines the visual design language, UI components, layout principles, accessibility standards, and user experience guidelines for the MyGigMint platform.

The objective is to ensure consistency across all interfaces including web, mobile, and admin dashboard.

---

# 2. Design Principles

The UI shall follow these principles:

- Simple
- Clean
- Modern
- Minimal
- Professional
- Responsive
- Accessible
- Fast
- Consistent

---

# 3. Design Philosophy

MyGigMint focuses on:

- Clear navigation
- Minimal distractions
- Fast task completion
- Easy onboarding
- High readability
- Mobile-first experience

---

# 4. Brand Identity

Brand Name

MyGigMint

Brand Personality

- Trustworthy
- Professional
- Friendly
- Modern
- Reliable

---

# 5. Color Palette

## Primary

Mint Green

HEX: #16A085

---

## Secondary

Dark Navy

HEX: #1F2937

---

## Accent

Gold

HEX: #F59E0B

---

## Success

HEX: #22C55E

---

## Warning

HEX: #F59E0B

---

## Danger

HEX: #EF4444

---

## Background

HEX: #F8FAFC

---

## Card

HEX: #FFFFFF

---

## Border

HEX: #E5E7EB

---

# 6. Typography

Primary Font

Inter

Fallback

Arial

Hierarchy

H1

48px

H2

36px

H3

28px

H4

22px

Body

16px

Caption

14px

Small

12px

---

# 7. Spacing System

Base Unit

8px

Spacing Scale

4px

8px

12px

16px

24px

32px

48px

64px

80px

96px

---

# 8. Border Radius

Buttons

8px

Cards

12px

Inputs

8px

Dialogs

16px

Avatars

50%

---

# 9. Shadows

Small

0 2px 8px rgba(0,0,0,.08)

Medium

0 8px 24px rgba(0,0,0,.12)

Large

0 16px 48px rgba(0,0,0,.18)

---

# End of Part 1
---

# Part 2 – UI Components & Navigation

# 10. Buttons

The platform shall provide consistent button styles.

## Primary Button

Purpose:

- Main actions
- Submit
- Save
- Continue

Style

- Background: Primary Color
- Text: White
- Border Radius: 8px

---

## Secondary Button

Purpose

- Cancel
- Back
- Edit

Style

- White Background
- Primary Border
- Primary Text

---

## Danger Button

Purpose

- Delete
- Remove
- Suspend

Color

- Red

---

## Button States

- Default
- Hover
- Active
- Focus
- Disabled
- Loading

---

# 11. Forms

All forms shall follow a consistent layout.

Requirements

- Labels above inputs
- Required field indicator
- Inline validation
- Success messages
- Error messages
- Responsive layout

---

# 12. Input Components

Supported Inputs

- Text Field
- Password
- Email
- Phone Number
- Number
- Date Picker
- Time Picker
- Search
- Select Dropdown
- Multi Select
- Checkbox
- Radio Button
- Toggle Switch
- File Upload
- Text Area

Validation

- Required
- Minimum Length
- Maximum Length
- Pattern Validation
- Server Validation

---

# 13. Cards

Cards shall be used for:

- Jobs
- User Profiles
- Statistics
- Wallet Summary
- Premium Plans
- Notifications

Card Structure

- Header
- Content
- Footer
- Action Buttons

---

# 14. Tables

The system shall provide responsive tables.

Features

- Sorting
- Filtering
- Search
- Pagination
- Export
- Bulk Actions
- Row Selection

---

# 15. Navigation

Navigation shall include:

Top Navigation

- Logo
- Search
- Notifications
- Wallet
- Profile

Sidebar

- Dashboard
- Jobs
- Wallet
- Referrals
- Premium
- Reports
- Settings

Footer

- About
- Privacy Policy
- Terms
- Contact
- Help Center

---

# 16. Icons

Recommended Icon Library

- Lucide Icons
- Heroicons

Icon Sizes

- 16px
- 20px
- 24px
- 32px

---

# 17. Badges

Badge Types

- New
- Premium
- Verified
- Active
- Pending
- Completed
- Rejected

---

# 18. Alerts

Alert Types

- Success
- Warning
- Error
- Information

Each alert shall support:

- Icon
- Title
- Description
- Close Button

---

# 19. Modal Dialogs

Supported Modals

- Confirmation
- Delete Confirmation
- Payment
- Withdrawal
- Login Required
- Premium Upgrade
- Success Message

Modal Features

- Responsive
- Keyboard Accessible
- ESC Close
- Overlay Click (optional)

---

# End of Part 2
---

# Part 3 – Dashboard, Responsive Design & Accessibility

# 20. Dashboard Layout

The dashboard shall provide a clean and organized workspace.

## User Dashboard

Widgets

- Wallet Balance
- Available Jobs
- Active Jobs
- Completed Jobs
- Referral Earnings
- Recent Transactions
- Notifications
- Profile Completion

Layout

- Left Sidebar
- Top Navigation
- Main Content Area
- Right Sidebar (optional)

---

## Employer Dashboard

Widgets

- Total Posted Jobs
- Active Jobs
- Pending Applications
- Approved Applications
- Wallet Balance
- Recent Payments
- Reports

---

## Admin Dashboard

Widgets

- Total Users
- Active Users
- Total Jobs
- Total Revenue
- Pending Withdrawals
- Pending Deposits
- Reports
- Support Tickets
- System Health
- AI Analytics

---

# 21. Job Card Design

Each job card shall display:

- Job Title
- Reward Amount
- Category
- Employer Name
- Available Slots
- Deadline
- Difficulty Level
- Apply Button
- Save Button

Card Actions

- Apply
- Save
- Share
- Report

---

# 22. Wallet UI

Wallet Page shall include:

- Current Balance
- Pending Balance
- Total Earnings
- Withdraw Button
- Deposit Button
- Transaction History
- Filter Options

---

# 23. Authentication Pages

Pages

- Login
- Register
- Forgot Password
- Reset Password
- Verify Email
- Two-Factor Authentication

Requirements

- Minimal Design
- Clear Validation Messages
- Password Strength Indicator
- Social Login Buttons

---

# 24. Responsive Design

Supported Devices

- Desktop (1920px+)
- Laptop (1366px+)
- Tablet (768px+)
- Mobile (320px+)

Responsive Rules

- Flexible Grid
- Responsive Images
- Responsive Typography
- Touch-Friendly Buttons
- Adaptive Navigation

---

# 25. Dark Mode

The platform shall support:

- Light Theme
- Dark Theme
- System Theme

Dark Mode Requirements

- Accessible Contrast
- Consistent Branding
- Smooth Theme Switching

---

# 26. Accessibility

The platform shall comply with WCAG 2.1 AA.

Requirements

- Keyboard Navigation
- Screen Reader Support
- ARIA Labels
- Visible Focus Indicators
- High Contrast Mode
- Accessible Forms

---

# 27. Loading States

Use loading indicators for:

- Page Loading
- API Requests
- Form Submission
- Image Upload
- Payment Processing

Loading Components

- Spinner
- Skeleton Loader
- Progress Bar

---

# 28. Empty States

Provide meaningful empty states for:

- No Jobs Found
- No Notifications
- Empty Wallet History
- No Search Results
- No Support Tickets

Each empty state should include:

- Illustration/Icon
- Helpful Message
- Call-to-Action Button

---

# 29. Error Pages

Provide dedicated pages for:

- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 429 Too Many Requests
- 500 Internal Server Error
- Maintenance Mode

Each page should include:

- Friendly Message
- Error Code
- Navigation Button
- Contact Support Link

---

# 30. Design Tokens

Colors

- Primary
- Secondary
- Success
- Warning
- Danger
- Background
- Surface
- Border

Typography

- Font Family
- Font Size
- Font Weight
- Line Height

Spacing

- 4px
- 8px
- 12px
- 16px
- 24px
- 32px
- 48px
- 64px

Border Radius

- Small
- Medium
- Large
- Full

Elevation

- Low
- Medium
- High

---

# 31. Figma Guidelines

The design system shall include:

- Color Styles
- Text Styles
- Components
- Variants
- Auto Layout
- Design Tokens
- Icons
- Grid System
- Responsive Frames
- Prototype Flow

---

# Conclusion

The MyGigMint UI/UX Design System establishes a consistent, responsive, accessible, and scalable user experience across all products and devices.

---

# End of UI/UX Guidelines
