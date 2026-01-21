---
title: Master Concept AR Automation Platform - Complete UX Specification
type: project
status: processing
date: 2026-01-21
read: true
tags:
  - project
  - UI-UX
  - architecture
  - design
  - automation
  - fintech
  - deep-dive
  - technical
  - actionable
---

# Master Concept AR Automation Platform
## Complete UX Specification

**Version:** 1.0
**Date:** January 2026
**Client:** Master Concept (Daniel, Amy)
**Document Purpose:** UX mockup development guide

---

# Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [User Personas](#2-user-personas)
3. [System Overview](#3-system-overview)
4. [Information Architecture](#4-information-architecture)
5. [Module 1: Cash Application](#5-module-1-cash-application)
6. [Module 2: AP Portal Bot](#6-module-2-ap-portal-bot)
7. [Shared Components](#7-shared-components)
8. [User Flows](#8-user-flows)
9. [Wireframes](#9-wireframes)
10. [Design Guidelines](#10-design-guidelines)
11. [Responsive Considerations](#11-responsive-considerations)
12. [Appendix](#12-appendix)

---

# 1. Executive Summary

## 1.1 Project Background

Master Concept is a Google Premier Partner with operations across Hong Kong, Singapore, Malaysia, Taiwan, and China. They need to automate two critical AR processes:

1. **Cash Application** - Matching incoming bank payments to open invoices in NetSuite
2. **AP Portal Submission** - Submitting invoices to customer procurement portals (Coupa/Ariba) and tracking payment status

## 1.2 Business Goals

| Goal | Metric |
|------|--------|
| Reduce manual payment matching time | Target: 80% reduction |
| Automate portal submissions | Target: 100% automated |
| Improve cash visibility | Real-time status tracking |
| Reduce errors | Target: <2% error rate |

## 1.3 Scope Summary

| Module | Primary User | Frequency | Volume |
|--------|--------------|-----------|--------|
| Cash Application | AR Team | Daily | 700-800 invoices/month |
| AP Portal Bot | Amy | Monthly | 10 customers, 2 portals |

---

# 2. User Personas

## 2.1 Primary Persona: Amy (AR Specialist)

```
┌─────────────────────────────────────────────────────────────┐
│  AMY - AR Specialist                                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Location:     Malaysia                                     │
│  Reports to:   Daniel                                       │
│  Covers:       Malaysia, Singapore operations               │
│                                                             │
│  Daily Tasks:                                               │
│  • Match bank payments to invoices (Cash Application)       │
│  • Submit invoices to customer portals (Portal Bot)         │
│  • Follow up on overdue payments                            │
│  • Update NetSuite records                                  │
│                                                             │
│  Pain Points:                                               │
│  • Payments arrive without invoice references               │
│  • Manual login to multiple portals is tedious              │
│  • Tracking payment status across portals is scattered      │
│  • Different customers use different portal systems         │
│                                                             │
│  Goals:                                                     │
│  • Spend less time on repetitive matching                   │
│  • Never miss a portal submission deadline                  │
│  • Have one place to see all payment statuses               │
│                                                             │
│  Tech Comfort:  Medium - comfortable with web apps          │
│  Language:      English, Mandarin                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 2.2 Secondary Persona: Daniel (Finance Director)

```
┌─────────────────────────────────────────────────────────────┐
│  DANIEL - Finance Director                                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Location:     Hong Kong                                    │
│  Role:         Oversees AR across all entities              │
│                                                             │
│  Responsibilities:                                          │
│  • Monitor overall AR health                                │
│  • Approve large write-offs or exceptions                   │
│  • Report to leadership on cash flow                        │
│  • Evaluate automation tools                                │
│                                                             │
│  Needs from System:                                         │
│  • Dashboard showing AR status across all entities          │
│  • Exception alerts for items needing attention             │
│  • Monthly/weekly reports                                   │
│  • Audit trail for compliance                               │
│                                                             │
│  Tech Comfort:  Medium - prefers clean, simple interfaces   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 2.3 Tertiary Persona: System Admin

```
┌─────────────────────────────────────────────────────────────┐
│  ADMIN - IT/System Administrator                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Responsibilities:                                          │
│  • Configure NetSuite integration                           │
│  • Set up bank connections                                  │
│  • Manage user access                                       │
│  • Configure portal credentials                             │
│  • Troubleshoot integration issues                          │
│                                                             │
│  Needs from System:                                         │
│  • Clear setup wizards                                      │
│  • Connection status indicators                             │
│  • Error logs and diagnostics                               │
│  • User management interface                                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

# 3. System Overview

## 3.1 High-Level Architecture

![AR Automation Platform Architecture](/sharehub/images/ar-automation-architecture.png)

## 3.2 Module Summary

| Module | Purpose | Key Screens |
|--------|---------|-------------|
| **Dashboard** | Overview of all AR activities | Home, Analytics |
| **Cash Application** | Match payments to invoices | Payments Queue, Matching, Exceptions |
| **AP Portal Bot** | Submit & track portal invoices | Portal Setup, Invoice Queue, Bot Console |
| **Email Hub** | Process remittance emails | Inbox, Email Detail |
| **Reports** | Generate AR reports | Report Builder, Scheduled Reports |
| **Settings** | Configure system | Integrations, Users, Preferences |

---

# 4. Information Architecture

## 4.1 Navigation Structure

```
┌─────────────────────────────────────────────────────────────┐
│  MAIN NAVIGATION (Left Sidebar)                             │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  🏠 Dashboard                                               │
│                                                             │
│  ─── CASH APPLICATION ───                                   │
│  💳 Payments                                                │
│     ├── All Payments                                        │
│     ├── Unmatched                                           │
│     ├── Ready to Post                                       │
│     └── Exceptions                                          │
│  📧 Email Hub                                               │
│     ├── Inbox                                               │
│     └── Processed                                           │
│  👥 Customers                                               │
│                                                             │
│  ─── PORTAL BOT ───                                         │
│  🤖 Portal Invoices                                         │
│     ├── To Submit                                           │
│     ├── Submitted                                           │
│     ├── Awaiting Payment                                    │
│     └── Paid                                                │
│  🔗 Portal Connections                                      │
│  📊 Bot Runs                                                │
│                                                             │
│  ─── REPORTS ───                                            │
│  📈 Analytics                                               │
│  📋 Reports                                                 │
│                                                             │
│  ─── SETTINGS ───                                           │
│  ⚙️ Settings                                                │
│     ├── Integrations                                        │
│     ├── Users & Permissions                                 │
│     ├── Matching Rules                                      │
│     └── Notifications                                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 4.2 Header Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│  [Logo]  [Entity Selector ▼]           🔔 Notifications  [User Menu ▼]  │
│          Hong Kong                      (3)              Amy            │
└─────────────────────────────────────────────────────────────────────────┘
```

## 4.3 Entity Selector (Multi-Entity Support)

Users can switch between legal entities:
- Master Concept HK (HKD)
- Master Concept SG (SGD)
- Master Concept MY (MYR)
- Master Concept TW (TWD)
- Master Concept CN (CNY)
- **All Entities** (consolidated view)

---

# 5. Module 1: Cash Application

## 5.1 Module Overview

**Purpose:** Automate matching of incoming bank payments to open invoices in NetSuite.

**Data Flow:**
```
Bank Statement ─┐
                ├──▶ AI Matching ──▶ Review ──▶ Post to NetSuite
Email Remittance┘
```

## 5.2 Screen: Payments Queue

**URL:** `/payments`

**Purpose:** Central list of all incoming payments to process

### Layout

![Payments Processing Queue](/sharehub/images/ar-payments-queue.png)

**Key Elements:**
- Status tabs with counts (Unmatched, Matched, Ready, Posted, Exception)
- Filter bar with search, bank account, and date range selectors
- Table columns: Date, Description, Amount, Match Confidence, Status, Actions
- Bulk action bar appears when items selected (Auto-Match, Post to NetSuite)
- Pagination controls

### Component Specifications

**Status Badges:**
| Status | Color | Icon | Description |
|--------|-------|------|-------------|
| Unmatched | Gray | ○ | No match found yet |
| Matched | Blue | ● | AI suggested match, needs review |
| Ready | Green | ✓ | Confirmed, ready to post |
| Posted | Light Gray | ✓✓ | Posted to NetSuite |
| Exception | Orange | ⚠ | Needs manual attention |

**Row Actions Menu (···):**
- View Details
- Match Manually
- Mark as Exception
- Mark as Duplicate
- Archive

**Bulk Actions:**
- Process Selected (run AI matching)
- Post Selected to NetSuite
- Mark as Exception
- Export to CSV

---

## 5.3 Screen: Payment Matching (Core Screen)

**URL:** `/payments/{id}/match`

**Purpose:** Review and confirm AI-suggested payment-to-invoice matches

### Layout (Three-Panel)

![Payment Matching Workspace](/sharehub/images/ar-payment-matching.png)

**Three-Panel Structure:**

| Panel | Purpose | Key Elements |
|-------|---------|--------------|
| **Left: Payment Source** | Bank transaction details | Amount, AI confidence score, source details, linked remittance email/attachment |
| **Center: Customer & Invoices** | Invoice selection | Customer dropdown, open invoices list with checkboxes, date/amount/status per invoice |
| **Right: Allocation Summary** | Running totals | Selected invoices, payment vs applied amounts, balance status (Balanced/Over/Under) |

**Footer Actions:** Skip Transaction, Save Draft, Mark Exception, Confirm Match

### Panel 1: Payment Source

**Elements:**
- Bank transaction details (description, date, amount, bank account)
- Linked email indicator (if remittance email found)
- Email preview link
- Attachment preview (Excel/PDF parsing)

### Panel 2: Customer & Invoices

**Elements:**
- Customer dropdown with AI confidence indicator
- Customer search functionality
- List of open invoices for selected customer
- Invoice row details: Number, Amount, Due Date, Days Overdue
- AI match indicator on each invoice
- Multi-select checkboxes
- Manual invoice search

**Invoice Row States:**
| State | Visual |
|-------|--------|
| AI Recommended | Blue highlight + ✓ icon |
| User Selected | Checkbox checked |
| Partial Apply | Amount input field shown |

### Panel 3: Allocation Summary

**Elements:**
- List of selected invoices
- Running total
- Payment vs Applied comparison
- Remaining balance indicator
- Balance status (Balanced / Overpayment / Underpayment)

**Balance States:**
| State | Color | Action |
|-------|-------|--------|
| Balanced ($0) | Green ✓ | Ready to confirm |
| Overpayment | Orange | Prompt to create credit memo |
| Underpayment | Orange | Prompt for partial/short pay handling |

---

## 5.4 Screen: Exception Queue

**URL:** `/payments/exceptions`

**Purpose:** Handle payments that couldn't be automatically matched

### Layout Specification

```
┌─────────────────────────────────────────────────────────────────────────┐
│  Exceptions (4)                                    [Filter by Type ▼]   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  Exception Types: [All] [Unknown Payer] [No Match] [Amount Mismatch]    │
│                                                                         │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │  ⚠ UNKNOWN PAYER                                    3 days in queue│ │
│  │  ─────────────────────────────────────────────────────────────────│ │
│  │                                                                    │ │
│  │  Bank Transaction: "TFR FROM J WONG"                              │ │
│  │  Amount: HK$ 8,500.00  │  Date: Jan 18, 2026                      │ │
│  │                                                                    │ │
│  │  Issue: No customer match found for "J WONG"                      │ │
│  │                                                                    │ │
│  │  Suggestions:                                                      │ │
│  │  ┌──────────────────────────────────────────────────────────────┐ │ │
│  │  │ ○ Wong Trading Co                          67% match         │ │ │
│  │  │ ○ J&J Wong Enterprises                     54% match         │ │ │
│  │  │ ○ Create New Customer                                        │ │ │
│  │  └──────────────────────────────────────────────────────────────┘ │ │
│  │                                                                    │ │
│  │  [Select & Match]    [Request Info from Customer]    [Archive]    │ │
│  │                                                                    │ │
│  └───────────────────────────────────────────────────────────────────┘ │
│                                                                         │
│  ┌───────────────────────────────────────────────────────────────────┐ │
│  │  ⚠ OVERPAYMENT                                      1 day in queue│ │
│  │  ─────────────────────────────────────────────────────────────────│ │
│  │                                                                    │ │
│  │  Customer: Global Tech Solutions                                  │ │
│  │  Payment: HK$ 50,000.00  │  Date: Jan 20, 2026                    │ │
│  │                                                                    │ │
│  │  Best Match: INV-2024-0987 = HK$ 48,500.00                        │ │
│  │  Overpayment: HK$ 1,500.00                                        │ │
│  │                                                                    │ │
│  │  [Apply + Create Credit $1,500]  [Partial Apply]  [Refund]        │ │
│  │                                                                    │ │
│  └───────────────────────────────────────────────────────────────────┘ │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Exception Types

| Type | Icon | Description | Resolution Options |
|------|------|-------------|-------------------|
| Unknown Payer | 👤? | Customer not recognized | Select existing, Create new |
| No Match | 🔍 | Can't find invoice combination | Manual search, Request info |
| Multiple Match | 🔀 | Ambiguous - several possibilities | Select correct match |
| Overpayment | 💰+ | Payment > Invoice total | Credit memo, Refund |
| Underpayment | 💰- | Payment < Invoice total | Partial apply, Short pay |
| Duplicate | 📋📋 | Possible duplicate payment | Confirm or Reject |

---

## 5.5 Screen: Email Hub

**URL:** `/emails`

**Purpose:** View and process incoming remittance emails

### Layout

![Remittance Email Hub](/sharehub/images/ar-email-hub.png)

**Two-Panel Structure:**

| Panel | Purpose | Key Elements |
|-------|---------|--------------|
| **Left: Email List** | Inbox with filters | Status tabs (Open/In Progress/Processed), email cards with sender, subject, auto-tags, date |
| **Right: Email Detail** | Selected email view | Headers (From/To/Date/Subject), email body, AI-extracted invoice data from attachments, Link to Payment action |

**AI Extraction:** Automatically parses Excel/PDF attachments to extract invoice numbers and amounts

### Email Auto-Tags

| Tag | Color | Trigger |
|-----|-------|---------|
| Remittance | Blue | Contains "remittance", "payment advice", payment-related attachment |
| Tax Document | Purple | Contains "tax certificate", "tax exempt" |
| Dispute | Red | Contains "dispute", "query", "incorrect" |
| Statement Request | Yellow | Contains "statement", "balance" |
| General | Gray | Default |

---

# 6. Module 2: AP Portal Bot

## 6.1 Module Overview

**Purpose:** Automate invoice submission to customer procurement portals (Coupa/Ariba) and track approval/payment status.

**Data Flow:**
```
NetSuite Invoice ──▶ Bot ──▶ Coupa/Ariba ──▶ Track Status ──▶ Update NetSuite
```

## 6.2 Screen: Portal Invoices Queue

**URL:** `/portal-invoices`

**Purpose:** Manage invoices requiring portal submission

### Layout

![AP Portal Submission Queue](/sharehub/images/ar-portal-queue.png)

**Key Elements:**
- Status cards with counts (To Submit, Submitted, Approved, Paid, Errors)
- Filter bar with search, portal type, and customer selectors
- Table columns: Invoice #, Customer, Portal (Coupa/Ariba), PO #, Amount, Status
- Run Bot Now primary action button
- Bulk actions: Submit Selected, Remove

### Status Flow

```
┌──────────┐    ┌───────────┐    ┌──────────┐    ┌──────────┐    ┌──────┐
│To Submit │───▶│ Submitted │───▶│ Pending  │───▶│ Approved │───▶│ Paid │
└──────────┘    └───────────┘    │ Approval │    └──────────┘    └──────┘
                     │          └──────────┘          │
                     ▼               │                ▼
                ┌─────────┐         ▼           ┌──────────┐
                │  Error  │    ┌──────────┐     │ Disputed │
                └─────────┘    │ Rejected │     └──────────┘
                               └──────────┘
```

---

## 6.3 Screen: Portal Connections

**URL:** `/portal-connections`

**Purpose:** Configure and manage customer portal credentials

### Layout Specification

```
┌─────────────────────────────────────────────────────────────────────────┐
│  Portal Connections                               [+ Add Connection]    │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │ Customer        │ Portal │ URL                 │ Status │ Actions│   │
│  ├─────────────────┼────────┼─────────────────────┼────────┼────────┤   │
│  │ Acme Corp       │ Coupa  │ acme.coupahost.com  │ ● OK   │ [Edit] │   │
│  │ Last sync: Jan 21, 10:00 AM                    │        │ [Test] │   │
│  ├─────────────────┼────────┼─────────────────────┼────────┼────────┤   │
│  │ BigCo Ltd       │ Ariba  │ supplier.ariba.com  │ ● OK   │ [Edit] │   │
│  │ Last sync: Jan 21, 10:00 AM                    │        │ [Test] │   │
│  ├─────────────────┼────────┼─────────────────────┼────────┼────────┤   │
│  │ GlobalEnt       │ Coupa  │ global.coupahost.com│ ⚠ Error│ [Edit] │   │
│  │ Error: Password expired                        │        │ [Test] │   │
│  ├─────────────────┼────────┼─────────────────────┼────────┼────────┤   │
│  │ TechGlobal      │ Coupa  │ tech.coupahost.com  │ ● OK   │ [Edit] │   │
│  │ Last sync: Jan 20, 2:30 PM                     │        │ [Test] │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Add/Edit Connection Modal

```
┌─────────────────────────────────────────────────────────────────┐
│  Add Portal Connection                                    [X]   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Customer *        [Select Customer               ▼]           │
│                    (from NetSuite)                              │
│                                                                 │
│  Portal Type *     ○ Coupa    ○ Ariba                          │
│                                                                 │
│  Portal URL *      [https://                              ]    │
│                    e.g., customer.coupahost.com                 │
│                                                                 │
│  ───────────────────────────────────────────────────────────── │
│  Credentials                                                    │
│  ───────────────────────────────────────────────────────────── │
│                                                                 │
│  Username *        [                                      ]    │
│                                                                 │
│  Password *        [••••••••••••••••]              [👁 Show]   │
│                                                                 │
│  ───────────────────────────────────────────────────────────── │
│  Advanced Settings (Optional)                          [▼]     │
│  ───────────────────────────────────────────────────────────── │
│                                                                 │
│  │  Invoice # Field:    [Invoice Number           ▼]          │
│  │  PO # Field:         [Purchase Order           ▼]          │
│  │  Amount Field:       [Total Amount             ▼]          │
│  │  Custom Notes:       [                              ]      │
│                                                                 │
│  ───────────────────────────────────────────────────────────── │
│                                                                 │
│        [Test Connection]                   [Cancel]  [Save]    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 6.4 Screen: Bot Console

**URL:** `/bot-console`

**Purpose:** Monitor bot execution in real-time and view run history

### Layout

![Automation Bot Console](/sharehub/images/ar-bot-console.png)

**Key Sections:**

| Section | Purpose |
|---------|---------|
| **Current Run Progress** | Progress bar, invoice count, Pause/Stop controls |
| **Live Log Stream** | Real-time timestamped log entries showing bot actions |
| **Live Preview** | Screenshot of bot browser session (updated in real-time) |
| **Run History Table** | Past runs with date, items processed, success rate, View Logs action |

**Controls:** Pause, Stop Bot, date range filter for history

---

# 7. Shared Components

## 7.1 Dashboard (Home)

**URL:** `/` or `/dashboard`

**Purpose:** Unified view of all AR activities across both modules

### Layout

![Dashboard Overview](/sharehub/images/ar-dashboard-overview.png)

**Key Elements:**
- Cash Application metrics (Received, Matched %, Ready to Post, Exceptions)
- Unprocessed Emails summary with quick actions
- Portal Bot status cards (To Submit, Awaiting Approval, Approved, Paid)
- Attention Required alerts for portal errors
- Recent Activity feed

---

## 7.2 Customer Detail Page

**URL:** `/customers/{id}`

**Purpose:** Unified view of customer across both modules

### Layout

![Customer Financial Profile](/sharehub/images/ar-customer-profile.png)

**Key Sections:**

| Section | Content |
|---------|---------|
| **Header** | Customer name, status badge, ID, email, Statement/Log Call actions |
| **Metrics Cards** | Open AR, Overdue amount, Avg Days to Pay, Credit Limit %, Portal type |
| **Tab Navigation** | Details, Invoices, Payments, Portal Activity, Emails |
| **Open Invoices Table** | Invoice #, Date, Amount, Status (with color badges), Actions |
| **Recent Payments** | Payment date, amount, linked invoice, status |

---

## 7.3 Settings

**URL:** `/settings`

### Settings Navigation

```
Settings
├── Integrations
│   ├── NetSuite Connection
│   ├── Bank Accounts
│   └── Email Integration
├── Matching Rules
│   ├── Confidence Thresholds
│   ├── Customer Name Matching
│   └── Amount Tolerance
├── Users & Permissions
│   ├── User Management
│   └── Roles
├── Notifications
│   ├── Email Alerts
│   └── Slack Integration
└── System
    ├── Entity Management
    ├── Audit Log
    └── Data Export
```

---

# 8. User Flows

## 8.1 Daily Cash Application Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     DAILY CASH APPLICATION FLOW                         │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────┐                                                            │
│  │  START  │  Amy logs in at 9:00 AM                                   │
│  └────┬────┘                                                            │
│       │                                                                 │
│       ▼                                                                 │
│  ┌─────────────┐                                                        │
│  │  Dashboard  │  Reviews overnight summary                            │
│  │             │  - 15 new payments received                           │
│  │             │  - 12 auto-matched (80%)                              │
│  │             │  - 3 new emails                                       │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────────┐                                                        │
│  │ Email Hub   │  Reviews remittance emails                            │
│  │             │  - Links emails to payments                           │
│  │             │  - AI extracts invoice numbers from attachments       │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────────┐                                                        │
│  │  Payments   │  Reviews matched payments                             │
│  │   Queue     │  - Confirms AI suggestions                            │
│  │             │  - Adjusts any incorrect matches                      │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────────┐                                                        │
│  │  Matching   │  Processes unmatched payments                         │
│  │   Screen    │  - Selects customer                                   │
│  │             │  - Selects invoices                                   │
│  │             │  - Confirms allocation                                │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────────┐                                                        │
│  │ Exceptions  │  Handles problem payments                             │
│  │             │  - Unknown payers                                     │
│  │             │  - Over/underpayments                                 │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────────┐                                                        │
│  │   Post to   │  Bulk posts confirmed payments                        │
│  │  NetSuite   │  - Updates invoice status                             │
│  │             │  - Creates customer receipts                          │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────┐                                                            │
│  │   END   │  Daily processing complete                                │
│  └─────────┘                                                            │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## 8.2 Monthly Portal Submission Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                   MONTHLY PORTAL SUBMISSION FLOW                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────┐                                                            │
│  │  START  │  1st of month (or when invoices ready)                    │
│  └────┬────┘                                                            │
│       │                                                                 │
│       ▼                                                                 │
│  ┌─────────────┐                                                        │
│  │  Dashboard  │  Amy sees invoices ready for portal submission        │
│  │             │  - 3 invoices flagged "To Submit"                     │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────────┐                                                        │
│  │   Portal    │  Reviews invoices to submit                           │
│  │   Queue     │  - Verifies PO numbers present                        │
│  │             │  - Selects invoices for submission                    │
│  └──────┬──────┘                                                        │
│         │                                                               │
│         ▼                                                               │
│  ┌─────────────┐    ┌─────────────┐                                     │
│  │  Run Bot    │───▶│    Bot      │  Bot runs automatically            │
│  │             │    │   Console   │  - Logs into each portal           │
│  │             │    │             │  - Uploads invoices                 │
│  └─────────────┘    │             │  - Captures confirmation           │
│                     └──────┬──────┘                                     │
│                            │                                            │
│                            ▼                                            │
│                     ┌─────────────┐                                     │
│                     │   Review    │  Amy reviews results               │
│                     │   Results   │  - 2 successful                    │
│                     │             │  - 1 error (missing PO)            │
│                     └──────┬──────┘                                     │
│                            │                                            │
│         ┌──────────────────┴──────────────────┐                         │
│         │                                     │                         │
│         ▼                                     ▼                         │
│  ┌─────────────┐                       ┌─────────────┐                  │
│  │   Success   │                       │   Handle    │  Fix issue      │
│  │   Status    │                       │   Errors    │  Re-run bot     │
│  │   Updated   │                       │             │                  │
│  └──────┬──────┘                       └──────┬──────┘                  │
│         │                                     │                         │
│         └──────────────────┬──────────────────┘                         │
│                            │                                            │
│                            ▼                                            │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      WEEKLY CHECK                                │   │
│  │   Bot runs "Check Status" to update approval/payment status     │   │
│  │   - Updates "Submitted" → "Approved" → "Paid"                   │   │
│  │   - Alerts Amy to any rejections or delays                      │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

# 9. Wireframes

## Interactive Prototype (Stitch)

**[View Draft Mockups on Google Stitch](https://stitch.withgoogle.com/projects/8323273447344960411?pli=1)**

The Stitch project contains AI-generated UI mockups for:
- AR Automation Dashboard Overview
- Payment Matching Workspace
- Remittance Email Hub
- AP Portal Submission Queue
- Automation Bot Console
- Customer Financial Profile

## 9.1 Wireframe Index

| # | Screen | Priority | Module |
|---|--------|----------|--------|
| W1 | Dashboard | P0 | Shared |
| W2 | Payments Queue | P0 | Cash Application |
| W3 | Payment Matching | P0 | Cash Application |
| W4 | Exception Queue | P1 | Cash Application |
| W5 | Email Hub | P1 | Cash Application |
| W6 | Portal Invoices Queue | P0 | Portal Bot |
| W7 | Portal Connections | P1 | Portal Bot |
| W8 | Bot Console | P1 | Portal Bot |
| W9 | Customer Detail | P2 | Shared |
| W10 | Settings - Integrations | P2 | Shared |

## 9.2 Component Library Requirements

**Status Badges:**
```
● Connected (Green)
○ Unmatched (Gray)
● Matched (Blue)
✓ Ready (Green)
✓✓ Posted (Light Gray)
⚠ Exception (Orange)
✗ Error (Red)
```

**Action Buttons:**
```
Primary:   [Confirm ✓]  - Blue filled
Secondary: [Save Draft] - Blue outline
Danger:    [Delete]     - Red outline
Ghost:     [Cancel]     - Text only
```

**Cards:**
```
Stat Card:  Large number + label + optional trend
Alert Card: Icon + message + action link
List Card:  Title + items + "View All" link
```

**Tables:**
```
- Checkbox column for bulk select
- Sortable column headers
- Status badge column
- Actions menu (···) column
- Pagination footer
```

---

# 10. Design Guidelines

## 10.1 Visual Design Principles

| Principle | Application |
|-----------|-------------|
| **Clarity** | Clear status indicators, no ambiguity |
| **Efficiency** | Minimize clicks for common tasks |
| **Confidence** | Show AI confidence scores prominently |
| **Traceability** | Always link to source documents |

## 10.2 Color Palette

```
Primary:        #2563EB (Blue)
Success:        #16A34A (Green)
Warning:        #F59E0B (Orange/Amber)
Error:          #DC2626 (Red)
Neutral:        #6B7280 (Gray)

Background:     #FFFFFF (White)
Surface:        #F9FAFB (Light Gray)
Border:         #E5E7EB (Gray 200)
Text Primary:   #111827 (Gray 900)
Text Secondary: #6B7280 (Gray 500)
```

## 10.3 Typography

```
Font Family:    Inter (or system default)

Headings:
  H1: 24px / 32px line-height / Semibold
  H2: 20px / 28px line-height / Semibold
  H3: 16px / 24px line-height / Semibold

Body:
  Regular: 14px / 20px line-height / Normal
  Small:   12px / 16px line-height / Normal

Numbers/Currency:
  Use tabular figures for alignment
  Right-align currency columns
```

## 10.4 Spacing

```
Base unit: 4px

Spacing scale:
  xs:  4px
  sm:  8px
  md:  16px
  lg:  24px
  xl:  32px
  2xl: 48px
```

## 10.5 Component Specifications

**Sidebar Navigation:**
- Width: 240px (expanded), 64px (collapsed)
- Background: White
- Active item: Blue background (#EFF6FF)
- Icons: 20px

**Cards:**
- Border radius: 8px
- Shadow: 0 1px 3px rgba(0,0,0,0.1)
- Padding: 16px

**Tables:**
- Row height: 52px
- Header background: #F9FAFB
- Hover state: #F3F4F6
- Border: 1px solid #E5E7EB

**Modals:**
- Width: 480px (small), 640px (medium), 800px (large)
- Border radius: 12px
- Overlay: rgba(0,0,0,0.5)

---

# 11. Responsive Considerations

## 11.1 Breakpoints

```
Desktop:  1280px and above (Primary target)
Laptop:   1024px - 1279px
Tablet:   768px - 1023px
Mobile:   Below 768px (View-only, limited functionality)
```

## 11.2 Responsive Behavior

| Screen | Navigation | Tables | Panels |
|--------|------------|--------|--------|
| Desktop | Full sidebar | Full columns | 3-panel layout |
| Laptop | Collapsible sidebar | All columns | 3-panel, narrower |
| Tablet | Hidden sidebar (hamburger) | Priority columns | 2-panel or stacked |
| Mobile | Bottom nav | Card view | Single panel |

## 11.3 Mobile Limitations

The following features are **desktop-only**:
- Payment matching (3-panel)
- Bot console with live preview
- Bulk operations

Mobile supports:
- Dashboard view
- Read-only lists
- Notifications
- Approval actions

---

# 12. Appendix

## 12.1 Glossary

| Term | Definition |
|------|------------|
| **Cash Application** | Process of matching received payments to open invoices |
| **Remittance Advice** | Document from customer listing which invoices a payment covers |
| **Coupa** | Cloud-based procurement platform used by enterprise customers |
| **Ariba** | SAP's procurement network for supplier management |
| **PO Number** | Purchase Order number - required for invoice submission |
| **Credit Memo** | Document reducing amount owed (for overpayments, returns) |

## 12.2 Reference Screenshots

The Fazeshift demo screenshots (9 pages) provide reference UI patterns:

| Page | Content | Reference For |
|------|---------|---------------|
| 1 | Platform overview | System architecture |
| 2 | Aging Schedule | Customer list view |
| 3-4 | Customer Detail | Customer profile layout |
| 5 | Payments Management | Payments queue design |
| 6-7 | Payment Allocation | Matching screen (KEY REFERENCE) |
| 8 | Email Hub | Email inbox design |
| 9 | Workflows | Automation rules (future reference) |

## 12.3 Technical Constraints

| Constraint | Implication for UX |
|------------|-------------------|
| NetSuite API limits | Batch operations, not real-time |
| Portal bot runs ~2-5 min/invoice | Show progress, not instant |
| Email parsing 95% accuracy | Always show source for verification |
| Multi-entity = separate data | Clear entity context required |

## 12.4 Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| Auto-match rate | 80%+ | Payments matched without manual intervention |
| Time to process | <30 min/day | Time Amy spends on daily cash application |
| Portal submission | 100% automated | Invoices submitted via bot vs manual |
| Error rate | <2% | Incorrect postings requiring correction |

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | Jan 2026 | Claude | Initial UX specification |

---

*End of Document*
