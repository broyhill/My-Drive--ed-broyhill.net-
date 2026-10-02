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
   `ECO_CANONICAL/E60/sql/200_e60_nervous_net.sql`
3. C/B/V bytes under `ECO_CANONICAL/E60/CBV/` — identity home is **E87**
4. Poll/survey SQL and the Dataiku folder under E60
5. The E11 training LMS — a different module on the same shelf. Provenance (Ed,
   2026-10-02): an ORIGINAL ecosystem from August 2025, built for candidate user
   training on NC FIRST — it predates the 2026 roster; "11B Training & Learning
   Management System" in the original roster is this, separate from 11 Budget.
   Not a CFO asset; placement is its own decision.

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
