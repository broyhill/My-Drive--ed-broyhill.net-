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

## 8. Open for Ed's confirmation

1. E95 = dedupe/rollup/dark-pool — confirm number.
2. E96 = dark donor research engine — confirm number (then rename the dark-donor
   blueprint file E95→E96).
3. Register E95/E96/E98/E99 in the canonical roster + E100 index in the migration PR —
   answered in writing, so the E61 mistake cannot repeat.
4. Does E61 SIIRE fold in wholesale (recommended per §4) and do the E60 poll/survey
   SQL + Dataiku pieces join the gang or land elsewhere?
5. RNC_REGID stamping at E99: v2 ids (`rnc_regid_v2`) or original?
6. Where is the FEC process code? (§5)
