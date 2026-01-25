---
title: "KnowledgeFactory Master Plan"
tags: [product, marketing, strategy, KnowledgeFactory, enterprise]
date: 2026-01-25
type: reference
status: active
priority: high
---

# KnowledgeFactory Master Plan

> Strategic plan for restructuring KnowledgeFactory product line under the `knowledgefactory` GitHub organization.

**Company**: crewnest.ai (TBD)
**Product Line**: KnowledgeFactory
**Document Version**: 1.0
**Last Updated**: 2026-01-25

---

## Executive Summary

KnowledgeFactory is an AI-powered knowledge manufacturing system that transforms raw information into organized, shareable insights. This document outlines the restructuring of all products under a unified GitHub organization with clear branding for free (self-serve) and enterprise (paid support) tiers.

### Key Decisions

| Decision | Choice |
|----------|--------|
| GitHub Organization | `knowledgefactory` |
| Company Brand | crewnest.ai (subtle: "By crewnest.ai") |
| Free Tier Name | KnowledgeFactory |
| Paid Tier Name | KnowledgeFactory Enterprise (KFE) |
| Claude Code Users | Free only (word of mouth) |
| OpenCode Users | Free (DIY) OR KFE (paid support + Vertex AI) |
| Repo Naming | Short names: `kf-opencode`, `kf-claude`, etc. |

---

## Brand Hierarchy

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CREWNEST.AI                                     │
│                           (Company Brand)                                    │
│                     Displayed as: "By crewnest.ai"                          │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           KNOWLEDGEFACTORY                                   │
│                          (Product Line)                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   ┌─────────────────────────────┐    ┌─────────────────────────────┐        │
│   │   KnowledgeFactory          │    │   KnowledgeFactory          │        │
│   │   (Free / Self-Serve)       │    │   Enterprise (KFE)          │        │
│   │                             │    │   (Paid Support)            │        │
│   ├─────────────────────────────┤    ├─────────────────────────────┤        │
│   │ • Open source tools         │    │ • Same open source tools    │        │
│   │ • Community support         │    │ • Pre-configured Vertex AI  │        │
│   │ • DIY installation          │    │ • Setup assistance          │        │
│   │ • GitHub issues only        │    │ • Priority support          │        │
│   │ • BYOK (bring your own key) │    │ • Flat fee pricing          │        │
│   └─────────────────────────────┘    └─────────────────────────────┘        │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Product Matrix

### Products Overview

| Product | Description | Platforms | Status |
|---------|-------------|-----------|--------|
| **kf-opencode** | OpenCode skill package | CLI (all OS) | Active |
| **kf-claude** | Claude Code plugin | CLI (all OS) | Active |
| **DoubleCopy** | Desktop clipboard capture | macOS | v1.1 |
| **Capture Dude** | Mobile capture app | iOS, Android | Planned |
| **Sharehub** | Publishing platform | Web (GitHub Pages) | Active |

### CLI + Tier Matrix

|  | KnowledgeFactory (Free) | KnowledgeFactory Enterprise |
|---|---|---|
| **Claude Code** | ✅ Free only (word of mouth) | ❌ No paid version |
| **OpenCode** | ✅ Free (DIY, self-serve) | ✅ Paid (Vertex AI + support) |

### Product-Tier Mapping

| Product | KF (Free) | KFE (Paid) | Notes |
|---------|-----------|------------|-------|
| kf-opencode | ✅ | ✅ | Same code, different support |
| kf-claude | ✅ | ❌ | Free only, word of mouth |
| DoubleCopy | ✅ | ✅ | Same app |
| Capture Dude | ✅ | ✅ | Same app |
| Sharehub | ✅ | ✅ | Same platform |
| Vertex AI API | ❌ BYOK | ✅ Included | Key differentiator |
| Setup Assistance | ❌ | ✅ | crewnest.ai support |
| Priority Support | ❌ | ✅ | crewnest.ai support |

---

## User Personas

### Persona A: Claude Code Power User

- **Profile**: Existing Claude Pro/Max subscriber, familiar with Obsidian
- **CLI**: Claude Code
- **Tier**: KnowledgeFactory (Free only)
- **Value to us**: Word of mouth, community growth
- **Journey**: Discover → Install kf-claude → Use → Recommend to others

### Persona B: Technical DIY User

- **Profile**: Developer, comfortable with CLI, wants free/open source
- **CLI**: OpenCode
- **Tier**: KnowledgeFactory (Free)
- **Value to us**: Community growth, potential KFE upgrade
- **Journey**: Discover → Install kf-opencode (DIY) → Struggle? → Maybe upgrade to KFE

### Persona C: Business/Team User

- **Profile**: Wants it to "just work", willing to pay for convenience
- **CLI**: OpenCode
- **Tier**: KnowledgeFactory Enterprise
- **Value to us**: Revenue (flat fee + Vertex AI resale)
- **Journey**: Discover → Contact sales → KFE onboarding → Ongoing support

---

## User Journey

```
                         ┌─────────────────┐
                         │   Discovers     │
                         │ KnowledgeFactory│
                         └────────┬────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              ▼                   ▼                   ▼
    ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
    │ "I have Claude  │ │ "I can DIY,     │ │ "I want it to   │
    │  Pro/Max"       │ │  give me free"  │ │  just work"     │
    └────────┬────────┘ └────────┬────────┘ └────────┬────────┘
             │                   │                   │
             ▼                   ▼                   ▼
    ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
    │   kf-claude     │ │   kf-opencode   │ │   KFE           │
    │   (FREE)        │ │   (FREE DIY)    │ │   (PAID)        │
    │                 │ │                 │ │                 │
    │ • Plugin install│ │ • Script install│ │ • Contact sales │
    │ • /kf-claude:*  │ │ • Short /capture│ │ • Onboarding    │
    │ • Word of mouth │ │ • GitHub issues │ │ • Vertex AI API │
    └────────┬────────┘ └────────┬────────┘ └────────┬────────┘
             │                   │                   │
             │                   │    ┌──────────────┘
             │                   │    │ "This is hard,
             │                   │    │  I need help"
             │                   │    ▼
             │                   │ ┌─────────────────┐
             │                   └▶│ Upgrade to KFE  │
             │                     └─────────────────┘
             │
             └──────────────────────────────────────────┐
                                                        │
                         ┌──────────────────────────────┘
                         ▼
              ┌─────────────────────────────────────────┐
              │  OPTIONAL ADD-ONS (All users)           │
              ├─────────────────────────────────────────┤
              │  • DoubleCopy (macOS desktop capture)   │
              │  • Capture Dude (iOS/Android mobile)    │
              │  • Sharehub (publishing platform)       │
              └─────────────────────────────────────────┘
```

---

## Revenue Model

### KnowledgeFactory (Free)

- **Revenue**: $0
- **Value**: Community growth, word of mouth, ecosystem adoption
- **Cost**: Minimal (GitHub hosting, community support time)

### KnowledgeFactory Enterprise

- **Revenue**: Flat fee (monthly/annual TBD)
- **Components**:
  - Vertex AI API access (crewnest.ai is Google Vertex AI reseller)
  - Setup assistance (1:1 onboarding)
  - Priority support (email/chat)
- **Margin**: Vertex AI resale margin + support fee

### Future Revenue Opportunities

- Exclusive KFE features (if project succeeds)
- Team/organization plans
- Custom integrations
- Training/consulting

---

## Marketing Strategy

### GitHub (Free Users)

- README focuses on free, open source, DIY
- Mentions KFE for those who need help
- Technical documentation
- Community-driven support (issues, discussions)

### Landing Page (KFE Priority)

- Hero section highlights KFE value proposition
- Pricing comparison (Free vs Enterprise)
- Contact sales CTA prominent
- Free version available but secondary

### App Stores

- Capture Dude branded as "Part of KnowledgeFactory"
- Links to landing page
- Free app (captures feed into ecosystem)

---

## Command Experience

### Claude Code Users (Plugin Standard)

```bash
# Install
/plugin marketplace add knowledgefactory/kf-claude
/plugin install kf-claude

# Use (plugin-prefixed)
/kf-claude:capture https://youtube.com/watch?v=abc
/kf-claude:idea Build a browser extension

# Opt-in short commands
/kf-claude:setup --enable-short-commands
# After: /capture, /idea, etc.
```

### OpenCode Users (Short Commands Default)

```bash
# Install
curl -fsSL https://raw.githubusercontent.com/knowledgefactory/kf-opencode/main/install.sh | bash

# Use (short commands)
/capture https://youtube.com/watch?v=abc
/idea Build a browser extension
/publish my-note.md
```

---

## KFE Onboarding Flow

```
┌─────────────────────────────────────────────────────────────────┐
│  KFE CUSTOMER ONBOARDING                                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. Customer contacts crewnest.ai                               │
│                    ↓                                             │
│  2. Sales call / signup for flat fee                            │
│                    ↓                                             │
│  3. crewnest.ai provisions:                                     │
│     • Vertex AI credentials                                      │
│     • Pre-configured opencode.json                              │
│     • Pre-configured MCP server settings                        │
│                    ↓                                             │
│  4. Setup assistance (1:1 or guided)                            │
│     • Install OpenCode                                           │
│     • Install kf-opencode                                        │
│     • Configure vault                                            │
│     • Test capture workflow                                      │
│                    ↓                                             │
│  5. Ongoing support                                              │
│     • Priority email/chat                                        │
│     • Troubleshooting                                            │
│     • Feature requests                                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Success Metrics

### Free Tier (KnowledgeFactory)

| Metric | Target |
|--------|--------|
| GitHub stars (all repos) | 1,000+ |
| Monthly active installs | 500+ |
| Community contributors | 20+ |
| Word of mouth referrals | Track via surveys |

### Paid Tier (KFE)

| Metric | Target |
|--------|--------|
| Paying customers | 10+ (initial) |
| Monthly recurring revenue | $X (TBD) |
| Customer retention | 90%+ |
| NPS score | 50+ |

---

## Timeline

| Phase | Actions | Target |
|-------|---------|--------|
| **Phase 1** | Create GitHub org, transfer repos, restructure | Week 1-2 |
| **Phase 2** | Update READMEs, create landing page | Week 2-3 |
| **Phase 3** | KFE onboarding flow, Vertex AI integration | Week 3-4 |
| **Phase 4** | Marketing launch, announce restructuring | Week 4-5 |
| **Phase 5** | Capture Dude mobile app release | Q2 2026 |

---

## Related Documents

- [[KF-GITHUB-STRUCTURE]] - Detailed repo structure
- [[KF-MIGRATION-CHECKLIST]] - Step-by-step migration actions
- [[KF-README-TEMPLATES]] - README templates for each repo
- [[KF-LANDING-PAGE-SPEC]] - Landing page wireframe and content

---

*KnowledgeFactory Master Plan v1.0 | By crewnest.ai*
