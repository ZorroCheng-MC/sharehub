---
title: "PDPO Compliance Assessment Report - Shave for Hope"
type: reference
status: evergreen
access: private
topics:
  - architecture
  - coding
tags:
  - security
  - privacy
  - compliance
  - PDPO
  - Hong-Kong
  - assessment
  - technical
  - actionable
created: 2026-01-23
---

# PDPO Compliance Assessment Report
## Shave for Hope (剃亮希望) Web Application

---

| Field | Value |
|-------|-------|
| **Assessment Date** | 2026-01-23 |
| **Campaign Duration** | 60 days |
| **Report Version** | 1.0 |
| **Classification** | Internal Use Only |
| **Prepared For** | Management Review & Development Team |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope & Context](#2-scope--context)
3. [Data Inventory](#3-data-inventory)
4. [PDPO Principles Assessment](#4-pdpo-principles-assessment)
5. [Security Controls Review](#5-security-controls-review)
6. [Third-Party Data Sharing](#6-third-party-data-sharing)
7. [Risk Register](#7-risk-register)
8. [Action Items for Development Team](#8-action-items-for-development-team)
9. [Post-Campaign Data Handling](#9-post-campaign-data-handling)
10. [Appendix: Evidence References](#10-appendix-evidence-references)

---

## 1. Executive Summary

### Overall Risk Level: **MEDIUM** 🟠

The Shave for Hope application is a 60-day fundraising campaign supporting the Children's Cancer Foundation (CCF). The application collects personal data including user profiles, biometric images (portraits), and payment information for donation processing.

### Key Findings

| Category | Status | Summary |
|----------|--------|---------|
| **Privacy Policy** | ✅ Adequate | Linked to CCF's existing policy; recommend supplementary notice |
| **Data Security** | 🟠 Needs Attention | Firestore security rules require tightening for payment data |
| **Third-Party Sharing** | 🟡 Acceptable | Google AI and payment providers; standard for this use case |
| **Data Retention** | ✅ Acceptable | 60-day campaign with planned post-campaign deletion |
| **User Rights (DSAR)** | 🟡 Acceptable | Manual handling sufficient for short campaign |

### Management Decision Required

1. **Approve** supplementary privacy notice for AI image processing disclosure
2. **Confirm** post-campaign data deletion timeline (recommend: 30 days after campaign end)
3. **Assign** developer resources to address P1 Firestore rules (estimated: 2-4 hours)

### Bottom Line

The application is **substantially compliant** with PDPO requirements for a short-term charitable campaign. Three minor remediation items are recommended before or shortly after launch to mitigate residual risks.

---

## 2. Scope & Context

### 2.1 Application Overview

| Attribute | Description |
|-----------|-------------|
| **Purpose** | Fundraising campaign where users upload photos, AI transforms them to show a "shaved head" look, and users share to raise donations for childhood cancer support |
| **Target Audience** | Hong Kong residents |
| **Campaign Duration** | 60 days |
| **Data Controller** | Children's Cancer Foundation (CCF) |
| **Hosting** | Firebase (Google Cloud) - Hong Kong/Asia region |

### 2.2 Technology Stack

| Component | Technology | Data Protection Relevance |
|-----------|------------|---------------------------|
| Frontend | Next.js 15, React 19 | Client-side validation |
| Backend | Firebase (Auth, Firestore, Storage) | Google-managed encryption, access controls |
| AI Processing | Google Genkit with Gemini 2.5 Flash | Third-party image processing |
| Payments | Alipay, PayMe (HSBC) | Third-party payment processing |
| Secrets Management | Firebase Environment Config | ✅ Not hardcoded in codebase |

### 2.3 Assessment Boundaries

**In Scope:**
- Source code review
- Firebase security rules
- Data flow analysis
- Third-party integration review

**Out of Scope:**
- Firebase project configuration audit (requires console access)
- Penetration testing
- Payment provider compliance (assumed PCI-DSS compliant)
- CCF organizational policies

### 2.4 Risk Context Adjustments

The following factors reduce the overall risk profile:

| Factor | Impact on Assessment |
|--------|---------------------|
| **60-day duration** | Reduced exposure window; long-term retention concerns not applicable |
| **Charitable purpose** | Lower attractiveness to attackers vs. commercial targets |
| **CCF as data controller** | Established organization with existing compliance framework |
| **Firebase hosting** | Google-managed security controls, encryption, DDoS protection |
| **Local codebase** | Secrets not exposed in public repositories |

---

## 3. Data Inventory

### 3.1 Personal Data Collected

| Data Field | Category | Collection Point | Storage Location | Retention |
|------------|----------|------------------|------------------|-----------|
| Email Address | Personal Data | Registration, Donation | Firebase Auth, Firestore | Campaign + 30 days |
| Display Name | Personal Data | Registration | Firestore `users` | Campaign + 30 days |
| Password | Sensitive | Registration | Firebase Auth (hashed) | Campaign + 30 days |
| Portrait Photo | Biometric | Image Upload | Firebase Storage | Campaign + 30 days |
| Transformed Image | Derived Data | AI Processing | Firebase Storage | Campaign + 30 days |
| Phone Number | Personal Data | Donation Receipt (optional) | Firestore `donors` | Campaign + 30 days |
| Mailing Address | Personal Data | Donation Receipt (optional) | Firestore `donors` | Campaign + 30 days |
| Donation Amount | Financial | Payment | Firestore `paymentTransactions` | 7 years (tax records) |
| Instagram Handle | Personal Data | Profile (optional) | Firestore `users` | Campaign + 30 days |
| Anonymous Session ID | Pseudonymous | Browser | Firestore `anonymousQuotas` | 7 days |

### 3.2 Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         DATA FLOW OVERVIEW                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│   USER DEVICE              FIREBASE                 THIRD PARTIES        │
│   ───────────              ────────                 ─────────────        │
│                                                                          │
│   ┌─────────┐    HTTPS    ┌─────────────┐                               │
│   │ Browser │ ──────────► │ Firebase    │                               │
│   │         │             │ Hosting     │                               │
│   └────┬────┘             └──────┬──────┘                               │
│        │                         │                                       │
│        │ Upload Photo            │                                       │
│        ▼                         ▼                                       │
│   ┌─────────┐             ┌─────────────┐         ┌──────────────┐      │
│   │ Image   │ ──────────► │ Firebase    │ ──────► │ Google       │      │
│   │ (5MB)   │   Base64    │ Storage     │  API    │ Gemini AI    │      │
│   └─────────┘             └─────────────┘         └──────┬───────┘      │
│                                                          │               │
│                                  ◄───────────────────────┘               │
│                            Transformed Image                             │
│                                                                          │
│   ┌─────────┐             ┌─────────────┐         ┌──────────────┐      │
│   │ Payment │ ──────────► │ Firestore   │ ──────► │ Alipay /     │      │
│   │ Form    │   Donor     │ Database    │  API    │ PayMe        │      │
│   └─────────┘   Info      └─────────────┘         └──────────────┘      │
│                                                                          │
│   Legend:                                                                │
│   ──────► Data flow (all HTTPS/TLS encrypted)                           │
│   Personal data highlighted in collection/storage                        │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.3 Data Classification Summary

| Classification | Count | Examples |
|----------------|-------|----------|
| **Sensitive Personal Data** | 4 | Password, portrait photos, phone, address |
| **Personal Data** | 5 | Email, name, Instagram, donation amount, IP |
| **Pseudonymous Data** | 2 | Anonymous session ID, transaction ID |
| **Non-Personal Data** | 3 | Aggregate statistics, timestamps, URLs |

---

## 4. PDPO Principles Assessment

### 4.1 Summary Matrix

| Principle           | Requirement               | Status        | Notes                                                    |
| ------------------- | ------------------------- | ------------- | -------------------------------------------------------- |
| **DPP1** Collection | Necessary, with notice    | ✅ Pass        | CCF policy linked; recommend supplementary notice        |
| **DPP2** Accuracy   | Accurate, up-to-date      | ✅ Pass        | Validation in place; users can update profile            |
| **DPP3** Use        | Limited to stated purpose | ✅ Pass        | Data used only for campaign purposes                     |
| **DPP4** Security   | Reasonable safeguards     | 🟠 Partial    | Firebase provides strong baseline; rules need tightening |
| **DPP5** Openness   | Transparent practices     | ✅ Pass        | CCF policy available                                     |
| **DPP6** Access     | Right to access/correct   | 🟡 Acceptable | Manual process sufficient for 60-day campaign            |

### 4.2 Detailed Assessment

#### DPP1: Collection Limitation Principle ✅

**Requirement:** Personal data must be collected for a lawful purpose, collection must be necessary, and individuals must be informed.

**Current State:**
- Data collection is necessary for campaign operation (registration, donations, image transformation)
- Privacy policy linked to CCF's existing policy at `https://ccf.org.hk/zh-hant/disclaimer/`
- Registration form includes terms acceptance checkbox

**Gap:** CCF's policy does not explicitly mention AI image processing by Google.

**Recommendation:** Add supplementary notice (see Section 8.1).

**Status:** ✅ **PASS** (with minor recommendation)

---

#### DPP2: Accuracy Principle ✅

**Requirement:** Personal data must be accurate and kept up-to-date.

**Current State:**
- Email validation via Zod schema
- Users can update their profile via settings page
- Display name has minimum length validation

**Status:** ✅ **PASS**

---

#### DPP3: Use Limitation Principle ✅

**Requirement:** Personal data must not be used for purposes other than those stated at collection.

**Current State:**
- Data used only for: user authentication, image transformation, donation processing, receipt generation
- No marketing use
- No analytics beyond aggregate statistics
- CCF policy states data is "for internal use only"

**Status:** ✅ **PASS**

---

#### DPP4: Security Principle 🟠

**Requirement:** Reasonable security measures appropriate to the sensitivity of data.

**Current State:**

| Control | Status | Details |
|---------|--------|---------|
| Encryption in Transit | ✅ | Firebase enforces HTTPS |
| Encryption at Rest | ✅ | Google-managed encryption for Firestore and Storage |
| Authentication | ✅ | Firebase Auth with secure password hashing |
| Secrets Management | ✅ | Stored in Firebase config, not in codebase |
| Access Control | 🟠 | Firestore rules need tightening (see below) |
| Input Validation | ✅ | Zod schemas for client-side validation |

**Issue:** Firestore security rules for `paymentTransactions` and `donors` collections allow unauthenticated writes:

```javascript
// Current (RISKY)
match /paymentTransactions/{transactionId} {
  allow read, create, update: if true;  // Anyone can create/update
  allow delete: if false;
}
```

**Risk:** An attacker could potentially create fake payment records or manipulate donation data.

**Status:** 🟠 **PARTIAL PASS** - Requires remediation (see Section 8.2)

---

#### DPP5: Openness Principle ✅

**Requirement:** Organizations must be transparent about their data practices.

**Current State:**
- Privacy policy available at CCF website
- Link provided during registration
- CCF is a registered charity with public accountability

**Status:** ✅ **PASS**

---

#### DPP6: Access and Correction Principle 🟡

**Requirement:** Individuals have the right to access and correct their personal data.

**Current State:**
- Users can view their own profile and transformations
- Users can update their profile information
- No self-service data export or deletion endpoint

**Assessment for 60-Day Campaign:**
- Low volume of requests expected for short charity campaign
- Manual handling via CCF support is acceptable
- Full DSAR automation would be over-engineering

**Status:** 🟡 **ACCEPTABLE** for campaign duration

---

## 5. Security Controls Review

### 5.1 Security Posture Summary

| Domain | Rating | Notes |
|--------|--------|-------|
| **Authentication** | Strong | Firebase Auth with Google OAuth and email/password |
| **Authorization** | Needs Work | Firestore rules too permissive for payment data |
| **Encryption** | Strong | TLS in transit, AES-256 at rest (Google-managed) |
| **Secrets Management** | Strong | Firebase environment config (not in codebase) |
| **Input Validation** | Adequate | Client-side Zod validation; server trusts Firebase |
| **Logging** | Adequate | Firebase provides operational logs |
| **DDoS Protection** | Strong | Firebase/Google Cloud built-in protection |

### 5.2 Firestore Security Rules Analysis

| Collection                 | Current Access          | Risk Level  | Recommendation                 |
| -------------------------- | ----------------------- | ----------- | ------------------------------ |
| `users`                    | Auth required for write | ✅ Low       | No change needed               |
| `transformations`          | Auth required for write | ✅ Low       | No change needed               |
| `anonymousTransformations` | Open create/read        | 🟡 Medium   | Acceptable for campaign        |
| `donors`                   | Open create/update      | 🟠 High     | Tighten rules                  |
| `paymentTransactions`      | Open create/update      | 🔴 Critical | Tighten rules                  |
| `shares`                   | Open create/read        | 🟡 Medium   | Acceptable for sharing feature |
| `statistics`               | Open write              | 🟡 Medium   | Consider rate limiting         |

### 5.3 Payment Security

| Control | Status |
|---------|--------|
| PCI-DSS Compliance | ✅ Handled by Alipay/PayMe (not our scope) |
| No card data stored | ✅ Only transaction references stored |
| Webhook integrity | 🟡 Recommend adding signature verification |

---

## 6. Third-Party Data Sharing

### 6.1 Third-Party Services

| Service | Data Shared | Purpose | Legal Basis | DPA Required |
|---------|-------------|---------|-------------|--------------|
| **Google Firebase** | All app data | Hosting, database, storage | Legitimate interest | ✅ Covered by Google ToS |
| **Google Gemini AI** | Portrait images | Image transformation | Consent (implicit in use) | ✅ Covered by Google ToS |
| **Alipay** | Name, email, amount | Payment processing | Contract performance | ✅ Merchant agreement |
| **PayMe (HSBC)** | Name, email, amount | Payment processing | Contract performance | ✅ Merchant agreement |

### 6.2 Data Transfer Considerations

| Destination | Data Residency | Adequacy |
|-------------|----------------|----------|
| Google Cloud (Firebase) | Asia (configurable) | ✅ Adequate - Google certified |
| Alipay | Mainland China / HK | ✅ Adequate - regulated financial institution |
| PayMe | Hong Kong | ✅ Adequate - HSBC regulated |

### 6.3 Recommended Disclosure

The following should be disclosed to users (via supplementary notice):

> Your portrait photo will be processed by Google AI services to generate the "shaved head" effect. Payment information is processed by third-party payment providers (Alipay/PayMe). All personal data will be deleted within 30 days after the campaign ends.

---

## 7. Risk Register

### 7.1 Identified Risks

| ID | Risk | Likelihood | Impact | Current Controls | Residual Risk | Action |
|----|------|------------|--------|------------------|---------------|--------|
| R1 | Fake payment records created | Medium | High | None | 🔴 High | Tighten Firestore rules |
| R2 | Donor data manipulation | Medium | Medium | None | 🟠 Medium | Tighten Firestore rules |
| R3 | Portrait images accessed by unauthorized party | Low | Medium | Storage rules require auth | 🟢 Low | None |
| R4 | Statistics manipulation | Medium | Low | None | 🟡 Low | Accept for campaign |
| R5 | User unaware of AI processing | Low | Low | CCF policy linked | 🟢 Low | Add supplementary notice |
| R6 | Data retained beyond necessary period | Low | Medium | Planned deletion | 🟢 Low | Execute post-campaign |

### 7.2 Risk Acceptance

The following risks are **accepted** for the 60-day campaign duration:

| Risk | Rationale |
|------|-----------|
| No automated DSAR endpoint | Manual handling sufficient; low request volume expected |
| Open statistics writes | Low impact; can be manually corrected |
| No webhook signature verification | Payment providers have own fraud detection; manual reconciliation possible |

---

## 8. Action Items for Development Team

### 8.1 P1: Add Supplementary Privacy Notice

**Priority:** P1 (Before Launch)
**Estimated Effort:** 1-2 hours
**Owner:** Frontend Developer

**Task:** Add a brief supplementary notice on the registration and/or image upload page.

**Suggested Text (Traditional Chinese):**

```
私隱補充聲明：
• 你的相片將透過 Google AI 服務處理以產生「剃頭」效果
• 捐款資料由第三方支付服務商（Alipay / PayMe）處理
• 活動結束後 30 天內，所有個人資料將被刪除
• 詳情請參閱兒童癌病基金私隱政策
```

**Location Options:**
- Below image upload component (`src/components/image-uploader.tsx`)
- On registration form (`src/components/inline-registration.tsx`)
- As a tooltip/info icon near the upload button

**Files to Modify:**
- `src/lib/constants.ts` - Add translated strings
- `src/components/image-uploader.tsx` or `src/components/inline-registration.tsx` - Add notice component

---

### 8.2 P1: Tighten Firestore Security Rules

**Priority:** P1 (Before Launch or Within First Week)
**Estimated Effort:** 2-4 hours
**Owner:** Backend Developer

**Task:** Update `firestore.rules` to restrict write access to payment and donor collections.

**Current State (Risky):**
```javascript
match /paymentTransactions/{transactionId} {
  allow read, create, update: if true;
  allow delete: if false;
}

match /donors/{oddonorId} {
  allow read, create, update: if true;
  allow delete: if false;
}
```

**Recommended Changes:**

Option A - Require Authentication:
```javascript
match /paymentTransactions/{transactionId} {
  // Only authenticated users or server can create
  allow read: if true;
  allow create: if request.auth != null;
  allow update: if request.auth != null && request.auth.uid == resource.data.userId;
  allow delete: if false;
}

match /donors/{donorId} {
  allow read: if true;
  allow create: if request.auth != null;
  allow update: if request.auth != null && request.auth.uid == resource.data.odUserId;
  allow delete: if false;
}
```

Option B - Use Firebase Admin SDK (Server-Side Only):
- Remove client-side write access entirely
- Create Cloud Function or API route to handle payment/donor creation
- Webhook handlers use Admin SDK (bypasses rules)

**Files to Modify:**
- `firestore.rules`
- Potentially `src/app/api/payment-test/*/route.ts` if switching to server-side writes

**Testing Required:**
- Verify donation flow still works
- Verify payment webhooks still update records
- Verify anonymous donations (if supported) still function

---

### 8.3 P2: Implement Post-Campaign Data Deletion Script

**Priority:** P2 (Before Campaign End)
**Estimated Effort:** 4-8 hours
**Owner:** Backend Developer

**Task:** Create a script/Cloud Function to delete all personal data 30 days after campaign ends.

**Data to Delete:**

| Collection | Action | Notes |
|------------|--------|-------|
| `users` | Delete all documents | Cascade to Firebase Auth |
| `transformations` | Delete all documents | |
| `anonymousTransformations` | Delete all documents | Should auto-expire but verify |
| `donors` | Archive then delete | Keep anonymized donation totals for reporting |
| `shares` | Delete all documents | |
| Firebase Storage | Delete all files | `transformations/`, `anonymous/`, `avatars/` |
| Firebase Auth | Delete all users | Triggered by user deletion |

**Retain (Anonymized):**
| Collection | Action | Notes |
|------------|--------|-------|
| `paymentTransactions` | Anonymize, retain 7 years | Remove name/email, keep amount for tax records |
| `statistics` | Retain | Aggregate only, no PII |

**Implementation Options:**
1. Manual script run by admin after campaign
2. Scheduled Cloud Function triggered by date
3. Firebase Extensions for TTL-based deletion

---

### 8.4 P3: Add Webhook Signature Verification (Optional)

**Priority:** P3 (Nice to Have)
**Estimated Effort:** 2-4 hours
**Owner:** Backend Developer

**Task:** Verify webhook authenticity using provider signatures.

**For Alipay:**
```typescript
// Verify MD5 signature
const expectedSign = md5(sortedParams + alipayKey);
if (receivedSign !== expectedSign) {
  return new Response('Invalid signature', { status: 401 });
}
```

**For PayMe:**
```typescript
// Verify HMAC-SHA256 signature
const expectedSignature = hmacSha256(payload, paymeSigningKey);
if (receivedSignature !== expectedSignature) {
  return new Response('Invalid signature', { status: 401 });
}
```

**Files to Modify:**
- `src/app/api/payment-test/alipay/webhook/route.ts`
- `src/app/api/payment-test/payme/webhook/route.ts`

---

## 9. Post-Campaign Data Handling

### 9.1 Data Retention Schedule

| Milestone | Action | Responsible |
|-----------|--------|-------------|
| Campaign End (Day 60) | Stop accepting new registrations/donations | Operations |
| Campaign End + 7 days | Final reconciliation of donations | Finance |
| Campaign End + 14 days | Generate final reports (anonymized) | Operations |
| Campaign End + 30 days | Execute data deletion script | Development |
| Campaign End + 30 days | Verify deletion complete | Development |
| Campaign End + 37 days | Decommission application | Operations |

### 9.2 Data Deletion Checklist

```
[ ] Export anonymized statistics for reporting
[ ] Export donation totals (anonymized) for tax records
[ ] Run deletion script for Firestore collections
[ ] Verify Firebase Storage buckets emptied
[ ] Delete Firebase Auth users
[ ] Disable Firebase project or set to maintenance mode
[ ] Document deletion completion with timestamp
[ ] Notify CCF compliance team of completion
```

### 9.3 Exception Handling

| Scenario | Action |
|----------|--------|
| User requests data before deletion | Provide export within 40 days |
| Regulatory inquiry | Retain relevant records as required |
| Dispute on donation | Retain transaction record until resolved |

---

## 10. Appendix: Evidence References

### 10.1 Key Files Reviewed

| File | Relevance |
|------|-----------|
| `firestore.rules` | Security rules for database access |
| `storage.rules` | Security rules for file storage |
| `src/lib/firebase/types.ts` | Data schemas and structures |
| `src/lib/firebase/firestore.ts` | Database operations |
| `src/lib/firebase/auth.ts` | Authentication implementation |
| `src/components/inline-registration.tsx` | User registration form |
| `src/components/image-uploader.tsx` | Image upload component |
| `src/ai/flows/transform-photo-to-shaved-head.ts` | AI image processing |
| `src/app/api/payment-test/*/route.ts` | Payment API endpoints |
| `src/lib/firebase/payment-transactions.ts` | Payment data handling |
| `src/lib/firebase/donors.ts` | Donor data handling |

### 10.2 External References

| Resource | URL |
|----------|-----|
| CCF Privacy Policy | https://ccf.org.hk/zh-hant/disclaimer/ |
| Hong Kong PDPO | https://www.elegislation.gov.hk/hk/cap486 |
| PCPD Guidelines | https://www.pcpd.org.hk/ |
| Firebase Security Docs | https://firebase.google.com/docs/rules |

### 10.3 Assessment Limitations

This assessment is based on:
- Source code review only
- Assumptions about Firebase project configuration
- No access to production environment or logs
- No penetration testing performed

Recommendations should be validated against actual Firebase project settings.

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-01-23 | PDPOGuard Assessment | Initial assessment |

---

**END OF REPORT**
