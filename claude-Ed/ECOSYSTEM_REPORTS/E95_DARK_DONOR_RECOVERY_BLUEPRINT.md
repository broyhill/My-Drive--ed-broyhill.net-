> **⚠ RE-STATIONED — 2026-10-02 (Ed's dictated pipeline; roster verified same day).**
> This engine is STAGE 3 of the E9x pipeline (E95 is dedupe/rollup/dark-pool).
> Roster check in `~/BroyhillGOP/ECO_CANONICAL`: **E96 is OCCUPIED (Central Donor
> Lens, Ed's 2026-08-18 ruling) — destination is now E97 ⚠ (unchartered, free),
> awaiting Ed's confirmation.** Single source of truth:
> `E9X_INGESTION_PIPELINE_CHARTER_2026-10-02.md` (same folder). Inventory and
> migration checklist below remain valid; rename this file on confirmation.

# E95 → E97 ⚠ — Dark Donor Recovery Engine (DDRE)

**Version:** 0.1 (2026-10-02, Claude extraction draft for Ed's review)
**Status:** SPEC + MIGRATION PLAN ONLY — code move BLOCKED pending GitHub re-auth (`gh auth login`); parent E61 implementation was itself gated on donor identity pipeline Stages 0–5 completing, and that gate carries over
**Owner:** Ed Broyhill | Spec author: Claude | Extracted from: `E61_SOURCE_INGESTION_IDENTITY_RESOLUTION_BLUEPRINT.md` v1.0 (2026-04-27)
**Universe:** DATA (ingestion family — Ed's ruling 2026-10-02: E95 sits next to the other ingestion ecosystems)
**Numbering authority:** E95 assigned by Ed, 2026-10-02, this session. Roster-registry cross-check against E85–E101 PENDING repo access.

---

## 1. Why this is its own ecosystem

Ed's ruling 2026-10-02: the dark donor functionality moves out of E61 and becomes E95.

E61 (SIIRE) answers: *"can this row be matched to a canonical identity?"* via the T1–T6
ladder. E95 answers a different question with different data, different models, and a
different risk profile: *"what can be recovered, learned, and monitored from the rows
that could NOT be matched?"* Dark rows are a population, not an error state. They have
their own drift behavior, their own training value for E21, and their own recovery
mathematics (Path B' smart fallbacks + C₂ adjacency-merge). Keeping them inside E61
couples the match ladder's correctness to the recovery heuristics' looseness — exactly
the kind of intent-cramming that buried CFO under E61 in the first place.

## 2. What moves from E61 to E95 (the complete inventory)

Every item below is cited to the E61 blueprint; nothing else moves.

| # | Artifact (E61 blueprint location) | Becomes in E95 |
|---|---|---|
| 1 | `python/e61/dark_recovery.py` — Path B' (smart fallbacks) + C₂ (adjacency-merge) on residual (blueprint line 120; flow step 8, line 399) | `python/e95/recovery.py` — same algorithms, invoked via contract instead of in-process |
| 2 | `brain/rules/dark_donor_drift.yaml` — dark_rate > 0.45 on batch sources → E20 warning (lines 158, 421–427) | `brain/rules/e95_dark_donor_drift.yaml` — E95 owns the rule; E20 subscription unchanged |
| 3 | Dark metrics inside `ingestion_run.metrics` JSONB — `dark_clusters`, `dark_rate` (line 226) | E95 run metrics; E61 keeps only `residual_count` (how many rows it handed off) |
| 4 | `match_tier = NULL` ("dark") semantics on resolved rows (line 239) | E95 dark-cluster registry; E61 rows carry `handed_to_e95_run_id` instead of bare NULL |
| 5 | E61 → E21 monthly bulk export of `(matched, dark, quarantined)` labeled rows (line 89) | Splits: E61 exports `matched` + `quarantined`; **E95 exports `dark` + `recovered`** — the dark labels are E95's product |

## 3. What stays in E61 (explicitly NOT moving)

Normalization, address component parsing, cluster_id_v2 assignment, the T1–T6
DataTrust match ladder, quarantine routing, lineage stamping, canary verification
(Ed/Pope/Melanie), publish-to-canonical, and all E61 API endpoints. E61's mission
statement is unchanged; it loses one step (old step 8) and gains one handoff.

## 4. Contracts (the seams)

```
E61 ──[residual handoff: unmatched rows + normalize/parse artifacts]──> E95
E95 ──[recovered matches, with recovery_method + confidence]──> E61 re-entry queue
                                                                 (re-verified by E61
                                                                  canary + publish path —
                                                                  E95 NEVER writes to
                                                                  core.* directly)
E95 ──[dark + recovered labeled rows, monthly]──> E21 (model retraining)
E95 ──[dark_donor_drift events]──> E20 Brain (rule family unchanged)
E95 ──[dark_rate, recovery_rate, dark_cluster_count]──> E27 dashboards
```

Contract rules:
- **One direction of trust.** E95 output is a *suggestion* with confidence attached;
  only E61's canary-verified publish path writes canonical records. This preserves the
  canary invariant (Ed/Pope/Melanie, and the E98 cluster 372171 database canary) with a
  single enforcement point.
- **Handoff is by immutable reference** — E61 run_id + file_sha256 + row ids, not
  copied payloads, so lineage stays single-sourced in E61's archive.
- **Keyless** (binding rule): E95 inherits E61's deployment surface; no new stored
  credentials anywhere.

## 5. Migration checklist (execute when repo access returns)

1. `gh auth login` (Ed) — the current token is invalid; GitHub MCP and gh CLI both fail.
2. Verify what actually exists in `ecosystems/e61_source_ingestion_identity_resolution/`:
   spec only, skeleton, or working code. **Nothing below proceeds on assumption.**
3. Cross-check the current roster for E85–E101: confirm E95 is unclaimed and identify
   the neighboring ingestion ecosystems Ed referenced. If E95 is occupied, STOP and
   report to Ed — do not take the next free number.
4. Create `ecosystems/e95_dark_donor_recovery/` mirroring E61's 7-layer layout
   (sql/, python/e95/, api/, ml/, brain/, admin/, tests/).
5. `git mv` the five inventory items (§2) — move, never copy-and-drift; E100 COPY-ONLY
   seal applies to the protected piles, but the live ecosystem tree move is a normal
   git operation recorded in one PR.
6. Patch E61's `ingest.py` step 8 from in-process `dark_recovery()` call to the E95
   handoff contract (§4).
7. Update `dark_donor_drift.yaml` event source from `e61.run.completed` to
   `e95.run.completed`.
8. Register E95 in the canonical roster + E100 index in the same PR (treasure-map rule:
   new trees are registered where they're created, so no future session rediscovers
   this from scratch).
9. Tests move with code: any dark-recovery fixtures/tests follow `recovery.py`; E61
   keeps the match-ladder tests (broyhill_variants, renee_hill, tate_corporate).
10. Verification per zero-trust standard: E61 run on a fixture file must produce
    identical published records before and after the split (the split changes topology,
    not results), with the handoff visible in lineage. Canary assertion before any
    commit touching staging data.

## 5b. Revision under discussion (Ed, 2026-10-02, not yet ruled)

Ed is considering a larger shape: **combine E61 SIIRE + the E60 ingestion pieces
(poll/survey SQL, Dataiku folder) into E95** as the unified ingestion ecosystem,
ganged with E96 and E98 (canary). If ruled, this blueprint is rewritten from
"dark-donor extraction" to "combined ingestion charter," and dark recovery becomes
either (a) a module inside E95 or (b) its own gang number — E96 if free/suitable.

**HARD RULE (Ed, 2026-10-02): E99 stays separate from all others.** Nothing gangs
into E99, nothing wires to it, no matter how convenient the adjacency looks.

## 6. Open questions for Ed

1. Which ecosystems are the "other ingestion ecos" adjacent to E95? (Roster not
   locally verifiable; needed for step 3.)
2. Does E95 also absorb future dark-money/dark-entity research scope, or strictly the
   unmatched-row recovery population? This draft scopes it strictly to the latter.
3. Does the E61 SIIRE implementation gate (donor pipeline Stages 0–5) still hold as of
   October 2026?
