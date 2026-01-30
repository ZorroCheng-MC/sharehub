---
title: 剃亮希望 Shave for Hope - Project Schedule
date: 2026-01-30
type: reference
status: processing
priority: high
project: shave-for-hope
related: "[[2025-12-31-ccf-shave-for-hope-firebase-studio-project]]"
tags:
  - project
  - CCF
  - schedule
  - actionable
  - high-priority
read: true
---

# 剃亮希望 Shave for Hope
## Project Schedule (Jan 30, 2026)

> 📋 **Main Project Plan:** [[2025-12-31-ccf-shave-for-hope-firebase-studio-project]]

---

## Key Milestones

| Date | Day | Milestone |
|------|-----|-----------|
| **Feb 2** | Sun | Asiapay test gateway ready |
| **Feb 3** | Mon | Mockup review complete |
| **Feb 3** | Mon | GCP credit ready |
| **Mar 1** | Sun | 🚀 **Official Launch** |
| **Mar 14** | Sat | 📅 **Head Shaving Day** (中環街市) |

---

## Schedule Overview

### Gantt Chart

```mermaid
gantt
    title 剃亮希望 Shave for Hope - Project Schedule
    dateFormat  YYYY-MM-DD
    tickInterval 7day

    section Development
    Payment Integration (Asiapay)    :pay, 2026-02-02, 5d
    Video Generation (Veo 3.1)       :video, 2026-02-03, 3d
    English Pages (i18n)             :i18n, 2026-02-03, 3d
    Bug Fixes & Polish               :bugs, 2026-02-07, 4d

    section Review
    Mockup Review                    :milestone, mockup, 2026-02-03, 1d
    Service Team Review              :service, 2026-02-11, 7d

    section UAT
    UAT Round 1                      :uat1, 2026-02-18, 3d
    UAT 1 Fixes                      :fix1, 2026-02-21, 2d
    UAT Round 2                      :uat2, 2026-02-23, 3d
    UAT 2 Fixes                      :fix2, 2026-02-26, 2d

    section Launch
    Final Deploy                     :deploy, 2026-02-28, 1d
    Official Launch                  :milestone, crit, launch, 2026-03-01, 1d
    Soft Launch Period               :soft, 2026-03-01, 13d
    Head Shaving Day                 :milestone, crit, event, 2026-03-14, 1d
```

### Timeline Summary

| Week | Dates | Focus |
|------|-------|-------|
| W1 | Feb 2-8 | Development (Payment, Video, English) |
| W2 | Feb 9-15 | Bug Fixes + Service Team Review |
| W3 | Feb 16-22 | UAT Round 1 + Fixes |
| W4 | Feb 23-28 | UAT Round 2 + Deploy |
| W5-6 | Mar 1-14 | Soft Launch → Head Shaving Day |

---

## Detailed Schedule

### Phase 1: Development (Feb 2-10)

| Task | Start | End | Days | Owner | Dependencies |
|------|-------|-----|------|-------|--------------|
| Payment Integration (Asiapay) | Feb 2 | Feb 6 | 5 | Dev | Asiapay gateway ready |
| Video Generation (Veo 3.1) | Feb 3 | Feb 5 | 3 | Dev | GCP credit ready |
| English Page (i18n) | Feb 3 | Feb 5 | 3 | Dev | - |
| Bug Fixes & Polish | Feb 7 | Feb 10 | 4 | Dev | Core features complete |

**Note:** Payment, Video, and English can run in parallel.

---

### Phase 2: Review (Feb 3-17)

| Task | Start | End | Days | Owner | Dependencies |
|------|-------|-----|------|-------|--------------|
| Mockup Review | Feb 3 | Feb 3 | 1 | CCF + Dev | - |
| Service Team Review | Feb 11 | Feb 17 | 7 | CCF Service | After mockup review |

---

### Phase 3: UAT (Feb 18-27)

| Task | Start | End | Days | Owner | Dependencies |
|------|-------|-----|------|-------|--------------|
| UAT Round 1 | Feb 18 | Feb 20 | 3 | CCF | Service review complete |
| UAT 1 Bug Fixes | Feb 21 | Feb 22 | 2 | Dev | UAT 1 feedback |
| UAT Round 2 | Feb 23 | Feb 25 | 3 | CCF | UAT 1 fixes deployed |
| UAT 2 Bug Fixes | Feb 26 | Feb 27 | 2 | Dev | UAT 2 feedback |

---

### Phase 4: Launch (Feb 28 - Mar 14)

| Task | Start | End | Days | Owner | Notes |
|------|-------|-----|------|-------|-------|
| Final Deploy & Smoke Test | Feb 28 | Feb 28 | 1 | Dev | Production deployment |
| 🚀 **Official Launch** | **Mar 1** | - | - | All | Public announcement |
| Soft Launch / Marketing | Mar 1 | Mar 13 | 13 | CCF Marketing | Viral growth period |
| Hotfix Support | Mar 1 | Mar 13 | 13 | Dev | On-call support |
| 📅 **Head Shaving Day** | **Mar 14** | - | - | CCF | 中環街市 |

---

## Phase Summary

| Phase | Duration | Dates | Status |
|-------|----------|-------|--------|
| Development | 9 days | Feb 2-10 | ⏳ Pending |
| Review | 15 days | Feb 3-17 | ⏳ Pending |
| UAT | 10 days | Feb 18-27 | ⏳ Pending |
| Deploy | 1 day | Feb 28 | ⏳ Pending |
| Soft Launch | 13 days | Mar 1-13 | ⏳ Pending |
| **Total** | **41 days** | Feb 2 - Mar 14 | - |

---

## Critical Path

```mermaid
flowchart TD
    subgraph EXTERNAL["External Dependencies"]
        A1[🔌 Asiapay Gateway<br/>Feb 2]
        A2[💳 GCP Credit<br/>Feb 3]
    end

    subgraph DEV["Development Phase"]
        B[💳 Payment Integration<br/>5 days]
        C[🎬 Video Generation<br/>3 days]
        D[🌐 English Pages<br/>3 days]
        E[🔧 Bug Fixes<br/>4 days]
    end

    subgraph REVIEW["Review Phase"]
        F[👀 Service Team Review<br/>7 days]
    end

    subgraph UAT["UAT Phase"]
        G[🧪 UAT Round 1<br/>3 days]
        H[🔨 UAT 1 Fixes<br/>2 days]
        I[🧪 UAT Round 2<br/>3 days]
        J[🔨 UAT 2 Fixes<br/>2 days]
    end

    subgraph LAUNCH["Launch"]
        K[🚀 Deploy<br/>Feb 28]
        L[✨ Official Launch<br/>Mar 1]
        M[📅 Head Shaving Day<br/>Mar 14]
    end

    A1 --> B
    A2 --> C
    B --> E
    C --> E
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J --> K
    K --> L
    L --> M

    style L fill:#F5A623,stroke:#333,stroke-width:2px
    style M fill:#4A90D9,stroke:#333,stroke-width:2px
```

**Critical Path Duration:** 27 days (Feb 2 → Feb 28)

---

## Parallel Tracks

| Track | Tasks | Dates | Notes |
|-------|-------|-------|-------|
| **Track A: Payment** | Asiapay Integration | Feb 2-6 | Blocks video unlock feature |
| **Track B: AI** | Video Generation | Feb 3-5 | Depends on GCP credit |
| **Track C: i18n** | English Pages | Feb 3-5 | Independent |
| **Track D: Review** | Service Team | Feb 11-17 | Can start after mockup |

---

## Dependencies & Blockers

| Item | Blocker | Ready Date | Impact |
|------|---------|------------|--------|
| Asiapay Test Gateway | External | Feb 2 | Blocks payment dev |
| GCP Credit | External | Feb 3 | Blocks video generation |
| Mockup Review | CCF | Feb 3 | Blocks service review |
| Service Team Availability | CCF | Feb 11 | Blocks UAT |

---

## Risk Buffers

| Buffer | Days | Purpose |
|--------|------|---------|
| Dev contingency | 3 days (Feb 7-10) | Overflow from parallel dev |
| Pre-launch | 1 day (Feb 28) | Final testing |
| **Soft launch** | **13 days** (Mar 1-13) | Marketing ramp-up, hotfixes |

---

## Deliverables Checklist

### Before Service Review (Feb 10)

- [ ] Payment integration complete (Asiapay: PayMe, Alipay, Credit Card)
- [ ] Video generation working
- [ ] English pages translated
- [ ] All known bugs fixed

### Before UAT 1 (Feb 17)

- [ ] Service team sign-off
- [ ] Test accounts prepared
- [ ] UAT checklist ready

### Before Launch (Feb 28)

- [ ] UAT 2 passed
- [ ] Production environment ready
- [ ] Domain configured (shaveforhope.ccf.org.hk)
- [ ] Analytics configured
- [ ] Monitoring enabled

### Launch Day (Mar 1)

- [ ] Production deployed
- [ ] Smoke test passed
- [ ] Social media announcement ready
- [ ] Support team briefed

---

## Team Responsibilities

| Role              | Phase 1 (Dev)       | Phase 2 (Review) | Phase 3 (UAT)     | Phase 4 (Launch) |
| ----------------- | ------------------- | ---------------- | ----------------- | ---------------- |
| **Dev Team**      | Build features      | Support review   | Fix bugs          | Deploy & support |
| **CCF Service**   | -                   | Review mockup    | Conduct UAT       | -                |
| **CCF Marketing** | -                   | -                | Prepare materials | Launch campaign  |
| **Google**        | Provide credentials | -                | -                 | DNS setup        |

---

## Contact & Escalation

| Role                | Contact                     | Escalation                  |
| ------------------- | --------------------------- | --------------------------- |
| Dev Lead            | zorro.cheng@hkmci.com       | dennis.wong@hkmci.com       |
| CCF Project Manager | kenny.lok@ccf.org.hk        | magdalena.cheung@ccf.org.hk |
| CCF Service Lead    | magdalena.cheung@ccf.org.hk | ben.wong@google.com         |

---

*Schedule Version: 1.0 | Created: 2026-01-30 | Launch: Mar 1, 2026 | Event: Mar 14, 2026*
