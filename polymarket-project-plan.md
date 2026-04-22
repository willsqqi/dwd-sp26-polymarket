## Polymarket Project Plan

### Objective
- `polymarket_api_analysis.ipynb` for platform-level Polymarket analysis.
- `polymarket_us_iran_ceasefire_case_study.ipynb` for a focused event-window wallet case study.

---

## 1) Scope and Working Principles
- Keep general analytics and case-study logic separate.
- Use UTC consistently for all time filters and comparisons.
- Keep API credentials in environment variables (no hardcoded secrets).

---

## 2) Notebook A: General Polymarket Analysis

### Goal
Provide a clear overview of open-event activity and platform-level trading context at the event-aggregation layer.

### Data Sources
- Polymarket Gamma API (`/events` only — `/markets` is intentionally not used; it truncates and is not representative at the sample size the free endpoint allows)
- Optional AI commentary step using Vertex AI on one selected event

### Procedure
1. Pull open-event metadata with pagination from `/events`.
2. Derive a usable category per event from tags/series; coerce volume/liquidity/createdAt in place.
3. Build event-level summary tables: total/mean/median volume, top N by volume, top N by liquidity.
4. Create 4 presentation charts (volume distribution, top-15 by volume, creation trend, top categories).
5. Export the cleaned events table to `polymarket_outputs/events_open_raw.csv`.

### Event-to-AI Flow
- Prints top open events by volume.
- Selects event ID manually.
- Sends selected event context to AI agents for suggestion text.

### Deliverables
- One polished general-analysis notebook.
- Reusable CSV output for the event snapshot (`events_open_raw.csv`).

---

## 3) Notebook B: Ceasefire Case Study

### Goal
Evaluate whether wallet behavior in a fixed pre-announcement window appears unusually concentrated.

### Fixed Parameters
- Event slug: `us-x-iran-ceasefire-by`
- Anchor timestamp: `2026-04-07 22:32:00 UTC`
- Pre-window length: 5 hours

### Data Sources
- Gamma API for event/submarket scope
- Dune API for trade-level records and wallet metadata

### Procedure
1. Retrieve the target event and identify relevant submarkets before anchor time.
2. Map selected markets to Dune identifiers using market detail tables.
3. Pull pre-window trades from Dune with configured amount filtering.
4. Build wallet-level aggregates from pulled trades.
5. Split exposure by trade direction (ceasefire side vs no-ceasefire side).
6. Enrich selected wallets with metadata (`created_time`, `first_funded_time`).
7. Export case-study tables with consistent naming in `polymarket_outputs/`.

### Deliverables
- One reproducible case-study notebook.
- CSV outputs for mapping, pre-window trades, wallet summaries, and metadata join.

---

## 4) Validation Checklist

### General Notebook
- Open-event retrieval runs end-to-end.
- Charts render without manual patching.
- Top-event selection and AI prompt flow work with manual event ID.
- Required CSV exports are refreshed correctly.

### Case Notebook
- Event slug resolves correctly.
- Selected submarkets are pre-anchor only.
- Pre-window filter is correctly enforced in UTC.
- Wallet analysis is based on case-study trade pull only.
- Wallet metadata includes both creation and first-funding fields when available.

---

## 5) Execution Sequence
1. Finalize general notebook data pull/cleaning and chart sections.
2. Finalize top-event selection and single-event AI suggestion flow.
3. Finalize ceasefire case-study pull, aggregation, and enrichment.
4. Run validation checks for both notebooks.
5. Clean redundant files and keep only final outputs for submission.
