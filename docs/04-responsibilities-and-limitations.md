# Responsibilities & Limitations

> **Audience:** customer QA / CSV leads, clinical programming leads, Dataiku presenters.
> This page states plainly who is responsible for what under GxP, and what the MVP does **not** do.

---

## 1. Three layers of responsibility

No software is "GxP compliant" on its own. Compliance comes from a validated system **used under the customer's procedures**. For this asset there are three layers:

| Layer | Provides |
|---|---|
| **Dataiku (vendor)** | A controlled platform: documented SDLC and change management, ISO 27001 / SOC 2 / ISO 9001, release notes per version, audit-trail and access-control features, a published Part 11 shared-responsibility matrix (*Statement of Quality*, July 2026). Dataiku states that it **is not itself subject to GxP** and that **DSS and Govern are not an electronic-signature system**. |
| **This asset** | A customer-side implementation of controls that Part 11 leaves to the customer: the **enforced sequence of steps**, a **governed approval workflow**, **drift detection with automatic revocation**, and conformance checks within the pipeline. |
| **Customer** | Platform validation (IQ/OQ/PQ), SOPs and accountability policies, user training, access reviews, the Part 11 e-signature system, records retention, backups and disaster recovery, and the scientific validity of the derivations. |

---

## 2. 21 CFR Part 11 mapping

| Clause | Requirement | Dataiku (platform) | This asset | Customer |
|---|---|---|---|---|
| §11.10(a) | System validation | Vendor quality system; change control on releases | Gated scenarios form a basis for OQ test cases | **Validates the system** (IQ/OQ/PQ, CSA) |
| §11.10(b) | Accurate copies for inspection | Dataset export, project bundles, flow documentation, Git history | XPT and TLF packages, wiki, Govern record | Decides formats suitable for agencies |
| §11.10(c) | Record protection & retention | Not a data store; versioned project configuration | Fingerprints identify the approved data and code version | Retained, backed-up storage for inputs and released outputs |
| §11.10(d), (g) | Authorised access & authority checks | Groups, permissions, SSO, User Isolation Framework | Gates read sign-off status before building | Assigns roles, reviews access periodically |
| §11.10(e) | Audit trail | Time-stamped audit events; forwarding to external storage | Govern record captures results, fingerprints, approvals | Configures retention and reviews the audit trail |
| §11.10(f) | **Enforced step sequencing** | Not provided by Dataiku (use-case specific) | **Core value of the asset:** SDTM → ADaM → release cannot be skipped; drift revokes approvals | Defines the approved sequence in SOPs |
| §11.10(h) | Valid data sources | Connection permissions, schema checks | ⚠️ SDTM currently uploaded as files | Verifies source systems |
| §11.10(i) | Training | Academy, documentation, certifications | Demo script and wiki | Trains and qualifies users |
| §11.10(j) | Accountability policies | Named accounts, audit trail | — | Written policies |
| §11.10(k) | Documentation control | Versioned product docs, project wiki, Git | This documentation set | Controls distribution and change of system documentation |
| §11.50 | Signature shows name, date/time, meaning | Govern sign-off records the user, timestamp and step | Sign-off steps per stage | **Provides the Part 11 e-signature** (linked through the Govern REST API) |

---

## 3. Limitations of the MVP

### Scope
The MVP covers a **governed SDTM → ADaM → TLF/XPT pipeline**. Items outside that are on the roadmap (§4).
- **SDTM is the input.** No raw → SDTM mapping, no EDC/API ingestion. The data is synthetic (50 subjects, single arm).
- **Two approval gates** (SDTM, ADaM). A raw → SDTM gate would come with raw → SDTM mapping.
- **No metadata-driven standards library, medical review dashboards or QMS integration.**
- **No Define-XML** and no final reviewer's guides. The attached ADRG, SDRG and SAP are illustrative drafts that don't fully match the pipeline.
- **TLFs are partial:** 4 summary tables as text and CSV. No listings, figures or RTF output, and numbering not yet aligned to the SAP.
- **No independent double-programming path.** QC is limited to 26 in-flow consistency checks plus conformance validation.

### Standards & validation
- **SDTM conformance** uses CDISC CORE 0.16.0 (SDTMIG 3.4). **ADaM conformance** relies on a manual Pinnacle 21 Community run outside Dataiku.
- **XPT metadata isn't specification-driven:** variable labels currently equal variable names, and lengths are derived from the data. Pinnacle 21 flags this.
- Custom dataset **ADREACT** models reactogenicity. The CDISC Vaccines TAUG modelling conventions haven't been assessed.

### Governance
- Govern sign-offs are **governance approvals, not Part 11 electronic signatures**.
- One Govern study record per app instance must be configured by hand; the default record is shared.
- Fingerprints **detect** changes but don't **keep a copy** of the approved data. Reproducing an approved version relies on the customer's storage and retention.
- The asset doesn't include an IQ/OQ/PQ package or a scripted installer.

### Technical (the deck's "challenges")

| Challenge | Status in the MVP |
|---|---|
| SAS format catalogs | Not addressed (inputs aren't SAS datasets) |
| Strict XPT constraints (8-character names, 40-character labels) | Names comply; labels not yet driven by a specification |
| Complex derivations needing code | Baseline logic uses `admiral`; everything else is visual |
| No native Pinnacle 21 / Define-XML connector | Confirmed: P21 is manual, Define-XML is on the roadmap |
| Keeping visual and code in sync | Code fingerprint detects any recipe change; no output comparison |
| Reviewers expect program files | Code recipes are plain R/Python; visual recipes need exported flow documentation |
| "Black-box" Prepare recipes | 8 Prepare recipes, one per derivation topic; step-level documentation still to add |
| Concurrency & version locking | Data and code fingerprints with automatic revocation; per-study records to configure |
| Platform validation | Customer responsibility; self-managed deployment recommended |

---

## 4. Roadmap candidates

These are prioritised by the customer's answers in discovery (see [demo script §4](02-demo-script.md#4-discovery-questions)):

| Item | Depends on |
|---|---|
| Raw → SDTM mapping (`sdtm.oak` or visual) with a raw → SDTM approval gate | Customer SDTM process |
| EDC/API ingestion | Customer EDC |
| Metadata-driven standards library | Customer MDR / specification format |
| Specification-driven XPT metadata → **Define-XML 2.1** | Customer MDR / specification format |
| Full TLF suite to the customer's shells (RTF, listings, figures) | Customer templates |
| Independent visual-vs-code QC path with automatic comparison | Customer QC policy |
| Medical review dashboards | Customer medical review process |
| Part 11 e-signature integration via the Govern REST API | Customer e-signature system |
| QMS integration of Govern audit metadata | Customer QMS |
| Pinnacle 21 Enterprise integration | Customer licence |
| Multi-study operation, standard domain recipes | Portfolio size |
| Installer, IQ/OQ test pack | Committed pilot |
