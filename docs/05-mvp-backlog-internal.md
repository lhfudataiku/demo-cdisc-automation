# MVP Backlog — Internal

> ⚠️ **Internal only. Do not share with prospects.**
> Known issues found while reviewing `CLIN_FLU2026_VISUAL`, the app instance `CLIN_FLU2026_VISUAL_dUOrLYMV` and Govern record `ar.114` (2026-10-21), and the recommended MVP scope.

---

## 1. Known issues (with evidence)

| # | Issue | Evidence | Impact |
|---|---|---|---|
| K1 | **SDTM gate can never fail** | `build_sdtm` › step *Report SDTM conformance to Govern* counts findings whose `severity` matches `error\|reject\|not execut\|fail`. In `SDTM_VALIDATION`, `severity` holds rule **executability** (`fully executable` 2,088 · `partially executable - possible overreporting` 3,957 · `… underreporting` 2). Govern: *"6047 CORE findings (0 error/reject-class)"* → `conformance_result_sdtm = PASS`. | The "hard gate" enforces nothing. A sceptical audience will ask to see it fail. |
| K2 | **Stale Pinnacle 21 package** | No recipe or scenario writes `p21_submission_package.zip`; it ships with the bundle (template file dated 2026-06-18). In `dUOrLYMV` the zip is from 07-27 while the XPT files are from 07-28. | Reviewers validate old files, then approve new ones. |
| K3 | **P21 step can be skipped** | `release_submission` › *GATE* sets `validation_status` from the in-flow `ADAM_VALIDATION` when blank. Govern: `validation_errors = "In-flow ADaM QC (validate_adam): 26 checks … All PASS"`. `P21_ADAM_REPORT` folder is empty; no P21 report attached. | Contradicts "mandatory Pinnacle 21 step". |
| K4 | **All instances share one Govern record** | `dUOrLYMV` has `govern_artifact_id = ar.114`; homepage HTML hard-codes `ar.114` URLs; the instance's runs (2026-07-28 13:36/13:39 UTC) wrote to `ar.114`. | Concurrent demos or studies overwrite each other's approvals. |
| K5 | **API keys can approve sign-offs** | Both sign-offs list 4 global API keys (via the `administrators` group) as feedback givers and approvers. | Part 11: a signature must belong to one identifiable person. |
| K6 | **Self-approval, no feedback, no comments** | Pierre Portal is reviewer *and* approver on SDTM; both approvals by him (13:35 / 13:39 UTC), empty comments, empty feedback lists. | Segregation of duties broken; no recorded meaning. |
| K7 | **Debug reset on the operator homepage** | Section "⚙️ Debug · reset validation state" runs `debug_reset_signoffs` (clears every sign-off). | Anyone using the app can erase approvals. |
| K8 | **Gate-bypassing scenarios still shipped** | `run_full_chain`, `run_full_chain_1`, `AAA` (all "zz DEPRECATED"). | Undermines the "no bypass" message. |
| K9 | **Integrity sweep inactive** | `governance_integrity_check` has an hourly trigger but the scenario is inactive. | Drift is only caught when someone runs a gated scenario. |
| K10 | **ADaM build continues after failure** | `build_adam` › build step has `proceedOnFailure: true`. | Partial builds can reach the record step. |
| K11 | **R packages unpinned** | `cdisc_adam_r` spec: `admiral xportr haven rtables tern dplyr tidyr` (no versions), live CRAN mirror. | Not reproducible; blocks validation. |
| K12 | **XPT labels = variable names** | `xpt_export`: `label = substr(names(df), 1, 40)`, `strict_checks = FALSE`. | P21 findings; not specification-driven. |
| K13 | **TLFs don't match the SAP** | SAP plans 14.1.1 Demographics, 14.2.1 Reactogenicity, 14.2.2 AE by severity, 14.3.1 HAI GMT, 14.3.2 GMFR, 14.3.3 Seroconversion/seroprotection, Listings 16.2.1 and 16.2.7. Flow produces 14-1-1 demographics, **14-2-1 immunogenicity, 14-2-2 reactogenicity, 14-3-1 TEAE**; no listings. | Statisticians notice immediately. |
| K14 | **Reviewer guides don't match the flow** | ADRG/SDRG (`uf.9`, `uf.10`) list an **SV** domain and an **ADENDPOINT** dataset, which don't exist. SDRG/SAP name safety/reactogenicity as primary; the wiki calls ADIS the primary endpoint. | Undermines trust in the package. |
| K15 | **Contradictory change note in Govern** | `description_of_changes_sdtm` says EPOCH was re-derived to fix the 3,957 missing-EPOCH findings; the same record still reports 3,957. | Credibility. |
| K16 | **Wiki out of date** | Article 3 says reviewers/approvers are `designers` / `space_administrators`; actual config is `administrators` + named users. | Documentation accuracy. |
| K17 | **CDISC workflow inside a shared blueprint** | `bv.adam_validation_2` is a version of `bp.asset_inventory`, which also holds AI-governance records; 11 mock study records also use it. | Packaging drags along unrelated content. |
| K18 | **Approvers are named users, not roles** | Pierre and Remy assigned directly. | They vanish on blueprint export/import. |
| K19 | **Auto-generated commit messages** | Project Git log, e.g. "Saved Jupyter notebook…", "Updated article…". | Contradicts deck slide 10 ("mandatory commit messages"). |
| K20 | **Reference project missing** | Project description cites `CLIN_FLU2026_NATIVE` ("every value verified identical"); the project isn't on the instance. | Claim can't be demonstrated. |
| K21 | **Fingerprint loads full datasets into memory** | `_fingerprint()` uses `get_dataframe()` on every SDTM dataset. | Fine at 50 subjects; won't scale to real LB volumes. |

### Not yet verified (needs a Govern **admin** API key)
- Blueprint version definition, sign-off configurations, role assignments.
- Whether the integrity hook *"CDISC validation integrity"* exists (`list-hooks` returned none, possibly due to permissions).
- Linked records `ar.112`, `ar.140`, `ar.143` and the dataset records.
- Whether Govern audit logging is enabled.

---

## 2. Recommended MVP scope

Goal: **every claim made in the room is true**, with one concrete "metadata-driven" moment.

### Must do: credibility fixes

| # | Action | Fixes | Effort |
|---|---|---|---|
| M1 | Rewrite deck claims to match the *Statement of Quality* (see §3) | — | S |
| M2 | SDTM gate: classify findings by real severity or rule category (or a list of blocking rule IDs); prepare a "fails on purpose" demo variant | K1 | S–M |
| M3 | Build the P21 zip inside the flow from the current XPT files | K2 | S |
| M4 | Require an explicit ADaM conformance result + uploaded P21 report before release; remove the auto-fill | K3 | S |
| M5 | Approvals: remove API keys; two demo users (reviewer / approver); assign through Govern roles; require a comment | K5, K6, K18 | S |
| M6 | Hide the debug reset from the homepage; delete deprecated scenarios; activate or remove the integrity sweep; set `proceedOnFailure` to false | K7–K10 | S |
| M7 | Align TLF numbers and titles to the SAP; fix the ADRG/SDRG (remove SV and ADENDPOINT) or detach them; fix the change note | K13–K15 | S |
| M8 | Pin R package versions | K11 | S |
| M9 | Documentation set (this folder); update wiki article 3 | K16 | S |

### Should do: high value

| # | Action | Fixes | Effort |
|---|---|---|---|
| S1 | Drive XPT labels and lengths from a small specification CSV (groundwork for Define-XML) | K12 | M |
| S2 | Add SAP tables 14.3.2 (GMFR) and 14.3.3 (seroconversion/seroprotection) | K13 | S–M |
| S3 | One double-programming comparison (e.g. independent R ADSL vs. visual ADSL, with a difference report) | K20 | M |
| S4 | Export flow documentation for one ADaM dataset as a sample "reviewer program" | — | S |
| S5 | Per-instance Govern record (create on instantiation, or documented manual step) and parameterised homepage links | K4 | M |

### Out of scope: roadmap only
Define-XML, raw → SDTM / EDC ingestion, Part 11 e-signature integration, P21 Enterprise, full TLF suite (RTF, listings, figures), installer and IQ/OQ pack, multi-study standards library, SAS format catalogs, scale testing, dedicated blueprint extraction (K17, needed only for packaging).

---

## 3. Deck claim corrections

| Slide | Current claim | Suggested wording |
|---|---|---|
| 2 | "100% GxP compliance" | "Designed to support the customer's GxP validation" |
| 3 | Standards "mandated by FDA, EMA, PMDA" | "Required by FDA and PMDA; EMA does not yet routinely require datasets" |
| 4 | "API-based ingestion", "Global Standards Library … without per-study scripting" | Present as target architecture / roadmap |
| 6 | "Dual independent visual/code paths with automated divergence alerts" | Roadmap, unless S3 is delivered |
| 7 | "Automated Define-XML, 100% verified" | Roadmap |
| 7 | "60% reduction in data cleaning labor" | Remove, or cite a source; data cleaning is out of scope |
| 7, 12 | "Medical review dashboards" | Roadmap |
| 10 | "Govern: 21 CFR Part 11 compliance", "formal electronic signatures" | "Govern approval gates support Part 11 controls; electronic signatures come from the customer's e-signature system, linked via API" |
| 10 | "Mandatory commit messages" | Remove, or enforce it |
| 10 | "Lead Statistician sign-off at Raw→SDTM" | The MVP gates at SDTM and ADaM; raw → SDTM is out of scope |
| 12 | "100% GxP Audit-Ready", "Validated Environment Management", "zero Define-XML discrepancy risk" | Remove; describe controls rather than outcomes |
