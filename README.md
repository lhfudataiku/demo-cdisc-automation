# demo-cdisc

Documentation for the **FLU2026 CDISC Submission Pipeline**, a Dataiku MVP that demonstrates a governed SDTM → ADaM → TLF workflow for pharma clinical teams evaluating a self-hosted SAS replacement.

- **Dataiku project:** `CLIN_FLU2026_VISUAL` (developed on Dataiku Cloud; target deployment is self-managed Dataiku)
- **Govern record (development):** `ar.114`
- **Data:** synthetic Phase II influenza-vaccine study, 50 subjects. Not for submission or clinical use.

## Documentation

| # | Document | Audience |
|---|---|---|
| 00 | [CDISC & submission primer](docs/00-cdisc-submission-primer.md): standards, regulation, incumbent tools | Anyone new to clinical data |
| 01 | [Overview](docs/01-overview.md): what the asset is, deliverables, real vs. roadmap | Everyone |
| 02 | [Demo script](docs/02-demo-script.md): storyline, step-by-step, discovery questions, objections | Presenters |
| 03 | [Architecture & prerequisites](docs/03-architecture-and-prerequisites.md): self-managed target, licences, installation checklist | Architects, customer IT |
| 04 | [Responsibilities & limitations](docs/04-responsibilities-and-limitations.md): Part 11 shared responsibility, honest limitations | Customer QA, clinical leads |
| 05 | [MVP backlog (internal)](docs/05-mvp-backlog-internal.md): known issues, phased work plan, deck corrections | **Internal only** |
| 06 | [Scope baseline](docs/06-scope-baseline.md): agreed scope, change register vs. the original scoping document, messaging guardrails | **Internal only** |
| — | [Readiness map](docs/readiness-map.html): one-page HTML view of architecture, gaps, assumptions and work plan | **Internal only** |

**Scope:** re-baselined on 2026-11-05 to a governed SDTM → ADaM → TLF/XPT pipeline. Raw → SDTM mapping, EDC ingestion, a standards library, Define-XML, a code-validation path, dashboards and QMS integration are roadmap. See [06 · Scope baseline](docs/06-scope-baseline.md).
