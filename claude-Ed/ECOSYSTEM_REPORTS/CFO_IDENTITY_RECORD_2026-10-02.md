# CFO IDENTITY — CONTROLLING RECORD (2026-10-02)

**Ruling:** **E11 is the CFO.** Ed changed the decision to E11 on 2026-10-02; the
documented ruling lives on `main` at `docs/canonical/E11_CFO_DECISION_2026-10-02.md`.
No existing file was moved when that ruling was recorded.

## The shape of it

- **E11 owns the money decision:** ceilings, burn, runway, NPV, IRR, attribution.
- **E20 asks E11 before a GO.**
- **E87 is C/B/V** (cost/benefit/variance) — its own ecosystem, independent, feeding E11.
- **E60 does not keep the CFO title.**

## E11 already is the CFO function (existing assets)

- The Aug 3 doctrine + math appendix
  (`docs/canonical/E11_CFO_CAPITAL_BUDGETING_DECISION_DOCTRINE_2026-08-03.md`)
- The E11 budget model
- The budget Python: `ecosystem_11_budget_management_complete.py`, the earlier budget
  module, and the dual-grading patch
- The finance-advisor spec = the Wizard-facing voice of the same function

## Stale references awaiting a banner pass (Ed's timing)

Twenty-three documents still say E60 is the CFO — including the June 30 architecture,
the June 29 E60 spec, the master plan, and `ECO_CANONICAL/E60/ECO_MODEL.md`.
**They stay put until Ed wants a banner pass.** Do not treat any of them as current on
the CFO question.

## E60 shelf — placement open (Ed's call, NOT part of the CFO shelf)

1. Psychology: `docs/canonical/E60_ADDICTION_PSYCHOLOGY_ENGAGEMENT_ENGINE_CANONICAL_2026-07-01.md`
   — **Ed, 2026-10-02: very important; highest-stakes item on this shelf.** This is a
   political platform: candidate users WILL lose elections if left to their own "good
   judgment" in donor communication (polite, rational appeals don't move donors). The
   psychology engine is what the E61 funnel's TRIGGER/ENGAGE stages and E20's issue
   selection depend on to convert. It needs a real home with its own number — parking
   it under E60's dead CFO banner invites the same cramming that buried CFO at E61.
2. Nervous net, LP solver, cost ledger: `ECO_CANONICAL/E60/code/` and
   `ECO_CANONICAL/E60/sql/200_e60_nervous_net.sql` (DDL staged May 2, never
   applied; still gated on `I AUTHORIZE THIS ACTION`).
   **RULED (Ed, 2026-10-02): the THROTTLE is essential and GOES WITH THE CFO —
   E11 owns the throttle.** Thresholds, rules, and fire receipts are E11's;
   disposition of the pieces:
   - **Throttle AUTHORITY → E11 CFO**: thresholds, ceilings, burn limits, kill
     conditions are money decisions. The throttle is the NO side of the ruled
     "E20 asks E11 before a GO."
   - **Throttle EXECUTION → E20 Brain as T-codes** (where the June 29 E60 spec
     already ruled the IFTTT rules belong). E20 fires E11's rules, never its own.
   - **Throttle EVIDENCE → E11's own records, AUDITED by E87.** Ed's correction
     2026-10-02: **C/B/V is an auditor ONLY** — E87 owns no operational store.
     Every fire writes its receipt into E11's ledger; E87 reads it (read-only)
     and reports cost/benefit/variance on the throttle rules themselves.
   - **Cost ledger → E11** (the CFO owns the book of spend; `core.cost_ledger`
     DDL rehomes under E11). E87 audits the ledger, never writes it.
   - LP solver / investment dial → E11's toolbox.
3. C/B/V bytes under `ECO_CANONICAL/E60/CBV/` — identity home is **E87**
4. Poll/survey SQL and the Dataiku folder under E60
5. The E11 training LMS — a different module on the same shelf. Provenance (Ed,
   2026-10-02): an ORIGINAL ecosystem from August 2025, built for candidate user
   training on NC FIRST — it predates the 2026 roster; "11B Training & Learning
   Management System" in the original roster is this, separate from 11 Budget.
   Not a CFO asset; placement is its own decision.

## C/B/V lineage (per the recovered 2026-07-01 split ruling)

C/B/V has lived at three numbers: **E61** (as "Cost/Benefit/Variance ML Brain
Control": Welford variance, Bayesian Thompson sampling, budget optimizer,
attribution resolver) → **E72** (Campaign Investment Engine, 2026-07-01 ruling;
`database/migrations/004_e72_campaign_investment_engine.sql`, which **still
carries stale `e61` schema names — namespace-clean before any execution**) →
**E87** (Ed's 2026-10-02 ruling, identity home; cf.
`database/migrations/LIVE_E87_VARIANCE_OBSERVE_SEAM_2026-09-23.sql`).
The July ruling also named E60 "Campaign Profit Engine / CFO Controller" —
that is the origin of the E60-CFO title Ed retired on 2026-10-02.

**Still-open E60 sub-collision (March 2026 diamond):** E60-A Addiction
Psychology / Engagement Engine vs E60-B Nervous Net (cost ledger + LP solver).
"Which takes E60, which gets renumbered" was posed 2026-07-01 and never
answered — the same open placement question as shelf items 1 and 2 above.

## Session-history note (why a superseded stub exists beside this file)

Earlier on 2026-10-02 this folder briefly recorded "E60 = CFO" from a verbal summary;
Ed changed the decision to E11 the same day and the documented ruling controls.
`E60_CFO_IDENTITY_RULING_2026-10-02.md` is the superseded stub.

## Related, unchanged by this ruling

- **E95 Dark Donor Recovery** extraction from E61 (`E95_DARK_DONOR_RECOVERY_BLUEPRINT.md`)
  proceeds as drafted; no CFO dependency.
- **Aperture Decision Studio v0.1.0** (verified 20/20 tests) remains the candidate
  decision kernel — now mapped to **E11** (decision half) + **E87** (C/B/V half), with
  the `outcome.recorded` outbox event as the E87→E11 feed and one fix needed for E87
  independence (key outcomes on snapshot hash, not E11-internal run ids).
