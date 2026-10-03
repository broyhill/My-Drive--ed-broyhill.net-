# E9x Ingestion Pipeline — Charter & Single Source of Truth (2026-10-02)

**Status:** CONSOLIDATED CHARTER. Per Ed's order ("move all that ingestion info to the
new home"), this document is now the **single source of truth for all ingestion**
on the platform. It absorbs and supersedes, on the ingestion subject:
- `E61_SOURCE_INGESTION_IDENTITY_RESOLUTION_BLUEPRINT.md` (2026-04-27) — machinery
  re-stationed below; file retained as detail appendix under banner
- `E61_CANDIDATE_NETWORK_UPLOAD_INTEGRATION.md` (2026-04-27) — upload path re-routed
  to E99; file retained as detail appendix under banner
- `E95_DARK_DONOR_RECOVERY_BLUEPRINT.md` (2026-10-02 AM) — engine re-stationed to
  E96 ⚠; file retained under banner

Number assignments marked ⚠ are Ed's working assignments ("I think"); confirm against
the canonical roster before code moves. **All code movement blocked on `gh auth login`.**

---

## 1. The chain

```
  raw FEC / NCBOE / CSV files — whoppers and patties alike, NO exceptions
        │
        ▼
┌──────────────────────────────────────────────────────────────────┐
│  E99 — GATEWAY (the airlock; SEPARATE FROM ALL OTHERS by rule)   │
│  inspect · reformat · pair schemas · normalize to DataTrust-     │
│  alike (all-caps, suffix dictionary) · sort by committees ·      │
│  stamp RNC_REGID + candidate committee + campaign committee IDs  │
└──────────────┬───────────────────────────────────────────────────┘
               │ stamped, DataTrust-shaped output (one-way, forward only)
               ▼
┌──────────────────────────────────────────────────────────────────┐
│  E95 ⚠ — DEDUPE & ROLLUP                                         │
│  dedupe · rollup · identity match ladder ·                       │
│  DARK DONOR POOL: unmatched donors pooled, marked, identified    │
└──────────────┬───────────────────────────┬───────────────────────┘
               │ clean matched records     │ pooled dark donors
               ▼                           ▼
     donor pipeline (silver/gold)   ┌──────────────────────────────┐
               ▲                    │  E96 ⚠ — DARK DONOR RESEARCH │
               │                    │  research info · identify    │
               │                    │  transactions · edit/correct │
               └────────────────────│  · resubmit → SILVER or GOLD │
                                    └──────────────────────────────┘

   E98 — CANARY (Ed/Pope/Melanie · cluster 372171: 147 txns / $332,631.30)
   watches every stage that writes toward the donor database.
```

## 2. Why the gateway exists (post-mortem, Ed 2026-10-02)

**Six months of dedupe effort constantly crashed because mixed files and poor formats
were fed straight into it.** Dedupe presumes comparable rows; raw FEC, NCBOE, and
ad-hoc CSVs with mismatched schemas, casing, and formats made every comparison
unreliable and the process unstable. The fix is structural: **nothing reaches E95's
dedupe that is not already inspected, schema-paired, DataTrust-shaped,
committee-sorted, and ID-stamped.** Any proposal to "just run dedupe on this file real
quick" bypassing E99 re-creates a failure that already cost six months. Refuse it and
cite this section.

**No exceptions by size or source: ALL files go through the E99 gate — whopper files
and patties alike.** Shape, not size, was always the failure. There is no fast lane.
Uploaders never pre-format; the gate formats. Files the gate cannot schema-pair go to
quarantine with a reason code — never best-guessed forward.

## 3. DataTrust format intelligence (Ed, 2026-10-02 — partially confirmed)

- Files formatted **all-capital**, in a **specific font**.
- A **small but consistent space** somewhere in the donor record — **signature line or
  address**, not yet determined which — **triggers an anchor** (format fingerprint).
- **Street-type identifiers standardized** (Street/Drive/Court one consistent
  rendering). E99 must adopt the identical suffix dictionary — byte-true, not
  approximate. The standardized suffix makes the address line the stronger anchor
  candidate; the consistent space may sit at the suffix boundary.

**⚠ NORMALIZATION TRAP:** whitespace-collapse would erase a deliberate anchor space.
Until decoded, E99 preserves original byte-level spacing in the raw archive AND in
`raw_signature` / `raw_address` shadow columns; collapse applies only to working match
columns. Never normalize away what we have not yet decoded.

**Investigation (pre-build):** character-level diff of signature vs address fields
across known DataTrust records; suffix census (abbreviated vs spelled out, casing,
boundary spacing); resolve what "specific font" means for data files. Runnable locally
the moment sample files are provided — no repo needed.

## 4. Station assignments — where E61's machinery lands

The E61 SIIRE blueprint specced real machinery. It is hereby re-stationed (spec-level;
code follows when the repo opens):

### → E99 GATEWAY takes
| From E61 | Role at E99 |
|---|---|
| `validate_source` + `source_registry` (source_id templates: `ncboe_party_committee`, `candidate_upload`, `donor_upload`) | gate intake registry; add `fec_*` source ids |
| `snapshot_to_archive` | raw byte-level archive (anchor preservation) |
| `parse_csv_to_rows` + format sniff / source-template detect | inspection + schema pairing |
| `normalize.py` (case/ws/zip/nickname) | DataTrust-alike formatting — WITH §3 shadow-column trap honored |
| `address_parse.py` | address components + suffix dictionary |
| `005_e61_lookup_tables.sql` (zip→county, **NCSBE 2-letter SVI prefix→county**, nickname pairs) | gate lookups |
| `file_sha256` double-load rule (`source_file_signature.yaml`) | file-level dedupe at the door (whoppers AND re-uploaded patties; cf. Graphify duplicate-inflation lesson) |
| `quarantine.py` + quarantine UI + reason codes (incl. the 105 NCBOE clerk-error rows pattern, `corporate_in_name_field.yaml`) | gate quarantine |
| `upload.ts` API + upload.html + E24/E25 portal upload paths | ALL upload doors route to E99 |
| **NEW (Ed):** committee sort + RNC_REGID / candidate-committee / campaign-committee stamping | the stamp station |

### → E95 ⚠ DEDUPE & ROLLUP takes
| From E61 | Role at E95 |
|---|---|
| `cluster_assign.py` (cluster_id_v2: alphabetical-adjacency + last+first+zip5) | identity clustering |
| `datatrust_match.py` (T1–T6 ladder: literal → initial → nickname → mailzip → address-num → county-anchor) | match ladder |
| dedupe + rollup (Ed's dictation) | core function |
| dark-pool marking (`match_tier` NULL → pooled, marked, identified) | hand-off point to E96 |
| `publish.py` + `lineage.py` (lineage_link: canonical → source row) | publish to silver/gold with lineage |
| run metrics JSONB → E27 dashboards | telemetry |

### → E96 ⚠ DARK DONOR RESEARCH takes
| From the (renamed) dark-donor blueprint | Role at E96 |
|---|---|
| `dark_recovery.py` — Path B' smart fallbacks + C₂ adjacency-merge | recovery mathematics |
| `dark_donor_drift.yaml` (dark_rate > 0.45 on batch) | drift alarm to E20 |
| research info · identify transactions · edit/correct · resubmit (Ed's dictation) | the repair loop → silver/gold |
| dark + recovered labeled exports → E21 | training data product |
| Full inventory & migration checklist | `E95_DARK_DONOR_RECOVERY_BLUEPRINT.md` (banner-retained; renumber E95→E96 on roster confirmation) |

### → E98 CANARY keeps
`canary.py` (`canary_verify`, Ed/Pope/Melanie) + `canary_breach.yaml` (breach →
critical alert, **block_publish**) + the E98 database canary (cluster 372171: 147 /
$332,631.30) asserted before any commit toward the donor database. DRY RUN LAW applies.

### → E21 plug-ins (serving the gang, owned by E21)
nickname_classifier · fuzzy_threshold (per-zip Levenshtein) · anomaly_detector.

## 5. Legacy accounting — the huge NCSBE process, and the FEC gap

- **The huge donor-file process built for NCSBE state files = the legacy Stage 1–2
  scripts**, named in the E61 blueprint's replacement table: *Stage 1 NCBOE loader
  script*, *ad-hoc match passes `stage2_pass1`–`pass6`*, scattered cluster SQL,
  per-source manual normalization, no quarantine (bad rows silently dropped), no
  lineage. These retire at cutover — their duties are the E99/E95 stations above.
- **FEC: a distinct FEC process is NOT in E61** — FEC appears only as a listed future
  input, with zero FEC-specific logic. If a separate FEC process exists, it lives in
  the repo; LOCATE IT during migration before writing E99's FEC source template.
- E61's own open question #1 ("Number lock… override if you've assigned E61 to
  something else") was never answered — the root cause of the E61 cramming. Question 3
  below exists so the E9x gang never repeats it.

## 6. Pipeline acceptance test (inherited, hardened)

Re-ingest `republican-party-committees-2015-2026.csv` (local copy:
`2015-2026-pacs-nc.csv.zip` at Drive root) through the FULL chain E99 → E95:
**match rate ≥ 75%** (legacy process achieved 39%), **all canaries intact**, quarantine
reasons populated for every excluded row, lineage traceable for every published row.
The pipeline does not take live traffic until this gate passes. The 105 NCBOE
clerk-error rows and the SIIRE fixtures (broyhill_variants, renee_hill_doublespace,
tate_corporate_lookalike) come along as gate tests at E99.

## 7. Rules of the gang

1. **E99 is separate from all others** (hard rule): the only ecosystem touching raw
   external files; no other ecosystem reads from or writes into it; one-way stamped
   output forward. No convenience coupling, ever.
2. **One-way flow.** E99 → E95 → (clean → silver/gold; dark → E96 → resubmit →
   silver/gold). No stage reaches backward.
3. **Medallion discipline.** Raw at the gate; silver/gold in the donor database;
   nothing skips a stage; dark donors are repaired and re-enter with full lineage,
   never discarded.
4. **Canary before commit** at every stage that writes toward the donor database.
5. **Keyless** (standing rule): no stored credentials anywhere in the gang.

## 8. ROSTER VERIFIED 2026-10-02 (local checkout `~/BroyhillGOP`, ECO_CANONICAL)

| # | Canonical state (first-hand read) | Charter consequence |
|---|---|---|
| **E95** | **FREE** — no ECO_CANONICAL dir, no AGENT_BOOT shelf | dedupe/rollup/dark-pool lands here ✔ |
| **E96** | **OCCUPIED — Central Donor Lens / Dynamic Single-Instance Lens** (Ed's ruling 2026-08-18). It IS the `Lens(path)` term over BEHAVIORAL_WHOPPER ⊗ DONATION_WHOPPER; reads donor tables read-only via `rnc_regid` join | NOT available for dark research. E96 is the pipeline's downstream CONSUMER — the gang relation Ed described ("ganged with e96") is supplier→consumer |
| **E97** | **UNCHARTERED placeholder** (Claude audit 2026-08-19) | ⚠ RECOMMENDED home for the Dark Donor Research engine — awaiting Ed's confirmation |
| **E98** | **PARTIALLY DEFINED — "gate behavior"**: likely a suppression/approval gate on E96 contract releases downstream (E96 Phase 1 open question #4 depends on it). NOTE: platform memory also names "E98 Canary" (cluster 372171: 147/$332,631.30) — gate vs canary identity needs Ed's one-line ruling; they may be the same organ (a gate that asserts invariants) | canary/gate station of the gang |
| **E99** | **BUILT AND AUTHORIZED — the ECO_CANONICAL "UNCHARTERED" placeholder is stale (Aug 19 audit).** `ecosystems/e99_source_standardization_front_door/` exists on main: runtime v0.1 PLAN_ONLY, authorized by Ed 2026-09-28 ("build E99 runtime v0.1 PLAN_ONLY"; "Synthesize the Enterprise Ingestion Architecture for E99/E61"). Schema contracts (EXACT_ORDERED match or QUARANTINED_SCHEMA), per-row constraint enforcement, bounded stream parsing with fault rows, NCBOE + FEC evidence envelopes, conduit dedupe ladder L0–L4 (`x_counts_toward_total` so memo/conduit copies never inflate), read-only T1/T3 spine match via SELECT-only relay, faction tagging, two-pass receipts, canary before and after. `e99.lead_score` keeps its namespace. | Ed's Gateway assignment matches a system already in flight — his dictation described a real build |

**⚠ CHAIN CONFLICT TO RULE ON (flagged per the flag-conflicts rule):** the built
E99's README declares the chain **E99 → E61 → E98 → E96** (E99 calls E61
`normalize_row` + the E61-A fec_cand_adapter; E61 is still a live number in the
built system). Ed's 2026-10-02 dictation declares **E99 → E95 (dedupe/rollup/
dark pool) → E96/E97 (dark research) → silver/gold**, with E61 folding away.
Partial overlap already exists: E99's conduit ladder + REDUNDANT/
REDUNDANT_CANDIDATE decisions perform file- and row-level dedupe that the
dictation assigns to E95. **RULED (Ed, 2026-10-02, "authorized and dedupe"):** the built E99 is
AUTHORIZED as it stands, and **E95 is the dedupe/rollup stage** downstream of
it — E99's conduit ladder remains in-file/in-run evidence dedupe
(sub_id/amendment/memo/conduit, never summing one gift twice); E95 owns
cross-file, cross-run, cross-source dedupe + rollup, and pools/marks the dark
donors (per the dictated architecture). Still open: (a) whether E61's
`normalize_row` role folds into E95 or stays E61 (built code calls E61 today;
no change until Ed rules and the code moves in one PR); (c) E98 gate-vs-canary.

**Whopper, canonically:** per `E96_WHOPPER_INTEGRATION_ADDENDUM_2026-08-18.md`, the
Whoppers are the two donor matrices — Behavioral Whopper (Half A, who the person is)
and Donation Whopper (Half B, giving behavior) — never flattened into one table. So
§2's rule reads precisely: every file feeding either Whopper half, bulk or patty,
passes the E99 gate.

**Dark donor code LOCATED (first-hand, `~/BroyhillGOP` + July 1 split ruling):**
`ecosystems/e61_source_ingestion_identity_resolution/sql/002_match_dark_donor.sql`
(beside `001_e61_complete.sql` and `003_signal_taxonomy_and_charity_ddl.sql`).
Deployment truth per the 2026-07-01 relay proof in
`docs/canonical/E60_E61_E72_SPLIT_RULING_2026-07-01.md`: **E61's core `e61.*`
schema was NOT live on Hetzner; only `match.dark_donor` existed, empty (0 rows)**
— so the E9x move is spec + SQL + the empty live table, not a running system.
Re-verify live state before migration (CLAIM/PROOF/VERIFIED).

**Legacy code LOCATED (first-hand, `~/BroyhillGOP`):**
- NCSBE huge process = `scripts/committee_ingestion_v4_stage2_*` family
  (safe_person_apply / completion_dryrun / safe_person_dryrun, + DESKTOP_RECOVERY
  copies, Apr 26 runbooks, and `docs/canonical/STAGE1_FORENSIC_RECONSTRUCTION_AUDIT_2026-09-12.md`).
- **FEC process EXISTS and is separate, exactly as Ed recalled:**
  `pipeline/fec_raw_import.py`, `pipeline/fec_nc_republican_donors.py`,
  `database/process_fec_donors.py`, `DESKTOP_RECOVERY/fec_pac_bulk_pull_cursor.py` —
  plus the scar tissue proving the post-mortem: `fix_01_fec_committees_party_column.sql`,
  `fix_04_fec_corrupt_dates.sql`, `fix_07_fec_party_committee_date_cast.sql`.
  Both processes route into E99/E95 at cutover.
- `docs/canonical/E11_CFO_DECISION_2026-10-02.md` verified verbatim: E11 = CFO,
  E87 = C/B/V (standalone auditor feeding E11), E60 overlay retired.

## 9. Remaining for Ed

1. Confirm **E97** for the Dark Donor Research engine (E96 is taken; alternative is a
   module inside E95).
2. One-line ruling on **E98: gate, canary, or one organ doing both** (E96's Phase 1
   build is blocked on this same question, per its own open question #4).
3. E61 SIIRE fold-in (recommended) and E60 poll/survey SQL + Dataiku placement.
4. RNC_REGID stamping at E99: v2 ids (`rnc_regid_v2`) or original?
5. Register E95/E97/E99 charters in ECO_CANONICAL + E100 index in the migration PR —
   in writing, so the E61 mistake cannot repeat.
