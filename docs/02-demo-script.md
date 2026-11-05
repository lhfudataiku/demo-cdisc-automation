# Demo Script

> **Audience:** Dataiku presenters (solutions engineers, account teams).
> **Length:** ~25 min demo + 20–30 min discussion.
> **Setting:** pharma clinical programming / biostatistics team evaluating a self-hosted SAS replacement.

---

## 1. Before the demo

### Environment checklist

| Check | How |
|---|---|
| Use a **dedicated app instance** of `CLIN_FLU2026_VISUAL`, not the template | Create it from the app "FLU2026 — CDISC Submission Validation" |
| The instance points to **its own Govern record** | Project variable `govern_artifact_id` (default `ar.114` is shared by every instance — see [backlog](05-mvp-backlog-internal.md)) |
| Govern sign-offs reset to **NOT_STARTED** and workflow on **SDTM Validation** | Run the `debug_reset_signoffs` scenario *before* the meeting, not on screen |
| Two Govern accounts ready: a **reviewer** and an **approver** (Lead Statistician) | Avoid approving your own work on screen — segregation of duties |
| Pinnacle 21 Community installed locally, configured for **ADaMIG 1.3** | Pre-load the configuration so the run takes seconds |
| Browser tabs open | App homepage · Flow · Govern record (workflow tab) · one Prepare recipe · `flag_vs_baseline` R recipe |

> ⚠️ **Known demo pitfalls until the backlog fixes land**
> - The **"Download ADaM + SDTM XPT package (.zip)"** tile serves a stale zip. For the P21 step, take the `.xpt` files from the `SUBMISSION_XPT` and `SDTM_XPT` folders instead.
> - SDTM conformance always shows **PASS**, even with 6,047 findings. Don't claim the gate "found no issues": say the findings are reviewed by a person before sign-off.
> - If ADaM conformance is left blank in Govern, the release gate fills in PASS from the in-flow QC. **Always set the P21 result explicitly** on the Govern record before releasing.

> 🧭 **Scope guardrails** (full list in [06 · Scope baseline §4](06-scope-baseline.md#4-messaging-guardrails))
> - Present the asset as a **governed SDTM → ADaM → TLF/XPT pipeline**, not "CRF to CSR".
> - Raw → SDTM mapping, EDC ingestion, a standards library, Define-XML, a code-validation path, dashboards and QMS integration are **roadmap**: raise them as discovery questions, never as features.
> - Govern sign-offs are **approval gates**, not Part 11 e-signatures. Don't say "100% GxP compliant" or quote "60%".

---

## 2. Storyline

| Minute | Section | Message |
|---|---|---|
| 0–3 | Context | The CDISC pipeline today is SAS-centric, opaque and enforced by SOPs |
| 3–8 | The flow | Visual-first ADaM, with open source where CDISC needs it |
| 8–13 | Lock & validate SDTM | Conformance plus a data/code fingerprint |
| 13–16 | SDTM sign-off | Governed approval in Govern |
| 16–19 | **"It blocks"** | Change the data → approvals revoked automatically |
| 19–23 | Build ADaM → P21 → sign-off | Same gate pattern for ADaM |
| 23–25 | Release | Validated XPT and TLF package |
| 25+ | Discussion | Discovery questions (§4) |

---

## 3. Step by step

### Step 0 · Context (no clicks)
> "Today a typical CDISC pipeline is thousands of lines of SAS macros. QC means writing everything twice, and the rule 'don't build ADaM before SDTM is signed off' lives in an SOP. We'll show the same pipeline built visually, with open-source CDISC tools where needed, and with those rules enforced by the system."

### Step 1 · Walk the Flow (≈5 min)
1. Open the **Flow**. Point out the colour-coded zones: SDTM inputs → five ADaM zones → validation → XPT → TLFs.
2. Open **`prep_adsl`** (Prepare recipe): population flags and age bands as readable formula steps.
   > "This is the kind of derivation that's usually 200 lines of SAS. Here a statistician can read it."
3. Open **`flag_vs_baseline`** (R recipe): ~12 lines of `admiral`.
   > "Where CDISC semantics need a trusted implementation (baseline flags), we use `admiral`, the pharmaverse package that Roche, GSK and others maintain. Code where it adds value, visual everywhere else."
4. Show **lineage** for one ADaM variable back to its SDTM source (that's ADaM's traceability requirement).

### Step 2 · Lock & validate SDTM (≈5 min)
1. App homepage → **section 1 · "Lock & validate SDTM"** → run.
2. While it runs, explain: it converts the 6 SDTM domains to XPT v5, runs **CDISC CORE** (open source, SDTMIG 3.4), and records a **fingerprint** of the data and the pipeline code in Govern.
3. **Section 2** → download the conformance workbook; show the *Issue Summary* tab.
   > "These findings get reviewed by a person. Some get fixed, some get explained in the reviewer's guide, which is standard practice."

### Step 3 · SDTM sign-off in Govern (≈3 min)
1. **Section 3** → "Open SDTM sign-off in Govern".
2. Show the metadata the scenario wrote automatically: conformance result, findings summary, **data-version fingerprint**, code version.
3. As **reviewer**: add feedback. As **approver**: approve, **with a comment**.
   > "In the target setup only the Lead Statistician role can approve, and the approval is tied to this exact fingerprint."

### Step 4 · "It blocks" (≈3 min): the key moment
Pick **one** of these:

- **Option A: ADaM before approval.** Before Step 3, run **section 4 · "Build ADaM (gated)"**. It fails with *"ADaM build blocked - the SDTM validation gate is not satisfied … SDTM Validation sign-off is 'WAITING_FOR_FEEDBACK' in Govern (must be APPROVED)"*.
- **Option B: data drift (stronger).** After Step 3, edit one SDTM value (or one formula in a Prepare recipe), then run **Build ADaM**. The gate detects the change, **revokes the approvals automatically**, rolls Govern back to SDTM Validation, and explains why.
  > "An approval only covers exactly what was reviewed. A silent data change can't ride on an old approval."

  Afterwards, undo the change and rerun Steps 2–3.

### Step 5 · Build ADaM → Pinnacle 21 → ADaM sign-off (≈4 min)
1. **Section 4** → "Build ADaM (gated)". The gate passes; the five ADaM datasets, the 26 QC checks, the XPT files and the TLFs are built.
2. Run **Pinnacle 21 Community** (ADaMIG 1.3) on the XPT files and show the report.
   > "We kept Pinnacle 21 because it's what your team and FDA use. With Pinnacle 21 Enterprise, this manual step is a candidate for integration."
3. In Govern, set the **ADaM conformance result**, then reviewer feedback → approver sign-off.

### Step 6 · Release (≈2 min)
1. **Section 7** → "Release submission (gated)". The record becomes **Validated** and moves to **ADaM Validated**.
2. Download the **TLF package**; open `t-14-02-01-immunogenicity.txt` (GMT, seroprotection, seroconversion by strain).
   > "The final outputs can only be released once both approvals exist for exactly this data and code."

### Step 7 · Close (no clicks)
> "This is an MVP that shows the pattern from SDTM onwards. SDTM creation, Define-XML, your TLF templates, your EDC, your e-signature system and your QMS are where we'd adapt it to you. Which brings us to some questions."

---

## 4. Discovery questions

Use the gaps as questions. Their answers decide what the next iteration builds.

| Topic | Question |
|---|---|
| Landscape | Which EDC and SCE do you use today? What does your SAS footprint look like (users, macro libraries, Grid/LSAF)? |
| Standards | Do you have a metadata repository? Which SDTMIG/ADaMIG versions and therapeutic-area guides do you follow? |
| Define-XML | How do you produce Define-XML today: P21 Enterprise, Formedix, in-house? |
| Validation | Do you own Pinnacle 21 Enterprise? How are conformance findings tracked and justified? |
| QC | Full double programming or risk-based? Which compare tooling? |
| TLFs | Do you have standard shells and RTF templates? How many outputs per study? |
| Open source | Are R / pharmaverse packages already qualified in your environment? |
| Signatures | Which e-signature system does your QA accept for Part 11? |
| Validation of the platform | How do you validate an SCE today (IQ/OQ/PQ, CSA)? Who owns it? |
| Migration | Which SAS assets must keep working (macros, format catalogs, legacy studies)? |
| Success criteria | What would a pilot need to prove, on which study, in what timeframe? |

---

## 5. Likely objections

| Objection | Suggested answer |
|---|---|
| "Is Dataiku validated / GxP compliant?" | No software is compliant on its own. Dataiku provides a controlled, audited platform (ISO 27001, SOC 2, ISO 9001, documented change control) and publishes a Part 11 shared-responsibility matrix. The customer validates it for their own use, and on a self-managed install they control upgrades. See [04](04-responsibilities-and-limitations.md). |
| "Is the Govern sign-off a Part 11 e-signature?" | No. Dataiku states that Govern is not an electronic-signature system. Govern provides the **approval gate and the record**. A Part 11 signature comes from your e-signature system, linked through the Govern API. |
| "FDA expects program files, not graphs." | Visual recipes export as flow documentation. Code recipes are plain R/Python files. For the regulated derivations we use `admiral`, which reviewers already recognise. |
| "Visual recipes can become black boxes too." | Agreed. The design rule is small, single-purpose Prepare recipes, one per derivation topic, plus a wiki per zone. |
| "Who maintains the open-source packages?" | The pharmaverse is maintained by a pharma consortium. Pinned versions in Dataiku code environments make results reproducible. |
| "Where's Define-XML?" | Not in the MVP. The next step is driving XPT metadata from a specification, which is the basis for Define-XML. |
