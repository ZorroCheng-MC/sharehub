---
title: "Gov Tender SOP Planning: Comprehensive Documentation & Automation Blueprint"
tags:
  - project
  - SOP
  - government
  - tender-management
  - automation
  - planning
  - documentation
  - workflow
  - technical
  - actionable
  - inbox
date: 2025-11-06
type: project
status: in-progress
priority: high
project: gov-tender-automation
team: Gov Team
contributors: BMad Master, Zorro
---

# Gov Tender SOP Planning: Comprehensive Documentation & Automation Blueprint

![Progress](https://img.shields.io/badge/Progress-60%25-yellow)
![Status](https://img.shields.io/badge/Status-Planning-blue)
![Updated](https://img.shields.io/badge/Updated-2025--11--06-green)

## 📋 Quick Reference

**Project**: Government Team Tender Management System
**Phase**: Planning & Information Gathering (60% Complete)

### Key Facts

| Aspect | Details |
|--------|---------|
| **Data Sources** | 19 tender portals + 1 email inbox |
| **Workflow** | 5-step simplified process |
| **Storage** | Google Drive link-based (replacing folder hierarchy) |
| **Tracking** | 3 spreadsheet sheets (Non-QPS, QPS5, GITP) |
| **Automation** | Modular app design for future development |

### Status at a Glance

✅ **Completed:**
- Portal sources mapped (19 portals)
- Email inbox specified (johnathan.ip@hkmci.com)
- Master spreadsheet structure analyzed
- Workflow simplified and documented
- Field specifications defined

⏸️ **On Hold:**
- Calendar integration specifications
- Google Chat notification setup
- Folder structure finalization

⏳ **Next Needed:**
- Email parsing patterns
- Portal-specific extraction rules
- Document naming conventions

---

## 🎯 Project Overview

### Purpose

This document serves as the **comprehensive planning blueprint** for the Gov Team Tender Management SOP:

1. **Operational Guide** - Day-to-day procedures for admin staff
2. **Automation Foundation** - Technical specifications for modular app development
3. **Knowledge Base** - Complete system documentation with programming-level detail

### Core Objectives

| Objective | Description | Owner |
|-----------|-------------|-------|
| 🔄 **Reduce Admin Workload** | Streamline repetitive tender processing tasks | Admin |
| ✅ **Minimize Human Error** | Standardize processes with validation rules | Admin |
| 🔔 **Enable Prospect Alerts** | Proactive notification system for team | Team |
| 📊 **Support Statistical Analysis** | Data collection and insights generation | Johnathan |

---

## 🌐 Data Sources

### Tender Portal Sources

**Total:** 19 portals categorized by automation capability
**Credentials:** Stored in [Google Sheets](https://docs.google.com/spreadsheets/d/1fYSravZMyCJNJ47MRUGCTL5jYq10kVDMKYO_J3LaK90/edit?gid=0#gid=0)

#### Fully Automated Portals (12) - No Recaptcha/OTP

| Portal | URL | Login ID | Workload % | Status |
|--------|-----|----------|------------|--------|
| **CityU** | https://etender.cityu.edu.hk/en/index.aspx | 2007239240 | 5% | ✅ Fully Auto |
| **CUHK** | https://cupro-cuhk.com/Supplier/en/Login/index.aspx? | 2023011560 | 4% | ✅ Fully Auto |
| **HKBU** | https://ts.tacticaasia.com/login.aspx | - | 3% | ✅ Fully Auto |
| **HKSTP** | https://tender-fm.hkstp.org/en/index.aspx | 3020024230 | 4% | ✅ Fully Auto |
| **HKPC** | https://eproq.hkpc.org/Supplier/en/Login/index.aspx | 10091931 | 4% | ✅ Fully Auto |
| **West Kowloon/M Plus** | https://wkprocure.westkowloon.hk/Supplier/en/Login/New.aspx | 9000008140 | 4% | ✅ Fully Auto |
| **HKU** | [Oracle Portal] | - | 3% | ✅ Fully Auto |
| **MTR** | https://www.hkextender.com/en/logon.aspx? | 1035771 | 3% | ✅ Fully Auto |
| **HKIA** | https://epros.hkairport.com/en/Supplier/Login/New.aspx | 2001881150 | 3% | ✅ Fully Auto |
| **LNU** | https://ln-ts.tacticaasia.com/Supplier/RFxOverview.aspx?id=30 | - | 2% | ✅ Fully Auto |
| **HKUST** | https://w5.ab.ust.hk/jstd/td_login | 1001420792 | 2% | ✅ Fully Auto |
| **HKMA** | N/A | - | 2% | 📝 Manual |

#### Semi-Automated Portals (5) - Requires Recaptcha/OTP

| Portal | URL | Login ID | Workload % | Status |
|--------|-----|----------|------------|--------|
| **gov.hk eprocurement** | https://www2.eprocurement.gov.hk/ePS_External_Portal/pages/login.zul | EPS00003114 | 40% | ⚠️ Semi-Auto |
| **Cyberport** | https://eprocurement.cyberport.hk/login | 201000980 | 4% | ⚠️ Semi-Auto |
| **CIC** | https://supplierportal.cic.hk/ebvp/ | V04449 | 8% | ⚠️ Semi-Auto |
| **PolyU** | Tendering System | 901909370A0 | 5% | ⚠️ Semi-Auto |
| **Hospital Authority** | N/A | - | 4% | 📝 Manual |

### Email Inbox Source

**Email Account:** johnathan.ip@hkmci.com
**Purpose:** Tender notifications and documents sent via email
**Processing:** Semi-automated (email parsing + attachment extraction)

---

## 📊 Master Spreadsheet Structure

**File:** `SOA-QPS - Monthly Return_Master Concept.xlsx`
**Location:** Google Drive (shared)
**Purpose:** Central repository for ALL tender records

### Primary Tracking Sheets (3)

#### 1. Non-QPS Sheet

**Records:** 2,484 tenders
**Columns:** 13 fields
**Purpose:** Track non-QPS contract tenders (universities, institutions)

**Key Fields:**
- Client
- Project Name
- Received Date
- Tender Date
- Contact Name, Contact No., Title, Email
- Submitted / No Go
- Bid Price
- Win / Lost
- Remarks

#### 2. QPS5 Sheet

**Records:** 438 tenders
**Columns:** 153 fields (complex multi-category structure)
**Purpose:** Track QPS5 (Qualified Procurement Scheme 5) government contracts
**Contract Reference:** GCIO90525965-A-J-C18 (Cat A Major)

**Service Categories:**
- Category 1, 2, 3, 4 (One-off Services)
- Category 3 (On-going Services)
- Category 4 (On-going Services)

**Key Fields:**
- Service Category/Group
- Government Bureau/Department (B/D)
- Invitation Issue Date
- Proposal Submission Deadline
- Proposal Ref. #
- Work Assignment Title
- Contact Person, Tel, Title, Email
- Status (6 values)
- Rate fields (Daily/Hourly rates by category)

#### 3. GITP Sheet

**Records:** 29 tenders
**Columns:** 153 fields (same structure as QPS5)
**Purpose:** Track GITP (Government IT Products) procurement tenders

### Supporting Reference Sheets

#### BD_code Sheet
- **Rows:** 85 government departments
- **Purpose:** Department code lookup
- **Fields:** No., Department Code, Department Name
- **Examples:** AFCD, ArchSD, AUD, BD, CEDD, etc.

#### Category Sheet
- **Rows:** 6 categories
- **Values:** 1, 2/J, 2/N, 3/J, 3/N, 4

#### Status Sheet
- **Rows:** 6 status values
- **Values:** Debarred, Invitation Issued, Proposal Submitted, Proposal Rejected, Assignment Awarded, Assignment Cancelled

---

## 🔄 Simplified 5-Step Workflow

**Status:** Updated to reflect Google Drive link approach
**Key Changes:** Replaced folder hierarchy (Steps 1-4 old) with direct upload + link storage

### Process Overview

```mermaid
graph LR
    A[Download Tender] --> B[Upload to Drive]
    B --> C[Extract Info]
    C --> D[Store Link]
    D --> E[Update Spreadsheet]
    E --> F[Calendar/Alerts]
    F -.-> G[ON HOLD]
```

---

### Step 1: Upload Tender Document to Google Drive

#### Admin Action

1. Download tender document from portal/email
2. Navigate to: `Shared drive → Govt Team Drive → 0_Government Team Drive → 0_Projects / Tenders → 2_Government Projects / Tenders → [Upload Area]`
3. Upload document file(s)
4. Verify file integrity

**File Types:** PDF, DOC/DOCX, XLS/XLSX, ZIP

#### Automation Requirements

**Input:** Downloaded file from portal scraper OR email attachment
**Action:** Upload via Google Drive API
**Validation:**
- Allowed types: .pdf, .doc, .docx, .xls, .xlsx, .zip
- Size limit: 50MB
- Virus scan: Required

**Output:** File ID and Google Drive link
**Error Handling:** Retry on network failure, alert if file too large

**Note:** Folder structure under discussion → replacing with link-based storage

---

### Step 2: Extract Tender Information

#### Admin Action

1. Open uploaded document
2. Extract key information:
   - Client/B/D name
   - Project name (full title)
   - Reference number
   - Submission deadline (date + time)
   - Contact information (name, phone, email, title)

#### Decision Points

- **If unclear:** Flag for manual review
- **Multiple deadlines:** Use final submission deadline
- **Missing contact:** Use dept general contact from BD_code

#### Automation Requirements

**Document Parsing:**
- OCR/text extraction from PDF/DOC
- Pattern matching (portal-specific regex)
- NLP extraction for unstructured docs

**Field Extraction Patterns:**
- **Reference Number:** Portal-specific formats
  - `GCIO\d{8}-[A-Z]-[A-Z]-C\d{1,2}-\d{3}`
  - `GITP-2024-\d{3}`
- **Date/Time:** Multiple formats (DD/MM/YYYY HH:MM, YYYY-MM-DD, etc.)
- **Contact:** Name, title, phone, email patterns
- **B/D:** Match against BD_code sheet

**Validation:** Required fields present, dates in future, email format
**Output:** Structured data object
**Error Handling:** Confidence scores, flag low-confidence for review

---

### Step 3: Store Google Drive Link

#### Current Approach (Under Discussion)

**Previous:** Create B/D folder → Project folder → Upload
**Proposed:** Store Google Drive link directly in spreadsheet

#### Benefits
- ✅ Eliminates folder navigation complexity
- ✅ Faster retrieval via spreadsheet search
- ✅ Simplified automation
- ✅ Single source of truth

#### Automation Requirements

**Action:** Generate shareable link via Google Drive API
**Permissions:** Team-only, view-only
**Link Format:** `https://drive.google.com/file/d/[FILE_ID]/view`
**Storage:** New spreadsheet column "Document Link" or "Google Drive Link"
**Fallback:** Support both folder structure AND link during transition

---

### Step 4: Update Spreadsheet (with Google Drive Link)

#### Admin Action

1. Open: `SOA-QPS - Monthly Return_Master Concept`
2. Determine sheet:
   - **Non-QPS:** CityU, CUHK, MTR, HKIA, etc.
   - **QPS5:** Gov QPS5 contracts
   - **GITP:** Gov IT products tenders
3. Add new row with extracted data (Step 2)
4. Include Google Drive link (Step 3)
5. Leave blank: Submitted/No Go, Bid Price, Win/Lost, Remarks

#### Field Validation Rules

| Field | Rule |
|-------|------|
| **Client** | Must exist in BD_code OR recognized institution |
| **Received Date** | Auto-populate current date, format DD/MM/YYYY |
| **Tender Date** | Must be future date, format DD/MM/YYYY HH:MM |
| **Email** | Valid email format |
| **Status** | From Status sheet values |
| **Google Drive Link** | Valid Google Drive URL |

#### New Spreadsheet Column

**Column Name:** "Document Link" or "Google Drive Link"
**Position:** After "Email" column (before "Submitted/No Go")
**Format:** Hyperlink (clickable URL)

#### Automation Requirements

**Google Sheets API:** Append row to appropriate sheet

**Sheet Selection Logic:**
```python
if tender_source in [gov.hk, QPS portals]:
    if tender_type == "GITP":
        sheet = "GITP"
    else:
        sheet = "QPS5"
else:
    sheet = "Non-QPS"
```

**Data Mapping:**
- Map extracted fields to columns
- Include Google Drive link
- Auto-populate Received Date
- Format dates: DD/MM/YYYY (display), ISO (storage)

**Error Handling:** Log missing fields, flag for manual completion

---

### Step 5: Mark Calendar & Alerts

**Status:** ⏸️ ON HOLD - To be defined later

#### Planned Features
- Google Calendar event creation with reminders
- Google Chat notification to team
- Email alerts at milestones

#### Reminder Schedule (Draft)
- 📅 7 days before deadline
- 📅 3 days before deadline
- 📅 1 day before deadline
- 📅 2 hours before deadline (if time specified)

#### Placeholder Specifications
- Calendar ID: TBD
- Chat webhook URL: TBD
- Reminder timing: TBD
- Notification templates: TBD

---

## 📧 Email Inbox Processing Workflow

### Email Account
**johnathan.ip@hkmci.com**

### Current Manual Process

**Admin Action:**
1. Check inbox regularly (daily/multiple times)
2. Identify tender emails by:
   - Subject keywords
   - Sender addresses
   - Attachments present
3. Download attachments
4. Proceed with Steps 1-5

### Email Identification Criteria

#### Subject Line Keywords
- "Tender"
- "Request for Quotation" / "RFQ"
- "Request for Proposal" / "RFP"
- "Invitation to Tender"
- "Expression of Interest" / "EOI"
- Reference numbers (GCIO, GITP, dept codes)

#### Sender Domains
- @gov.hk
- @cityu.edu.hk, @cuhk.edu.hk (university domains)
- @hkstp.org, @hkpc.org, @wkcda.hk

#### Attachment Indicators
- PDF/DOC/ZIP attachments present

### Automation Requirements

**Email API:** IMAP or Gmail API
**Authentication:** OAuth2 or App Password
**Frequency:** Every 15-30 minutes OR real-time webhooks

#### Email Parsing Logic

```python
for email in inbox.unread():
    if (subject contains tender_keywords OR
        sender_domain in known_sources OR
        has_attachment and attachment_type in allowed_types):

        mark_as_potential_tender()

        extract:
            - sender_info
            - subject_line
            - email_body
            - all_attachments

        save_attachments_to_temp()

        parse_body_for:
            - reference_number
            - deadline_datetime
            - contact_info
            - project_title

        trigger_workflow_steps_1_5()

        if successful:
            mark_as_processed()
            add_label("Tender/Processed")
        else:
            flag_for_review()
            add_label("Tender/Review Needed")
```

#### Attachment Handling

**Naming Convention:** `{date}_{sender}_{original_filename}`
**Temporary Storage:** Before upload to Google Drive
**Validation:** File types and sizes
**Metadata:** Extract info from filename (often contains ref number)

#### Error Handling

| Error | Action |
|-------|--------|
| No attachments | Flag for manual review |
| Multiple attachments | Process all OR combine to ZIP |
| Unreadable attachment | Alert admin |
| Ambiguous content | Lower confidence, flag review |

#### Email Status Tracking (Labels)

- **Tender/Unprocessed** - Identified but not processed
- **Tender/Processed** - Successfully added to system
- **Tender/Review Needed** - Requires manual attention
- **Tender/Duplicate** - Already in system
- **Tender/Spam** - False positive

---

## 📝 Data Field Specifications

### Core Required Fields (All Sheets)

| # | Field | Type | Description | Automation |
|---|-------|------|-------------|------------|
| 1 | **Client/B/D** | String | Department code or client name | Match BD_code sheet |
| 2 | **Project Name** | String (max ~500 chars) | Full tender description | Extract from title |
| 3 | **Received Date** | Date (YYYY-MM-DD) | Date tender received | Auto-populate current date |
| 4 | **Tender Date** | DateTime (YYYY-MM-DD HH:MM) | Submission deadline | Parse from document |
| 5 | **Contact Name** | String | Primary contact person | Extract from doc |
| 6 | **Contact No.** | String | Phone/fax number | Extract from doc |
| 7 | **Title** | String (Optional) | Contact job title | Extract if available |
| 8 | **Email** | Email format | Contact email | Validate format |
| 9 | **Google Drive Link** | URL | Document link (NEW) | Generated in Step 3 |
| 10 | **Submitted/No Go** | Enum | Participation decision | Manual input later |
| 11 | **Bid Price** | Currency or "N.A" | Submitted bid amount | Manual input later |
| 12 | **Win/Lost** | Enum | Tender outcome | Manual input later |
| 13 | **Remarks** | String (Optional) | Additional notes | Manual input later |
| 14 | **Status** | Enum | From Status sheet | Default: "Invitation Issued" |

### QPS5/GITP Specific Fields

| Field | Format | Description |
|-------|--------|-------------|
| **Service Category/Group** | String | "1 - Cat. 1", "2/J - Cat. 2 major", etc. |
| **Proposal Ref. #** | String | GCIO 5/12-4-C12-xxx or GITP-2024-xxx |
| **Invitation Issue Date** | Date (DD/MM/YYYY) | Date invitation issued |

---

## ℹ️ Information Still Needed

### Pending Specifications

- [x] Tender list sample - **✅ COMPLETED**
- [x] Data sources and formats - **✅ COMPLETED**
- [x] Spreadsheet schema - **✅ COMPLETED**
- [x] Email inbox - **✅ COMPLETED** (johnathan.ip@hkmci.com)
- [⏸] B/D folder structure - **⏸️ ON HOLD** (Replacing with links)
- [⏸] Calendar specs - **⏸️ ON HOLD** (Later)
- [⏸] Google Chat specs - **⏸️ ON HOLD** (Later)
- [ ] Email parsing patterns (subject lines, sender filters)
- [ ] Document naming conventions
- [ ] Portal-specific reference number patterns
- [ ] Data extraction rules per portal

---

## 📅 Session Discussion Log

### Session 1: 2025-11-06

**Participants:** User, BMad Master Agent

#### Topics Covered

1. **Project Initiation**
   - Confirmed objective: SOP for Gov team tender management
   - Purpose: Operations + automation foundation
   - Programming-level detail required

2. **Meeting Context Captured**
   - Core objectives documented
   - 6-step process provided (later simplified to 5)
   - Action items noted

3. **Data Sources Documented**
   - 19 portals identified with credentials
   - Categorized: 12 fully auto, 5 semi-auto, 2 manual
   - Email inbox: johnathan.ip@hkmci.com

4. **Spreadsheet Analysis**
   - Analyzed master Excel file
   - 3 primary sheets: Non-QPS, QPS5, GITP
   - Supporting sheets: BD_code, Category, Status
   - 13 core + 3 QPS-specific fields

5. **Workflow Simplification**
   - Original: 6 steps with folder hierarchy
   - **Decision:** Replace folders with Google Drive links
   - Simplified to: 5-step process

6. **Integration Decisions**
   - Calendar: ON HOLD
   - Google Chat: ON HOLD
   - Folder structure: ON HOLD (pending decision)

7. **Email Processing Workflow**
   - Account: johnathan.ip@hkmci.com
   - Identification criteria documented
   - Automation logic created
   - Email labeling system designed

8. **Information Gaps Identified**
   - Email parsing patterns needed
   - Document naming conventions needed
   - Portal-specific extraction patterns needed

#### Decisions Made

- ✅ Use Google Drive link storage vs folder structure
- ✅ Focus on 3 primary sheets (Non-QPS, QPS5, GITP)
- ✅ Email inbox: johnathan.ip@hkmci.com
- ⏸️ Hold calendar/chat integration
- ⏸️ Hold folder structure pending discussion

---

## 🚀 Next Steps Guide

### How to Continue in Next Session

**This document is self-contained.** Start fresh with just this file.

#### Option A: Information Gathering

Continue collecting detailed specifications:

**Email Parsing Patterns:**
- Subject line keyword patterns
- Sender whitelist/blacklist
- Email body parsing rules
- Attachment identification logic

**Document Naming Conventions:**
- File naming format
- Date format (download vs tender date)
- Reference number handling
- Special character sanitization

**Portal-Specific Extraction:**
- Reference number regex per portal
- Date format per portal
- Contact info layout per portal
- Project title location per portal

#### Option B: Draft SOP Document

Create formal operational SOP:
- Admin daily procedures
- Step-by-step instructions
- Troubleshooting guide
- Edge cases and exceptions

#### Option C: Design Automation Architecture

Design modular app:
- Portal scrapers (per source)
- Document parser module
- Google Drive integration
- Google Sheets integration
- Calendar integration (when ready)
- Chat notification module (when ready)

#### Option D: Refine Existing Sections

Add more detail:
- Expand workflow steps
- Clarify ambiguous areas
- Add validation rules
- Document error scenarios

---

## 🏷️ Tags & Metadata

**Content Analysis:**
- **Type:** `project` (Planning documentation)
- **Topics:** Government tender management, SOP, automation, workflow
- **Characteristics:** Technical, actionable, comprehensive
- **Priority:** `high` - Foundation for major automation project

**Why These Tags:**
This document combines project planning, technical specifications, and operational procedures for a comprehensive tender management system. Tags enable discovery across project management, SOP documentation, and automation development contexts.

**Suggested Bases Filters:**
- Find project docs: `type = project AND status = in-progress`
- Find automation specs: `tags contains "automation" AND tags contains "technical"`
- Find workflow docs: `tags contains "workflow" AND tags contains "SOP"`
- Find high priority items: `priority = high AND status = in-progress`

---

## 📊 Document Metadata

| Field | Value |
|-------|-------|
| **Created** | 2025-11-06 |
| **Last Updated** | 2025-11-06 |
| **Status** | Paused - Ready for Next Session |
| **Contributors** | BMad Master, Zorro |
| **Project** | Gov Tender Automation |
| **Related Files** | TBD (SOP document, architecture diagrams) |
| **Next Session** | Resume with Option A, B, C, or D |

---

**💡 Quick Start for Next Session:**

```
Please read @project-notes/gov-tender-sop-planning.md and continue
our Gov Tender SOP project from where we left off.
```

---

*Document Version: 1.0 | Self-contained for next session | All context preserved*
