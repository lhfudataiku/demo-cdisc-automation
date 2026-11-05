# Scope Baseline

> **Decision:** re-baseline the asset to the scope the project delivers today.
> **Date:** 2026-11-05 · **Applies to:** Dataiku project `CLIN_FLU2026_VISUAL`, its Govern workflow, the deck and this documentation set.
> **Replaces:** the scope in the original scoping document *"Next-Gen CDISC Automation (Dataiku-Enabled)"* (§3–§9).
> **Audience:** internal (asset team, presenters). Prospect-facing scope is in [01 · Overview](01-overview.md).

---

## 1. Why re-baseline

The asset is an **MVP conversation starter** for pharma clinical teams evaluating a self-hosted SAS replacement. Time to invest is limited.

The original scoping document described a full CRF-to-submission programme: API ingestion, raw → SDTM mapping driven by a standards library, Define-XML, a parallel code-validation path, dashboards and QMS integration. The project delivers a narrower, working slice: a **governed SDTM-validated → ADaM → TLF/XPT pipeline**.

Rather than build the missing pillars, the scope is reset to what exists, and everything else becomes roadmap and discovery material. This keeps every claim true and turns gaps into questions for the prospect.

---

## 2. Baseline scope statement

### In scope

| Capability | Delivered by |
|---|---|
| **SDTM lock & conformance:** SDTM → XPT v5, CDISC CORE (SDTMIG 3.4), data and code fingerprints | `build_sdtm` |
| **SDTM sign-off** in Govern (gate 1) | Govern workflow step *SDTM Validation* |
| **ADaM derivation:** ADSL, ADAE, ADREACT (visual only), ADVS, ADIS (visual + `admiral` baseline) | Flow zones 2–6 |
| **In-flow ADaM QC:** 26 consistency checks | `validate_adam` |
| **ADaM XPT v5 export** | `xpt_export` (`xportr`) |
| **ADaM conformance with Pinnacle 21 Community** (manual run; result and report recorded in Govern) | Presenter / reviewer |
| **ADaM sign-off** in Govern (gate 2) | Govern workflow step *ADaM Validation* |
| **Gated release** of the XPT package and 4 summary tables (TLF package) | `release_submission` |
| **Version-to-data binding:** automatic revocation of approvals when SDTM data or pipeline code changes | Gate scripts, `governance_integrity_check` |
| **Operator experience:** 7-step app homepage, project wiki, this documentation set | App template, `docs/` |

### Boundaries and assumptions

| Boundary | Baseline position |
|---|---|
| Input | **Validated-ready SDTM** (6 domains) supplied from upstream. The asset doesn't create SDTM. |
| Data | One synthetic Phase II vaccine study: single arm, 50 subjects, no blinding. Not for submission. |
| Regulator focus | US FDA. PMDA specifics aren't covered. |
| Deployment target | Self-managed Dataiku (Design → Automation, plus Govern). Developed on Dataiku Cloud. |
| Licences | Govern node with Advanced Govern (to be confirmed with Dataiku) |
| Signatures | Govern sign-offs are **governance approvals**, not Part 11 electronic signatures. The customer's e-signature system can be linked through the Govern REST API (roadmap). |
| Platform validation | Customer responsibility (IQ/OQ/PQ). The asset provides controls, not a validated system. |
| Studies | One study per app instance, each with its own Govern record |

---

## 3. Scope change register

Every commitment in the original scoping document, and what the baseline does with it.

| Scoping § | Original commitment | Baseline | Rationale / note |
|---|---|---|---|
| §1 | "100% GxP compliance" | **Reworded** | Compliance is the customer's; the asset *supports* GxP controls (Dataiku Statement of Quality) |
| §2 | CDISC "mandated by FDA, EMA, PMDA" | **Corrected** | FDA and PMDA require it; EMA doesn't routinely require datasets |
| §3.1 | API-based ingestion from EDC | **Roadmap** | Depends on the customer's EDC; outside the original points 3–6 anyway |
| §3.2 | Metadata-driven engineering, Global Standards Library | **Roadmap** | Not built. First step (optional): XPT labels from a specification file (W17) |
| §3.3 | Trace any result back to the raw CRF data point | **Reduced** | Lineage runs SDTM → ADaM → outputs |
| §3.3 | Immutable version history | **Reduced** | Project Git history exists; explicit commit messages to be configured (W14) |
| §4 step 3 | SDTM standardization (raw → SDTM visual mapping) | **Reduced** | Replaced by SDTM *lock & conformance* on supplied SDTM. Mapping → roadmap |
| §4 step 4 | ADaM derivation per SAP | **In** | 5 datasets |
| §4 step 5 | TFL generation (tables, listings, figures) | **Reduced** | 4 summary tables, aligned to SAP numbering (W8). Listings, figures, RTF → roadmap |
| §4 step 6 | XPT v5 + Define-XML | **Reduced** | XPT in; Define-XML → roadmap |
| §5 | Visual vs. code validation path (automated comparison, version linking, validation logs) | **Roadmap** | Optional single comparison as a demo (W19). A notebook isn't recommended for the validation path: use a recipe in the flow |
| §6 | XPT via open-source converters | **In** | `xportr`; package versions to be pinned (W9) |
| §6 | Automated Define-XML "100% verified" | **Roadmap** | — |
| §6 | Pinnacle 21 reports | **In (manual)** | Report attached to the Govern record before ADaM sign-off (W5) |
| §6 | Interactive medical review dashboards | **Roadmap** | No dashboard in the project |
| §7.1 | Reduced SAS TCO, knowledge retention | **Messaging only** | Hypotheses to validate with the prospect |
| §7.1 | Shift-left oversight via dashboards | **Roadmap** | Depends on dashboards |
| §7.2 | Standard domain recipes across dozens of studies | **Roadmap** | Single study. See related project `CDISC_ADAM_FACTORY` (decision pending) |
| §7.2 | 60% reduction in cleaning and mapping labour | **Removed** | No source; data cleaning out of scope |
| §7.3 | Digital thread CRF → CSR | **Reduced** | SDTM → outputs only |
| §7.3 | Zero Define-XML discrepancy risk | **Removed** | No Define-XML |
| §8 | Technical and operational challenges | **Kept** | Discussion material in [doc 04](04-responsibilities-and-limitations.md) |
| §9 | Mandatory commit messages ("already the case") | **Reduced** | Currently `AUTO`; configure explicit mode (W14) or drop the claim |
| §9 | Electronic signatures at 3 gates (raw→SDTM, SDTM→ADaM, end of ADaM) | **Reduced** | **2 governance approval gates** (SDTM, ADaM). The raw→SDTM gate goes with raw → SDTM mapping. Not Part 11 e-signatures |
| §9 | Lead Statistician must sign before promotion | **In** | Enforced by gate scripts; approver role to be set up (W6) |
| §9 | Version-to-data binding | **In** | Data and code fingerprints with automatic revocation (goes beyond timestamps) |
| §9 | Closed system: SSO + RBAC | **Target architecture** | Customer configuration on a self-managed install ([doc 03](03-architecture-and-prerequisites.md)) |
| §9 | QMS integration of Govern audit metadata | **Roadmap** | — |

---

## 4. Messaging guardrails

Presenters and written materials should follow this split.

| ✅ We can say | ❌ We don't say |
|---|---|
| "A governed SDTM → ADaM → TLF/XPT pipeline" | "CRF to CSR" or "end-to-end submission automation" |
| "3 of 5 ADaM datasets with no code; open-source CDISC tools where needed" | "No code at all" or "80% visual" |
| "The system enforces the order of steps and revokes approvals when data or code change" | "Immutable audit trail" (until audit-log retention is configured) |
| "Govern approval gates that support Part 11 controls; e-signatures come from your e-signature system" | "Part 11-compliant electronic signatures" |
| "Designed to support your GxP validation on a self-managed install" | "100% GxP compliant" or "validated environment" |
| "Pinnacle 21 validation is part of the workflow" | "Integrated Pinnacle 21" |
| "Define-XML, dashboards, EDC ingestion, a standards library: let's discuss what you need" | Any of those as existing features |
| "Reduced dependence on SAS licences" (as a hypothesis) | "60% reduction" or other unsourced figures |

---

## 5. Roadmap (deferred from the original scope)

Each item is revisited only when a prospect's discovery answers justify it.

| Item | Original § | Revisit when |
|---|---|---|
| Raw → SDTM mapping (+ raw→SDTM gate) | §4.3, §9 | Prospect wants SDTM creation in Dataiku |
| EDC/API ingestion | §3.1 | Prospect names its EDC and wants a pilot |
| Metadata-driven standards library | §3.2 | Prospect has an MDR or specification format to plug in |
| Define-XML 2.1 | §4.6, §6 | After spec-driven metadata (W17) |
| Independent visual-vs-code validation path | §5 | Prospect's QC policy requires double programming |
| Medical review dashboards | §6, §7.1 | Prospect's medical review team is in the evaluation |
| Full TLF suite (listings, figures, RTF to the customer's shells) | §4.5 | Prospect shares templates |
| QMS integration | §9 | Prospect's QA defines the target QMS |
| Part 11 e-signature integration | §9 | Prospect names its e-signature system |
| Multi-study scale, standard domain recipes | §7.2 | Portfolio or CRO use case |

---

## 6. What changes in the materials

| Material | Change | Status |
|---|---|---|
| Original scoping document | Replace §3–§9 commitments with this baseline, or add a pointer to this page | To do (W2) |
| Deck | Apply the slide corrections in [doc 05 §3](05-mvp-backlog-internal.md#3-deck-claim-corrections) | To do (W2) |
| Project wiki | Align article 2 and 3 with the baseline (approver roles, scope wording) | To do (W16) |
| `docs/01`–`05`, `readiness-map.html` | Aligned with this baseline | Done (2026-11-05) |

---

## 7. Changing the baseline

Any change to this scope (adding a roadmap item to the asset, or removing an in-scope capability) should:

1. Be recorded as a new row in §3 with the date and rationale.
2. Update the messaging guardrails (§4) if it changes what can be claimed.
3. Update the readiness map and the internal backlog.
