---
title: "AI Analysis of a Low-Dose Lung CT (LDCT) Screening Study"
date: 2026-04-21
---
# AI Analysis of a Low-Dose Lung CT (LDCT) Screening Study

*A transparent demonstration of what a general-purpose multimodal LLM (ChatGPT / Claude) can and cannot do with a real DICOM imaging dataset.*

**Author:** Analysis performed by Claude Opus 4.7 (Anthropic), with cross-reference to an independent ChatGPT (GPT-4 family) analysis of the same study
**Report date:** 2026-04-21
**Study date:** 2026-04-02
**Patient identifiers:** De-identified for this report (raw `summary.txt` on the source disc contains PII; it was deliberately removed here)

---

## 1. Purpose & Audience

This report is intended for **clinicians and radiology professionals** who want an honest look at how well a general-purpose multimodal LLM performs on routine screening CT data, and where its limits are. The study used is a real low-dose lung CT (LDCT) acquired from a free community screening campaign in Hong Kong.

This is **not a diagnostic report**. It is a technical demonstration.

---

## 2. Study Metadata

Extracted directly from DICOM headers via `pydicom`.

| Field | Value |
|---|---|
| Modality | CT |
| Body part examined | CHEST |
| Study description | Thorax^CUHK_LUNG_LDCT_V2 (Adult) |
| Protocol | `CUHK_LUNG_LDCT_V2` (Chinese University of Hong Kong low-dose lung CT v2) |
| Scanner manufacturer | SIEMENS |
| Scanner model | SOMATOM Drive |
| Institution | Hong Kong Health Check |
| Study date/time | 2026-04-02, 16:08:08 local |
| Series count | 4 |
| Total instances | ~1,967 across all series |
| Total data volume | ~1.1 GB on the source DVD |

### 2.1 Series breakdown

| # | Folder | Description | Slices | Thickness | In-plane pixel | Kernel | Image type |
|---|---|---|---|---|---|---|---|
| 1 | `17310000` | `Topogram 1.0 Tr20` | 500 | — | 1.00 × 1.00 mm | — | `ORIGINAL, PRIMARY, LOCALIZER, CT_SOM5 TOP` |
| 2 | `17310001` | `LungCARE_V2_lung 0.6 Bl57 1` | 500 | 0.60 mm | 0.71 × 0.71 mm | `Bl57` (lung) | `ORIGINAL, PRIMARY, AXIAL, CT_SOM5 SPI` |
| 3 | `17310002` | `LungCARE_V2_BONE 0.6 Br57 1` | 500 | 0.60 mm | 0.71 × 0.71 mm | `Br57` (bone) | `ORIGINAL, PRIMARY, AXIAL, CT_SOM5 SPI` |
| 4 | `17310003` | `LungCARE_V2_mediast 3.0 MPR cor` | 467 | 3.00 mm | 0.99 × 0.99 mm | — | `DERIVED, PRIMARY, LOCALIZER, CT_SOM5 MPR` |

---

## 3. Methodology

### 3.1 Pipeline architecture

The AI does **not** read DICOM natively. A preprocessing pipeline renders images that the LLM's general-purpose vision layer can consume:

```
  ┌─────────────┐     ┌──────────┐     ┌─────────────────────────┐
  │ DICOM files │────▶│ pydicom  │────▶│ structured metadata     │──┐
  │ (~1,967     │     │ (I/O lib)│     │ (tags as text)          │  │
  │  slices)    │     └──────────┘     └─────────────────────────┘  │
  │             │          │                                        │
  │             │          └───▶ raw pixel arrays                   │
  │             │                       │                           │
  │             │                       ▼                           │
  │             │          ┌─────────────────────────┐              │
  │             │          │ numpy + PIL             │              │
  │             │          │ • HU rescale (slope/    │              │
  │             │          │   intercept)            │              │
  │             │          │ • windowing (WL/WW)     │              │
  │             │          │ • normalise to uint8    │              │
  │             │          └─────────────────────────┘              │
  │             │                       │                           │
  │             │                       ▼                           │
  │             │                 ┌──────────┐                      │
  │             │                 │   PNG    │                      │
  │             │                 └──────────┘                      │
  │             │                       │                           │
  └─────────────┘                       ▼                           │
                           ┌────────────────────────────┐           │
                           │ LLM multimodal vision      │◀──────────┘
                           │ (general-purpose, not      │
                           │  radiology-specialised)    │
                           └────────────────────────────┘
                                        │
                                        ▼
                              natural-language findings
```

### 3.2 Libraries used

| Component | Library | Role |
|---|---|---|
| DICOM parsing | `pydicom 3.0.2` | Reads DICOM headers and pixel data. **No interpretation.** |
| Array processing | `numpy` | HU rescale, windowing math, normalisation |
| Image encoding | `Pillow (PIL)` | Save uint8 arrays as PNG |
| Visual analysis | LLM vision (Claude 4.7 / GPT-4) | Pattern interpretation of rendered PNGs |

### 3.3 Windowing preset

Lung-window reconstructions used:
- **Window Level (WL):** −600 HU
- **Window Width (WW):** 1500 HU
- Applied after `RescaleSlope` × pixel + `RescaleIntercept` conversion to HU.

Same reasoning applied implicitly by the viewer for Series 3 (bone window) via its saved reconstruction kernel `Br57`.

### 3.4 Sampling strategy

| Sample | Coverage |
|---|---|
| 1 middle slice per series | Series 1, 3, 4 (one each) — coarse orientation |
| 9 evenly-spaced slices | Series 2 only, at indices 49/99/149/199/249/299/349/399/449 of 500 (~10/20/…/90%), sorted by `ImagePositionPatient` z-axis so the stack runs base → apex |
| **Total sampled** | 12 PNG slices out of 1,967 total instances (~0.6%) |

Out of the full volume that a radiologist would scroll through in a few seconds, this report examines **0.6% of the data**. That single fact sets a hard ceiling on the sensitivity of this analysis.

### 3.5 Reproducibility

Scripts and outputs are preserved:

| Artefact | Path |
|---|---|
| Metadata extractor | `~/Downloads/DICOM_for_ChatGPT/dicom_study_summary.py` |
| Slice extractor | `~/Downloads/DICOM_for_ChatGPT/extract_slices.py` |
| Python environment | `~/Downloads/DICOM_for_ChatGPT/.venv/` |
| Extracted 4-series middle slices | `~/Downloads/dicom_output/previews/*.png` |
| Series 2 base→apex walkthrough | `~/Downloads/dicom_output/series2_walkthrough/s2_01..09.png` |
| Machine-readable summary | `~/Downloads/dicom_output/summary.json` |

---

## 4. AI Visual Findings

All observations below are the **LLM's own reading of the rendered PNGs**, with no access to priors, no scrolling, no measurement tools, no windowing changes.

### 4.1 Orientation series

| Series | Level sampled | Observation |
|---|---|---|
| Series 1 (topogram) | Middle | Axial soft-tissue-window slice showing heart silhouette and flanking lung fields. Served scout/planning role; not used for diagnostic reading. |
| Series 3 (bone window, middle slice) | Mid-thorax | Ribs continuous and intact at this level. Vertebral body and spinous process normal. Sternum visible anteriorly. No obvious lytic or sclerotic lesions at the sampled level. |
| Series 4 (coronal MPR, middle slice) | Mid-coronal plane | Spine stacks cleanly midline. Symmetric chest outline. Dark vertical bars at image edges are scanner table / gantry artefacts, not anatomy. |

### 4.2 Series 2 — lung-window base → apex walkthrough (9 slices)

| Slice | Position | AI observation |
|---|---|---|
| 1 | 10% (idx 49) — upper abdomen | Mostly sub-diaphragmatic; liver dominates R, stomach/spleen on L. Lung-base slivers visible at top. Left upper-abdomen dark pockets = gastric/bowel gas (normal). No lung-base findings to flag. |
| 2 | 20% (idx 99) — lung bases + liver dome | Lower-lobe crescents visible around diaphragm. Right hemidiaphragm higher than left (normal liver effect). Normal vascular markings at bases. No focal consolidation, no pleural effusion visible, no nodules. |
| 3 | 30% (idx 149) — lower thorax / heart | Heart central; lungs symmetric and well-aerated flanking the heart. Vessels taper smoothly from hila outward. No focal densities. |
| 4 | 40% (idx 199) — mid-chest | Heart chambers differentiated internally. Lung fields clean. Normal vascular branching. No asymmetry, no lobar collapse pattern. |
| 5 | 50% (idx 249) — carina region | Main airways (carina, main bronchi) visible centrally. Hilar pulmonary arteries + bronchi symmetric left/right. No hilar enlargement or mass effect. Parenchyma clean. |
| 6 | 60% (idx 299) — upper-mid lung | Trachea central. Upper-lobe lung with normal vascular pattern. No nodules, no focal opacities. |
| 7 | 70% (idx 349) — upper lung | Trachea clearly central. Upper lobes symmetric, well-aerated. Faint streak patterns near dense structures interpreted as routine beam-hardening/motion artefacts. |
| 8 | 80% (idx 399) — high upper lung | Lung cross-section tapering toward apex. Bony thorax (ribs) prominent. Parenchyma dark and clean; no apical scarring or pleural thickening flagged. |
| 9 | 90% (idx 449) — apex / thoracic inlet | Small apical lung regions. Dominated by thoracic-inlet structures: trachea, neck/shoulder soft tissue, clavicles, shoulder joints. Both lung apices appear clear; no apical masses, cavities, or pleural thickening flagged. |

### 4.3 Aggregate read (Series 2, 9-slice sampling)

- **Aeration:** uniformly dark (well-aerated) from base to apex, bilaterally; **symmetric** at every sampled level
- **Vascular pattern:** normal-looking branching, smooth tapering from hila to periphery at every level
- **Airways:** trachea and main bronchi patent and central throughout
- **No AI-flagged findings:** no clear solid nodules > ~5–8 mm, no obvious consolidation, no lobar collapse, no bullae/cystic change, no honeycomb fibrosis pattern, no obvious pleural effusion, no apical scarring

**This is consistent with the negative binary screening result returned by the campaign.** It is not independent confirmation of it.

---

## 5. Known Limitations of This Approach

| # | Limitation | Clinical implication |
|---|---|---|
| 1 | **8-bit PNG input** | LLM sees ~256 gray levels vs DICOM's ~4096. Subtle density differences (e.g., ground-glass opacity) are compressed. |
| 2 | **No volumetric viewing** | LLM sees isolated static slices. Radiologists scroll 500+ slices continuously to track structures across planes. |
| 3 | **Sampled (~0.6%) coverage** | A 6 mm nodule between sample points is invisible. |
| 4 | **Single window per pass** | Radiologists switch windows fluidly. LLM gets one pre-rendered window per request. |
| 5 | **No priors / serial comparison** | Growth is the dominant signal in screening. LLM has none. |
| 6 | **No measurement tools** | LLM estimates sizes by appearance; cannot quantify in millimetres against a calibrated ruler. |
| 7 | **No radiology-specific supervision** | Model was trained on general web imagery + broad text; it has seen medical images but not at the depth/rigour of a radiology-trained CNN. |
| 8 | **No regulatory clearance** | Not CE-marked, not FDA-cleared, not a certified medical device. |
| 9 | **Hallucination risk** | LLMs can confidently confabulate findings. Cross-checking between models (ChatGPT vs Claude) and against ground truth is essential. |

---

## 6. Context: General-Purpose LLM vs Specialist Radiology AI

| Aspect | ChatGPT / Claude (this report) | Specialist radiology AI (e.g., Aidoc, Lunit INSIGHT CXR, Siemens AI-Rad Companion) |
|---|---|---|
| Input format | Rendered 8-bit PNG (via pydicom pipeline) | Native DICOM, full bit depth |
| Model architecture | General multimodal transformer | Task-specific CNN / transformer trained on labelled radiology studies |
| Training data | Web-scale text + images (incl. medical) | Millions of radiologist-labelled studies |
| Output | Natural-language description | Structured findings + measured confidence + bounding boxes |
| Regulatory status | Not a medical device | CE / FDA cleared for specific indications |
| Typical use | Orientation, education, patient literacy | Triage, worklist prioritisation, second-reader |
| Sensitivity to small nodules | Low-to-moderate at best | High (target-designed) |
| Hallucination risk | Present | Very low (task-bounded) |

The two are complementary, not interchangeable. General LLMs make medical imaging **accessible** to laypeople and educators; specialist AI provides **clinically useful** automated reads under regulatory supervision.

---

## 7. What a General-Purpose LLM *Is* Useful For Here

Despite the limits above, this workflow has genuine value:

1. **Patient-facing education.** A participant in a free screening programme who gets only a binary result can get a meaningful plain-language orientation to what was scanned and what was looked for.
2. **DICOM/data literacy.** The metadata-level explanation (series, kernels, windows, HU) teaches the imaging pipeline itself, useful for medical students, data scientists, and technically-minded patients.
3. **Triage of the user's own curiosity.** "Should I ask for a follow-up?" kinds of questions can be framed against what the AI thinks is gross-level normal vs abnormal.
4. **Research / demonstration.** Transparent comparisons like this one inform what generalist AI can and cannot contribute to radiology workflows.

---

## 8. Disclaimer

This report was produced by a general-purpose LLM without radiological training, without regulatory clearance, and without access to priors. It **does not** constitute a clinical diagnosis, a second-reader opinion, or any form of medical advice. Any concern about a patient's imaging must be directed to a qualified radiologist reading the original DICOM study in a certified workstation.

---

## Appendix A — Reproducibility commands

```bash
# Metadata + 3 representative PNGs per series
python dicom_study_summary.py \
  --root ~/Downloads/DVD_2604020417 \
  --out  ~/Downloads/dicom_output

# 9 evenly-spaced lung-window slices from Series 2
python extract_slices.py \
  --series ~/Downloads/DVD_2604020417/DICOM/26040209/17310001 \
  --out    ~/Downloads/dicom_output/series2_walkthrough \
  --n 9 --window lung --prefix s2
```

## Appendix B — Windowing reference used by the extractor

| Preset | WL (HU) | WW (HU) | Used for |
|---|---|---|---|
| lung | −600 | 1500 | Parenchymal evaluation |
| mediastinum | 40 | 400 | Vessels, soft tissue, nodes |
| bone | 500 | 2000 | Osseous detail |
| soft | 40 | 400 | Generic soft tissue |
| brain | 40 | 80 | CNS (not applicable here) |
