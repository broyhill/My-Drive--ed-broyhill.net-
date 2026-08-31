# BROYHILLGOP — AGENT BOOT FILE

**Supersedes:** `BroyhillGOP-Constitution-v2.md` (Jan 4 2026) · `BROYHILLGOP STARTUP
CONSTITUTION & SYSTEM GUIDE.md` (Jan 11 2026). Both are retained, banner-marked, unedited
below their banners. Nothing deleted — E100 is copy-only.

---

## JURISDICTION — read this before you trust a single line below

| | |
|---|---|
| **Written by** | Claude Code, running on Ed's MacBook, in `~/My Drive (ed@broyhill.net)` |
| **Written** | 2026-08-27 |
| **Could see** | Local filesystem · Bash · root SSH to both Hetzner boxes · `~/.claude` config and session transcripts |
| **Could NOT see at time of writing** | **AX162 and AX41 were DARK** — 100% ICMP loss, TCP refused on 22/443. Every remote fact below is CITED FROM PRIOR MEASUREMENT, not measured at authorship. Re-measure before quoting. |

**Why this header exists.** The document this replaces contained the line *"Claude SHALL
NOT use `bash_tool` — does not exist in this environment."* That was **true** — for the
Claude that wrote it, in a chat window, with no filesystem. It became false and dangerous
when it was filed as universal law and inherited by an agent with root SSH to production.
A rule without its jurisdiction is a trap. **Every rule below ends in a command that proves
or kills it. A rule you have not run is a rule you have not read.**

**Claude Code and claude.ai share the model and nothing else** — no memory, no filesystem,
no tools. This file, sitting in Drive, is the only substrate both can read. Keep it here.

---

## §0.0 — OPEN AGENDA. DO THIS BEFORE ANY NEW WORK.

**Ed's standing instruction, 2026-08-30: this is first on the agenda.** Not after the
boot sequence produces something interesting. First. If you finish a session without
either doing these or telling Ed plainly why not, the session failed regardless of what
else it produced.

### A1 — REGISTER THE TWO HOOKS. Highest leverage item on the platform.
`~/.claude/hooks/bgop-boot.sh` and `~/.claude/hooks/bgop-write-gate.py` are written,
tested, correct, and **inert** — unregistered across five sessions now. Memory is advisory
context an agent weighs. **Hooks execute whether the agent agrees or not.** This is the only
enforcement on this platform that does not depend on an agent choosing to comply, and
2026-08-30 was an eight-error demonstration of what that choice is worth.

Registration holds root SSH to production, so it is **Ed's call, not the agent's.** The
agent's duty is to ASK, every session, until Ed answers yes or no.
```bash
python3 -c "import json;d=json.load(open('/Users/Broyhill/.claude/settings.json'));print('hooks registered:', 'hooks' in d)"
# False = still inert. Ask Ed. The one-line install is in §5.
```

### A2 — BOOT PROTOCOLS FROM `origin/main`, NOT THE WORKING TREE. One line, kills a whole failure class.
See §7.5. The tree is on whoever's branch was last checked out and silently serves stale law.

### A3 — GENERATE THE BOOT CHAIN INSTEAD OF HAND-WRITING IT.
A binding document cannot fail to be cited if the citation list is generated from the
canonical set. `scripts/agent_files.py` already does exactly this for per-agent files —
extend the pattern. This is what let `NO_DRYRUN_BURIAL_PROTOCOL` be binding and invisible
for five weeks (§7.2).

### A4 — DRY-RUN LEDGER NEEDS `target_date` + `owner`, AND A `WITHDRAWN` STATE.
Zero of 309 ledger rows have ever resolved out of `PENDING_AUTHORIZATION`; 113 sit in
`UNRECONCILED`, which is indefinite by construction. Parking costs nothing today. That is
the whole reason the pile exists — see the analysis below.

### A5 — DEPLOY THE DRAIN PATCH **DARK**. Upstream of everything, and not blocked by what people think.
`backend/python/brain/brain_drain_patch.py` — 335 lines, written by Devin, sitting since at
least 2026-07-28. SPINE_INDEX Tier 3 calls it *"item 1 on the critical path."*

**It is not blocked by R4.** Its own gate, read 2026-08-30:
> *"When dark: observe_log written, **no HTTP dispatch**, journey row written with
> `journey_state='queued'`."*

`SENDS_ENABLED = False` means **no external call of any kind**. Every blocker the patch
lists — R4 opponent-donor suppression, Ed's `I AUTHORIZE THIS ACTION` for sends, W03-first —
gates `SENDS_ENABLED = True`. **None of them gates deploying it dark.** Those 4,887 stuck
`brain.weapon_queue` rows could have been producing journey rows and outcome measurement for
a month with nothing leaving the building.

**Its one real blocker is a ten-second check.** The patch documents a 17-column
`brain.weapon_queue` contract "verified 2026-07-25." Corroborated 2026-08-30 from an
independent same-date artifact — `database/migrations/dryrun/2026-07-25_BRAIN_GATED_SEND_DRYRUN_BOLIEK_TYPE_B1.sql`
names 13 columns on INSERT; the 4 missing (`executor_endpoint`, `http_response`,
`dispatched_at`, `delivered_at`) are dispatch-time. 13 + 4 = 17. Two sources agree.
Repo evidence is not live proof — confirm and go:
```bash
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -c '\d brain.weapon_queue'"
# 17 columns matching the patch header -> deploy dark. Mismatch -> stop, reconcile.
```
Also verify the other three Mountain 2 files against live schema (patch header names this).

**Why it outranks A6 (E70):** the drain fix unblocks measurement across all sixteen guns. E70 is one
gun and sits behind it. Do not build more guns onto a queue that does not drain.

### A6 — E70 / W13 DIGITAL AD ENGINE. THREE FILES. NAMED, HASHED, IN ORDER.
**Ed 2026-08-31: "store safely all three and make it popup everywhere tomorrow."** This is
that popup. Branch `claude/e70-apply-paths-2026-08-31`.

Moved out of `dryrun/` onto discoverable numbered paths per
`NO_DRYRUN_BURIAL_PROTOCOL_2026-07-24`. **Content byte-identical — sha256 unchanged — so
every prior review still refers to these exact bytes.** Numeric order IS apply order.

| # | Path | sha256 | What |
|---|---|---|---|
| 1 | `database/migrations/326_e70_digital_ad_engine_v1.sql` | `4fdb6e4c076d3586` | 11 tables, 2 views, 9 RLS, 3 seed rails. New `e70` schema, additive. |
| 2 | `database/migrations/327_e70_pricing_v1.sql` | `bf82de923325d196` | 5 tables, 2 views, `e70.price_line()`, immutability trigger. Needs #1. |
| 3 | `database/migrations/328_e70_executor_registration_v1.sql` | `02c1b8b544d10e7d` | 1 row -> `brain.action_executors`, `enabled=false`. **Only contended write.** Last. |

Read first: `database/migrations/E70_APPLY_ORDER_READ_FIRST.md`.

**Gates, none of them optional.** Collision check — **never run**, Hetzner account-blocked
since 2026-08-27 (`nc -z 37.27.169.232 5432` still closed 08-31). Canary 147 / $332,631.30
before and after each file. Second non-author APPROVE. Ed's `Authorized: apply <file>`, one
file at a time.

**HAZARD — do not apply from `cursor/e70-apply-ready-fd4b`.** It carries `.APPLY_READY`
markers on pricing at sha `87c6d015dedb0f4b`, the **pre-rev2** file. Its DDL is
byte-identical to `327` so it would not break the database, but its header asserts *"E87 was
SUPERSEDED into E60 / E60 OWNS cost accounting"* — voided **by name** by
`ECO_IDENTITY_FREEZE_WIZARD_FIRST_2026-08-11`. The ledger already records that Cursor
APPROVE as VOID at rev2, so `327` needs re-approval. Three separate systems reached that
wrong answer from the stale redirect banner in `ECO_CANONICAL/E87/ECO_MODEL.md`; the banner
is corrected on `claude/e25-posts-threads-2026-08-30`, **not yet merged**.

### A7 — E25 PORTAL, TWO DRY RUNS. Branch `claude/e25-posts-threads-2026-08-30`.
`324_e25_posts_threads.sql` (threaded discussion — the one Circle primitive the 45 live
`e25` tables lacked) and `325_e25_townhall_live_stage.sql` (stage, chat, Q&A votes,
ephemeral ingest grants). Order-independent; both end in `ROLLBACK`. Same branch carries
the E87 banners and `E25_COMMUNITY_BRIEF_FOR_BLIND_AGENTS_2026-08-30.md` — **paste that
brief into any agent that cannot read the repo**, or it will propose a parallel
`e25_community_spaces` schema, as five consecutive drafts did on 08-30.

**FIVE dry runs are now open at once.** Ed's own rule is one. That is the backlog forming
behind a single authorizer — the exact pattern that put 309 rows in the ledger with none
ever resolving. Worth ruling on whether additive + guarded + canaried migrations get a
delegated path, rather than letting the answer happen by default.

**Full analysis of why work is lost here — five measured mechanisms, eight ranked fixes:**
`https://claude.ai/code/artifact/d75fabc1-d6e5-444e-9a3e-338f6b56ee0b`

---

## §0 — THE TRIGGER

The word **`startup`**, alone, is the trigger for this file. So are `boot`, `protocols`,
and the first BroyhillGOP-shaped prompt of any session.

**Describing this checklist does not count as running it.** Summarizing it, listing what
you "would" check, or saying you are "ready to" run it are all the same failure and it is
the single most-repeated failure in this project's history (four consecutive sessions,
2026-08-24 through 08-27). Either the measured output is in your context, or you say
plainly that you have not measured.

---

## §1 — BOOT SEQUENCE

Run in order. Paste the output. No output, no claims.

```bash
# 1. Are the boxes even up? Everything else depends on this.
ping -c 2 -W 3000 37.27.169.232 ; nc -z -G 5 37.27.169.232 22 && echo AX162-UP || echo AX162-DOWN
ping -c 2 -W 3000 144.76.219.24 ; nc -z -G 5 144.76.219.24 22 && echo AX41-UP  || echo AX41-DOWN

# 2. Canary. Must be exactly 147 / 332631.30. Before AND after any DB work.
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -At -c \
  \"SELECT COUNT(*), SUM(amount::numeric) FROM raw.ncboe_donations WHERE cluster_id=372171;\""

# 3. Coupling map — read the JSON, never the .md mirror.
ssh root@37.27.169.232 "python3 -c \"import json;d=json.load(open('/opt/bgop/knowledge/out/db_coupling.json'));\
print(d['meta'],len(d['nodes']),len(d['edges']),len(d['contention']))\""

# 4. Live counts. Real counts — never pg_stat.
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -At -c \
  \"SELECT 'catalog',COUNT(*) FROM e84.document_catalog UNION ALL \
    SELECT 'chunks',COUNT(*) FROM e84.document_chunks UNION ALL \
    SELECT 'spine',COUNT(*) FROM spine.donor_transactions UNION ALL \
    SELECT 'profiles',COUNT(*) FROM core.donor_profile;\""
```

**If the boxes are down** (they have been since 2026-08-27): say so — then run step 3 from
GitHub instead, which needs no server and no key. Do not substitute remembered numbers for
measured ones.

```bash
# Coupling map, keyless, works while Hetzner is dark. Authoritative source.
git -C ~/BroyhillGOP fetch origin main --quiet
git -C ~/BroyhillGOP show origin/main:docs/graphify/db_coupling_v3.json.gz | gunzip \
| python3 -c "import json,sys;d=json.load(sys.stdin);print(d['meta']);\
print({k:len(d[k]) for k in ('nodes','edges','handoffs','contention','write_only','read_only')})"
# Expect: ecosystems 41 · db_objects 1119 · edges 1679 · handoffs 252 · contention 258

# The graph itself, same place:
git -C ~/BroyhillGOP show origin/main:docs/graphify/GRAPH_REPORT.md | head -20
# Expect: 513,640 nodes · 627,361 edges · 33,717 communities (built 2026-08-09, commit c22af361)
```

Steps 2 and 4 (canary, live row counts) have **no** offline substitute. If the boxes are
down, those are simply unmeasured — say `UNAVAILABLE-HETZNER-DARK` and quote nothing.

---

## §2 — STANDING LAW

Each rule carries the command that verifies it. Run the command; do not inherit the rule.

### 2.1 The database is `postgres`. There is no `broyhillgop` database.
```bash
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -At -c 'SELECT datname FROM pg_database;'"
```

### 2.2 `pg_stat_user_tables.n_live_tup` is a stale cache and it lies.
Ed: *"you are looking at wrong cache."* Take real `COUNT(*)`.
```bash
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -At -c \
  \"SELECT relname,n_live_tup FROM pg_stat_user_tables WHERE relname='donor_profile';\" ; \
  sudo -u postgres psql -d postgres -At -c 'SELECT COUNT(*) FROM core.donor_profile;'"
# The two numbers differ. That difference is the rule.
```

### 2.3 `raw.ncboe_donations` is READ-ONLY FOREVER.
It is the canary source. Never write to it under any circumstance.

### 2.4 THE WRITE GATE — DRY RUN LAW.
Every SQL change goes to `database/migrations/dryrun/` via PR and stays
**PENDING_AUTHORIZATION until Ed says "Authorized" against that specific file name.**
After deploy, stamp the file with a DEPLOYED header, deploy_id, and canary result.
A dry run sitting in `/root/` is off-protocol and cannot be authorized at all.

**THIS RULE IS ONLY HALF THE DUTY.** `docs/canonical/NO_DRYRUN_BURIAL_PROTOCOL_2026-07-24.md`
(binding, Ed) adds the other half: apply-ready SQL must ALSO live on a **discoverable** path
(`database/migrations/NNN_*.sql`) and you must issue an **AUTHORIZATION ASK the same
session**. `dryrun/` is for scratch and superseded drafts only. Parking finished work there
and waiting to be discovered is the prohibited act — see §7.2. Writing to dryrun and
stopping is not compliance, it is the violation.
```bash
ssh root@37.27.169.232 "ls -la /opt/bgop/git_main/database/migrations/dryrun/ | tail -20"
```

### 2.5 `I AUTHORIZE THIS ACTION` is NOT the database gate.
It is the **E100 file seal** — deletes, renames, moves. CI-enforced at
`.github/workflows/e100-memory-seal.yml`. The DB gate is "Authorized" against a named
dryrun file. **Do not conflate them.** (Conflating them is a recorded past error.)

### 2.6 No stored credentials. Anywhere.
Ed, 2026-08-23: *"i refuse keys anywhere. its stupid."* Not in `.env`, not in a systemd
`EnvironmentFile`, not inline at runtime. When a capability appears to need a key, the
answer is a **keyless architecture**, not a better-hidden key.
```bash
ssh root@37.27.169.232 "ls -la /etc/broyhillgop/ 2>/dev/null; echo '---'; env | grep -ci 'API_KEY\|TOKEN\|SECRET'"
```

### 2.7 Grep the coupling map before proposing ANY write.
On 2026-08-21, four proposed writes hit four contended tables. Contention is invisible from
table contents — only the map shows it.
**CORRECTED 2026-08-29 — this rule previously pointed at the wrong file.** It grepped
`~/.claude/hooks/bgop-coupling.json`, the **v1** parse: **91** contention objects. The
authoritative **v3** parse reports **258**, and it disagrees on writer *sets*, not just
totals:

| Object | v1 (old, wrong) | v3 (authoritative) |
|---|---|---|
| `brain.triggers` | E20 E39 E64 E79 | E14 E20 E39 **E40** E64 E79 |
| `brain.event_queue` | E20 E56 E61 | E20 **E48** E56 E61 |
| `e13.donor_issue_intensity` | E13 **E40** E59 | E13 **E20** E59 **E64** |

Any table called "safe to write" on v1 evidence must be re-checked. **v3 lives in GitHub,
so this check runs while Hetzner is dark.**

```bash
# Authoritative: docs/graphify/db_coupling_v3.json.gz on origin/main. Keyless, no server.
git -C ~/BroyhillGOP fetch origin main --quiet
git -C ~/BroyhillGOP show origin/main:docs/graphify/db_coupling_v3.json.gz | gunzip \
| python3 -c "import json,sys;d=json.load(sys.stdin);t=sys.argv[1].lower();\
c=[x for x in d['contention'] if x['object'].lower()==t];\
print('contention objects in map:',len(d['contention']),'(expect 258)');\
print(t,'->',c[0]['writers'] if c else 'no competing writer / not in map')" brain.event_queue
```

**Do NOT substitute:** `db_coupling_report_v3.md` (human summary, lists ~40 of the 258
rows) · `docs/graphify/ci/db_coupling-ci-*.json.gz` (built from v2, only 148) ·
`~/.claude/hooks/bgop-coupling.json` (v1, 91 — retained as
`bgop-coupling.json.bak-v1-stale`). If a map reports fewer than 200 contention objects,
it is the wrong file.

### 2.8 Never scope a candidate universe by `core.candidates.office_level`.
The 2-row **lowercase** `state` bucket holds Boliek (Oct 2026 pilot) and Zenger (the only
live `e25.portal_tenants` row). `WHERE office_level='STATE_EXEC'` silently drops Boliek.
Scope by name or `office_title`.
```bash
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -c \
  'SELECT office_level, COUNT(*) FROM core.candidates GROUP BY 1 ORDER BY 2 DESC;'"
# Print this distribution before trusting any scope. Every time.
```

### 2.9 Search four ways before saying anything is missing.
path LIKE · FTS phrase · FTS token · `body LIKE` ranked by `authority_weight`.
**One query is not a search.** Then check `ECO_CANONICAL/E100/TREASURE_MAP_INDEX.md` and
`git log --all` before the word "no record" leaves your mouth.
```bash
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -At -c \
  \"SELECT canonical_path, authority_class FROM e84.hybrid_search('YOUR TERM', NULL, 20, 20, NULL, NULL);\""
# hybrid_search with a NULL vector is SAFE — degrades to keyword mode. ~131ms. Keyless.
```

### 2.10 Build; do not redesign.
The December 2025 specs are a complete enterprise design (33 systems, 500+ pages) that sat
unread while a skeleton was built against them. **Ed's ruling 2026-08-11: don't
reconstruct — ELEVATE.** December is the target, AS_DEPLOYED is the baseline, the gap is
the backlog. Check for a subsystem's December spec before designing it.
`AS_EXECUTED_*` / `AS_DEPLOYED_*` in a filename means **it is live.**

---

## §3 — CLAIM TYPING

Every load-bearing statement carries a visible type:

- **MEASURED** — query + timestamp + result. Quotable.
- **CITED** — file path + line. Quotable.
- **DERIVED** — follows from the above, derivation shown.
- **ASSUMED** — none of the above. **Must be labeled inline.**

An unlabeled claim silently asserts MEASURED or CITED. That is the lie. There is no
internal signal separating retrieved from reconstructed — both feel identical from the
inside — so the label has to be mechanical, not felt.

`ops.claims` (`agent`, `claim_text`, `verification_sql`, `expected_result`, `status`,
`actual_result`, `deploy_id`) is the falsifiability harness and holds **1 row**. Any
non-trivial assertion should get a row with the SQL that would disprove it.
```bash
ssh root@37.27.169.232 "sudo -u postgres psql -d postgres -c 'SELECT * FROM ops.claims;'"
```

---

## §4 — END OF SESSION

Run `BroyhillGOP/End-of-Session Verification Protocol.md`. Itemize every factual claim
made; re-verify each with a tool; for any that fails, print **Original claim /
Verification result / Correction.** Close the canary.

---

## §5 — ENFORCEMENT OUTSIDE JUDGMENT

Memory is advisory context an agent gets to weigh. Hooks are executed by the harness
whether the agent feels like it or not.

| Hook | Purpose | Status |
|---|---|---|
| `~/.claude/hooks/bgop-boot.sh` | SessionStart directive + live measurement injected before the first token | **WRITTEN, TESTED, NOT REGISTERED** |
| `~/.claude/hooks/bgop-write-gate.py` | PreToolUse. Denies writes to `raw.ncboe_donations`; asks with the writer list on any contended object; fails closed | **WRITTEN, TESTED, NOT REGISTERED — repointed to v3 on 2026-08-29** |

**2026-08-29 — the gate was reading the wrong map and would have under-blocked.** It
sourced `AX162:/opt/bgop/knowledge/out/db_coupling.json` (v1, 91 contention) and cached it
at `bgop-coupling.json`. It now reads `docs/graphify/db_coupling_v3.json.gz` from
`origin/main` (258 contention), caches to `bgop-coupling-v3.json`, refreshes over `git`
rather than `ssh` — so it works while Hetzner is dark — and **fails closed** if handed any
map with fewer than 200 contention objects. Previous version kept at
`bgop-write-gate.py.bak-2026-08-29`. Verified 2026-08-29 on four paths: DENY on
`raw.ncboe_donations` · ASK on `brain.event_queue` listing 4 writers incl. E48 · refuse-to-gate
when a v1 map is substituted · silent pass-through on non-Postgres commands. **Had it been
registered before this fix, it would have cleared `brain.event_queue` as 3-writer and
missed E48 entirely.**

Registration is Ed's call — these hold root SSH to production.
```bash
mkdir -p "/Users/Broyhill/My Drive (ed@broyhill.net)/.claude" && cp /private/tmp/claude-501/-Users-Broyhill-My-Drive--ed-broyhill-net-/2cb5feb7-4623-4175-b07e-f705d53e5c18/scratchpad/bgop-settings.json "/Users/Broyhill/My Drive (ed@broyhill.net)/.claude/settings.json"
```
Requires a Claude Code restart — the settings watcher only watches directories that already
had a settings file at launch.

---

## §7 — KNOWN TRAPS. Each one has caught more than one agent.

A trap is a document that is wrong in a way that reads as authoritative. Reading more
carefully does not help; only knowing in advance does. Each entry names the command that
proves it.

### 7.1 Volume X gives E16 three jobs it does not have. FOUR agents have walked into this.
`BGOP_PLATFORM_PLAN_VOLUME_X…2026-06-12.md`, E16 purpose, says *"Manages media buying,
station targeting, FCC compliance."* **E16 does none of those.** It is creative production
only. That sentence predates the Band 08 gun architecture by ONE DAY and was never updated.

| The claim | Real owner |
|---|---|
| media buying | the guns, behind the socket — W13 / E70 |
| station targeting | E59 / E77 |
| FCC compliance | E10, via the socket's `requires_compliance` |

Victims so far: the Vibe research misfiled onto the E16 shelf (PR #817), the W08 studio spec
naming Vibe as E16's lead brand, and Claude on 2026-08-30. **CTV/streaming ad BUYING is E70,
not E16.** `OTT` in E16's code is a member of `class AdType` — a creative FORMAT, not a
channel.
```bash
sed -n '82,90p' ~/BroyhillGOP/ECO_CANONICAL/E16/code/AS_DEPLOYED_HETZNER_2026-07-30/ecosystem_16_tv_radio_complete.py
# OTT sits in AdType next to TV_SPOT and PRE_ROLL. 212 production terms vs 6 buy-side.
```

### 7.2 `NO_DRYRUN_BURIAL_PROTOCOL` is BINDING and is cited nowhere in the boot chain.
`docs/canonical/NO_DRYRUN_BURIAL_PROTOCOL_2026-07-24.md` — Ed's, binding on all agents.
It appears in **zero** of the five surfaces an agent reads at boot: this file (until now),
`docs/canonical/protocols/`, that folder's README, `DRY_RUN_PROTOCOL.md`, `SPINE_INDEX.md`.
The only trace is a 430-byte pointer stub in `AGENT_BOOT/`.

**It adds the duty DRY_RUN_PROTOCOL lacks.** Apply-ready SQL goes on a *discoverable* path
(`database/migrations/NNN_*.sql`), not dryrun-only, and you must issue an AUTHORIZATION ASK
**the same session**:

> **AUTHORIZATION ASK** · Ready to apply: `<exact path>` · Effect: … · Does not do: … ·
> Reply with exact phrase: `I AUTHORIZE THIS ACTION`

`dryrun/` holds scratch and superseded drafts ONLY. Parking finished work there and waiting
for Ed to discover it is the prohibited act. **Silence is the violation, not the deploy.**

### 7.3 Declared wiring is mostly not real wiring. Do not trust "Feeds into / Fed by."
MEASURED 2026-08-30, Volume X against `db_coupling_v3.json.gz`: **353 declared edges,
117 real DB handoffs, 15 in both.** 102 real couplings are documented nowhere.

Caveat that matters: the coupling map sees **Postgres only**. E16, E46 and E70 show zero DB
coupling and are *correct* at zero — they couple through files, streams and APIs. A zero
there is not drift. Do not repeat that inference.
```bash
git -C ~/BroyhillGOP show origin/main:docs/graphify/db_coupling_v3.json.gz | gunzip \
| python3 -c "import json,sys;d=json.load(sys.stdin);print(len(d['handoffs']),'real handoffs')"
```

### 7.5 READ PROTOCOLS FROM `origin/main`, NEVER FROM THE LOCAL WORKING TREE.
On 2026-08-30 Claude read `docs/canonical/protocols/DRY_RUN_PROTOCOL.md` in full at startup
and reported it read. The local checkout was on a Cursor feature branch behind `origin/main`
and **did not contain Ed's 2026-08-18 DRY RUN INTENT LAW at all.** The agent then spent the
session writing dry runs with no deployment intent — the exact act that ruling bans — while
believing it had read the governing protocol cover to cover.

A stale checkout does not announce itself. The file opens, reads complete, and is missing a
section you cannot know to look for.

```bash
# Always. The working tree may be on anyone's branch.
git -C ~/BroyhillGOP fetch origin main --quiet
git -C ~/BroyhillGOP show origin/main:docs/canonical/protocols/DRY_RUN_PROTOCOL.md | grep -n "INTENT LAW"
# Expect a hit at ~line 152. No hit = you are reading a stale copy.
git -C ~/BroyhillGOP status -sb | head -1   # shows branch + how far behind
```

**INTENT LAW, the part that binds every session:** a dry run may only be written with intent
to deploy it this session or the next. After writing one the agent does exactly one of two
things — presents it for authorization and deploys, **or** states plainly: *"This dry run is
PENDING_AUTHORIZATION. It deploys next session. Name: <file>."* Silence is the violation.
Only ONE open PENDING dry run at a time without Ed's explicit acknowledgment.

### 7.4 Moving is forbidden; re-tagging is the sanctioned mechanism.
E100 and PROTECTED_ASSETS prohibit delete/rename/move. They **endorse** add, supersede,
re-tag, index. `PARTS_INDEX.md`: *"one physical copy, many tags."* A doc filed under the
wrong eco is not moved — it gains a tag in the right lane and a note in the wrong one.
Reading the prohibition as "do nothing" is itself the error.

---

## §6 — WHAT THIS FILE DOES NOT KNOW

**CITED, not measured** (boxes dark at authorship, last good 2026-08-27 05:00Z):
canary 147 / $332,631.30 · `e84.document_catalog` 19,510 · `e84.document_chunks` 199,390 ·
`spine.donor_transactions` 2,449,580 / $397,224,068.88 · `core.donor_profile` 246,965 ·
`core.datatrust_voter_nc` 7,727,637 · coupling 1,221 objects / 1,575 edges / 303 handoffs /
91 contention / 1,002 write-only.

**UNRESOLVED — raise with Ed before quoting either number.** `spine.donor_transactions`
measured 2,449,580 / $397.2M on both 08-26 and 08-27, against a Deploy-162 figure of
3,565,875 / $467.7M. A **-1,116,295 row / -$70.5M divergence that is not explained.**

**Open and unowned:** Gate R4 (Compliance/Legal) — Sept 12 2026 deadline, no owner
assigned. Gate R3 (ML/Brain) is the critical path to the October 2026 Boliek pilot.

**Never verified by anyone:** whether this platform can be restored from backup.
