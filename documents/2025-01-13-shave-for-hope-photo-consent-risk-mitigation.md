---
title: "Shave for Hope - Photo Consent & PR Risk Mitigation"
tags: [reference, development, web-development, business, actionable, high-priority]
date: 2026-01-13
type: reference
status: processing
priority: high
project: shave-for-hope
---

# Shave for Hope - Photo Consent & PR Risk Mitigation

## 💡 Core Problem

Users could upload photos of other people without consent (celebrities, ex-partners, colleagues), transform them to show a "shaved head," and share publicly — creating serious PR and legal risk for the Childhood Cancer Foundation (CCF).

## 🚫 Why Camera-Only Doesn't Work

| Constraint | Reason |
|------------|--------|
| **HTML `capture` attribute** | Only a *hint*, not enforcement. Users can dismiss camera and pick from gallery |
| **No browser API blocks gallery** | By design — browsers prioritize user choice |
| **getUserMedia only** | Excludes desktop users, requires HTTPS, spotty mobile support |
| **Physical bypass** | User can photograph a screen or printed photo |

**The real issue is consent verification, not input method.**

## 🔄 Risk Perspective: Why Abuse May Be Limited

| Factor | Rationale |
|--------|-----------|
| **Camera bypass is trivial** | Even with camera-only, users can point the camera at a celebrity photo on another screen or print. Technical restrictions don't prevent intent. |
| **AI tools are ubiquitous** | Image transformation is easily done with many public AI tools (Midjourney, Stable Diffusion, etc.). Users with malicious intent would likely use those tools instead — no friction, no consent checkbox, no accountability. |
| **IG sharing is voluntary** | The suggested sharing message is optional. Users may: (1) transform without sharing at all, (2) share without our message, (3) use other tools and voluntarily include our campaign. **There is no direct technical binding between our app and any Instagram post.** |

**Implication**: Bad actors have easier, anonymous alternatives. Our tool's friction (login, consent, watermark) may actually make us a *less attractive* vector for abuse compared to open AI tools.

## ⚠️ Risk Scenarios

| Scenario | Impact | Likelihood |
|----------|--------|------------|
| Celebrity photo transformed & shared | High — media attention, legal | Medium |
| Revenge/harassment (ex, colleague) | High — victim complaints, legal | Medium |
| Political figure mockery | High — controversy | Low-Medium |
| Stock photo abuse | Low — looks fake | Low |

## 🛡️ Mitigation Strategies

### Tier 1: Friction-Based Deterrence (Low Effort)

| Strategy | Effect |
|----------|--------|
| **Require login** | Creates accountability trail |
| **Consent checkbox** | "I confirm this is my own photo" — legal cover |
| **Watermark outputs** | Deters sharing if watermarked |
| **Disable public sharing** | Keep transformations private by default |

### Tier 2: Technical Detection (Medium Effort)

| Strategy | Effect |
|----------|--------|
| **Selfie liveness check** | Require blink/smile/head turn before capture |
| **EXIF metadata check** | Flag photos taken >24hrs ago or from different device |
| **Reverse image search** | Check against Google Images for public figures |
| **Face comparison** | If user has profile photo, compare faces match |

### Tier 3: Process Controls (Operational)

| Strategy | Effect |
|----------|--------|
| **Moderation queue** | Review before allowing public share |
| **Report mechanism** | Let victims flag unauthorized use |
| **Delayed visibility** | 24hr delay before shareable |
| **Terms of service** | Clear liability transfer to user |

## 🎯 Recommendation

**Minimum viable protection:**

1. ✅ **Mandatory login** — No anonymous transformations
2. ✅ **Consent checkbox** — "I confirm I have the right to use this photo"
3. ✅ **Disable direct social sharing** — User downloads image, shares manually
4. ✅ **Clear ToS** — User accepts liability for misuse
5. ✅ **Report button** — On any publicly visible transformation

**If budget allows:**

6. 🔄 **Liveness detection** — Camera captures require blink/movement
7. 🔄 **Moderation queue** — Human review before public gallery

## 📊 Trade-off Matrix

| Protection Level | Abuse Risk | User Friction | Viral Potential |
|------------------|------------|---------------|-----------------|
| Current (open) | High | Low | High |
| Login + consent | Medium | Medium | Medium |
| Liveness + moderation | Low | High | Low |

**For a charity campaign, reputation protection > viral growth.**

## 📝 Next Steps

- [ ] Implement mandatory login for all transformations
- [ ] Add consent checkbox with legal language to upload form
- [ ] Review and update Terms of Service
- [ ] Add report mechanism to public gallery/share pages
- [ ] Evaluate removing or gating public sharing features
- [ ] Consult with CCF legal team on liability language

## 🏷️ Tags Analysis

**Content Analysis:**
- **Type**: `reference` (Technical decision document)
- **Topics**: `development`, `web-development`, `business` — covers technical implementation and organizational risk
- **Characteristics**: `actionable`, `high-priority` — requires immediate attention before launch
- **Priority**: `high` — PR crisis prevention is critical for charity reputation

**Suggested Bases Filters:**
- Find project notes: `project = "shave-for-hope"`
- Find high-priority actionable: `tags contains "actionable" AND priority = high`
- Find security/risk topics: `tags contains "business" AND tags contains "development"`

---

**Captured**: 2025-01-13
**Status**: processing
**Next Action**: Review with CCF stakeholders, implement minimum viable protection
