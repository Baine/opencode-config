# Model Choices — Why Each Agent Runs What It Runs

Reasoning behind every model assignment in [`oh-my-opencode-slim.json`](config/oh-my-opencode-slim.json).
Snapshot rewritten Oct 11 2026 from a full `/model-refresh`; evidence cited from Artificial
Analysis (Intelligence Index), community field reports, Go-route reliability sweeps, and the
OpenCode Go Estimated Requests table (burn authority — see principles). Transition history
lives in [`MODEL_CHANGELOG.md`](./MODEL_CHANGELOG.md); raw findings in
[`MODEL_EVIDENCE_CACHE.md`](./MODEL_EVIDENCE_CACHE.md).

Paid Contributor SKUs are already sub-cent, so paid entries are kept for route
stability. **Privacy disclosure:** Contributor SKUs (Muse Spark) may train on prompts —
usable on any lane per user decision (Sep 24 2026); every Contributor seat in this file
names its non-training privacy alternative.

## General principles

1. **Burn authority = the Estimated Requests table** at
   <https://opencode.ai/docs/go#estimated-requests>     (req/5h per model). **Never** reason from
   dollar prices or monthly dollar caps — quota economics are request-rate economics on a
   shared pool. Key burn facts (full table: MODEL_EVIDENCE_CACHE.md §Quota Economics,
   as-of 2026-10-11): DeepSeek V4.1 Flash is a flat permanent 26,000 req/5h
   (user-confirmed 2026-09-26, no multiplier); no per-model multipliers exist anywhere in
   the catalog (re-verified 2026-10-11).
2. **Burn floor for hot lanes** — any lane the user actively works through (orchestrator,
   explorer, librarian, designer, fixer) needs ≥~800 req/5h (GLM-5.3-Flash is the reference).
   Sub-floor models are only acceptable where calls are genuinely rare.
3. **Premium-burn models are excluded from every lane including council**.
4. **Quality-first for judgment lanes** (orchestrator/oracle/designer/fixer), **cheap-but-fast
   for volume lanes** (explorer/librarian). **Quality is
   bought efficiently**: on quality per burn (AA score vs req/5h), never raw quality or raw
   cheapness — a modest quality gain at a disproportionate burn multiple is rejected; run the
   cheaper model and iterate instead (only a missing capability like vision or
   catastrophic-failure risk overrides — never the burn floor or premium ban).
5. **Effort ceilings** — `high` default everywhere; `max` is reserved for oracle (when a deep
   call warrants it) and council synthesis; **`xhigh` is allowed on any lane** (policy #7
   amended 2026-10-06; previously only oracle/council could exceed `high`).
6. **Two-deep chains** — primary + one fallback, family-diverse within each lane and across
   orchestrator vs oracle. The same model/family across different agents' chains is fine.
   Current cross-check: orchestrator {DeepSeek, OpenAI} vs oracle {Anthropic, Meta-muse} —
   **no overlap** (since 2026-10-11 the oracle primary is claude-haiku-5-5, clearing the earlier
   gpt-6-luna overlap with the orchestrator *fallback*), so a single family outage degrades
   neither lane.
7. **Provider inclusion + Go-route reliability hard gate** — only `opencode-go/...` (paid Go
   pool) and free zen (`opencode/...`) models are eligible for any chain; all other providers
   are out of scope by default (policy amended Sep 24 2026). Proxy wiring
   (variants, tool calls, thinking blocks) matters more than raw benchmarks. Variant settings are set only
   where the model's variant map has an entry.
8. **Evidence hierarchy** — independent benchmarks (AA Intelligence Index) > community
   role-specific reports > vendor claims. "No data found" is recorded as unverified, never
   guessed.

---

## Orchestrator

**Chain:** `opencode-go/deepseek-v4.1-flash (high)` → `opencode-go/gpt-6-luna (high)`
📖 [What the Orchestrator does](https://github.com/alvinunreal/oh-my-opencode-slim#01-orchestrator-the-embodiment-of-order)

**Why DeepSeek V4.1 Flash leads:** AA Intel **39–40** (#6/113) and **198.6 tok/s (#5/113 —
the fastest model in the config)**. The orchestrator delegates constantly, so decode speed
compounds across every specialist spawn — latency is the lane's dominant tax. Burn: **26,000
req/5h**, 4x+ the hot-lane floor, flat permanent rate ($60/m cap), no multiplier. route-clean
(re-verified 2026-10-11). native image input (official spec). Run at
`high`, not `max` — the flash family's `thinkingLevelMap` maps `max`→null, and orchestration
needs snappy routing, not deep reasoning. Watch: a new opaque intermittent 400
(`{'model':'deepseek-v4.1-flash'}`, #51990, Sep 25–29) — see watch-list.

**Why GPT-6 Luna as #2:** AA Intel 37 (ties its predecessor 5.6-Luna at max), **4,230 req/5h**
at $0.10/$0.50, route-clean, training Not used, family-diverse (OpenAI vs DeepSeek). Known
limits, accepted at fallback depth: AA Coding Agent Index 41 (regression vs 5.6-Luna's 43 on
the current scale), presentation-Elo regressions (GDPval −75, Briefcase −45), verbose at max
(~51k output tokens/task), and a supervised-worker profile with documented unsupervised-drift
failures — it runs under the orchestrator's supervision, which is exactly its validated use
case. Fires only on primary failure.

**Next best alternative:** `opencode-go/mimo-v2.6-pro` (AA 46) is rejected here — time-to-first-answer
39–49s (no effort knob) is disqualifying for the most frequently invoked lane, and it already sits at
council beta. `opencode-go/glm-5.3 (high)` (AA 45) is 220 req/5h premium-burn — banned. The
reference floor model `opencode-go/glm-5.3-flash` remains a catalog revert option only if
its serving latency is ever fixed (see watch-list).

## Oracle

**Chain:** `opencode-go/claude-haiku-5-5 (xhigh)` → `opencode-go/muse-spark-1.3-contributor (xhigh)`
📖 [What the Oracle does](https://github.com/alvinunreal/oh-my-opencode-slim#03-oracle-the-guardian-of-paths)

**Why Claude Haiku 5.5 leads (since Oct 11 2026):** Chosen on **quality-per-burn** (principle
#1): AA Intelligence **41 (xhigh) / 43 (max)** — a full effort ladder (low 29 / medium 34 /
high 38 / **xhigh 41** / max 43) that beats the prior lead `gpt-6-luna` by ~7–9 points at
*every* tier, at slightly **less** burn (**3,850** vs 4,230 req/5h), with image+pdf input and
training Not used (non-contributor). It is **route-clean** (user decision 2026-10-11: the
earlier flag was a third-party client bug, not a Go-route defect). The swap also **clears the
gpt-6-luna cross-lane overlap** with the orchestrator fallback (principle #6) — oracle is now
{Anthropic, Meta-muse}, distinct from orchestrator {DeepSeek, OpenAI}. Run at **`xhigh`**, not
`max`: the lane is advisory/read-only and does not need max's heavy verbosity (haiku's max
burns ~162k output tokens/task, ~3× luna's).

**Tradeoffs accepted:** AA Coding Agent Index **0.414** (#22/24) — mid-pack for agentic coding;
TB4.0 **39.2% vendor / ~33–35% independent**; its independent profile reads **fast
workhorse/subagent, not a deep reasoner** — the same profile class as the model it replaced
(`gpt-6-luna`'s documented supervised-worker drift), so the lane's read-only containment
applies: the oracle is **read-only** (`read_files`) and its output is advice the orchestrator
vets, not unsupervised edits. Only real holdout: haiku is ~4 days old with no Go-route field
history (watch-list). The chain stays strong regardless — its fallback muse-spark-1.3 is AA 45.

**Why Muse Spark 1.3 Contributor as #2:** a genuine **third family** (Meta-muse) — the only
way to keep a systemic gpt or DeepSeek outage from touching both orchestrator and oracle
slots — and route-clean. AA **45 (xhigh, v4.3.2)**, TB2.1 85% (xhigh), 235.2 tok/s,
**45,300 req/5h** (effectively unbilletable), full multimodal (image+video+audio+pdf).
Contributor SKU — **trains on prompts** (accepted — see header).

> **Privacy alternative:** `opencode-go/qwen3.8-flash` (AA 40, image+video, 5,400 req/5h) —
> non-contributor, but route-flagged (see watch-list); not promoted while the flag stands.

**Next best alternatives:** `opencode-go/gpt-6-luna (xhigh)` — the prior lead (Oct 6–11),
route-clean, the same supervised-worker/read-only fit, but AA **34 xhigh / 37 max**: **7 points
below** haiku at equal-or-higher burn, so retained as the flip-back if haiku's field reports
disappoint; now the orchestrator fallback only (1 slot).
`opencode-go/deepseek-v4-pro (max)` has the strongest cached
correctness figures (TB2.1 87.9, SWE-bench Verified 80.6%, LiveCodeBench 93.5%) but is
**disqualified by the Go-route hard gate** — do not re-promote without a verified fix.
`opencode-go/mimo-v2.6-pro` (AA 46) is the recorded revert for a latency-tolerant deep seat,
but carries the overthinking tax the user rejected. `opencode-go/grok-4.7` (AA 46, CAI 56) is
permanently excluded (premium-burn, 169 req/5h).

## Explorer

**Chain:** `opencode-go/deepseek-v4.1-flash (high)` → `opencode-go/mimo-v2.6-flash`
📖 [What the Explorer does](https://github.com/alvinunreal/oh-my-opencode-slim#02-explorer-the-eternal-wanderer)

**Why DeepSeek V4.1 Flash leads:** **198.6 tok/s — the fastest
model in the config** (scouts live and die on latency), image input. Burn: **26,000 req/5h — the flat standing rate** (user-confirmed permanent 2026-09-26,
no multiplier). Deliberately run at `high`, not
`max` — the v4-flash family's `thinkingLevelMap` maps `max`→null.

**Why MiMo-V2.6-Flash as #2 (since Sep 24 2026):** Same $0.14/$0.28 class, **30,100 req/5h
(highest volume in the Go catalog)**, image+video+audio input, privacy Not used/0d, vendor
TB2.1 87.6. Accepted as an unverified trial in the low-risk fallback slot: **no AA page —
zero independent benchmarks**, and its documented failure shapes (nested conditional tool
calls, cyclic grep-retry loops) matter for a lane that runs grep/AST sweeps — monitor in
practice.

> **Upgrade path:** `opencode-go/muse-spark-1.3-contributor (high)` = recorded swap-back if
> mimo-v2.6-flash misbehaves; `opencode-go/glm-5.3-flash (high)` = non-contributor option
> once its serving watch clears.

## Librarian

**Chain:** `opencode-go/muse-spark-1.3-contributor (high)` → `opencode-go/deepseek-v4.1-flash (high)`
📖 [What the Librarian does](https://github.com/alvinunreal/oh-my-opencode-slim#05-librarian-the-weaver-of-knowledge)

**Why Muse Spark 1.3 leads:** Research is network-latency-dominated, so TTFT doesn't
disqualify it — and its strengths matter for docs: AA **45** (xhigh, v4.3.2 scale — see cache; high unmeasured), full multimodal
reading (image+video+audio+pdf) for screenshot/diagram-bearing docs, and **45,300 req/5h**
makes it effectively unbilletable at $0.10/$0.20.
Contributor SKU — **trains on prompts** (accepted — see header).

> **Privacy alternative:** the in-place fallback `opencode-go/deepseek-v4.1-flash (high)` is
> non-contributor (Not used / 0 days) — this chain already has a fully non-contributor rescue
> in place without any swap.

**Why DeepSeek V4.1 Flash as #2 (since Oct 6 2026):** AA Intel **39–40**, native image input,
**26,000 req/5h** flat standing rate, and the fastest decode in the config (198.6 tok/s) —
a route-clean, non-contributor rescue that reads diagrams. Run at `high` (daily ceiling; the
flash family's `thinkingLevelMap` maps `max`→null). Chosen over the prior free-preview
fallback (`longcat-2.5-preview-free`), which carried an unverified ~Oct 9 window expiry, zero
independent benchmarks, and an untested Go route. Watch the intermittent `deepseek-v4.1-flash`
400 (#51990) — see watch-list.

**Next best alternative:** `opencode-go/hy3 (high)` — AA 23, 4,300 req/5h, route-PASS,
text-only; the reliability-first cheap pick if v4.1-flash's 400 flare worsens.

## Designer

**Chain:** `opencode-go/muse-spark-1.3-contributor (xhigh)` → `opencode-go/gpt-5.6-luna (xhigh)`
📖 [What the Designer does](https://github.com/alvinunreal/oh-my-opencode-slim#06-designer-the-guardian-of-aesthetics)

**Why Muse Spark 1.3 leads (since Sep 27 2026):** MiMo-Pro's documented overthinking
(time-to-first-answer 39–49s, no effort knob — see watch-list) disqualifies it in the most
latency-sensitive interactive lane; Muse answers in 4.14s route-measured. Muse brings the
strongest design-adjacent evidence in the roster — Arena
WebDev design splits (Reference-Based Design 1655, Gaming 1724), praised visual output
(pelican-SVG test, HN 2026-09-03), TB2.1 85% at xhigh, 235.2 tok/s decode — full multimodal
reading (image+video+audio+pdf) for screenshot/video-bearing UI work, and **45,300 req/5h**
burn headroom. Contributor SKU — **trains on prompts** (accepted — see header).
Run at **`xhigh`**: it is the effort at which the AA 45 measurement was taken, and policy #7
(amended 2026-10-06) now allows `xhigh` on any lane.

> **Privacy alternative:** `opencode-go/qwen3.8-flash` (AA 40, image+video, 5,400 req/5h) or
> the in-place fallback `opencode-go/gpt-5.6-luna (xhigh)` — both non-contributor.

**Tradeoffs accepted:** AA measures 1.3 only at `xhigh` (45) and `max` (48, may not be
servable); **the `high` variant is unmeasured** — so the lane now runs at the measured
`xhigh` rather than an unmeasured `high`. Known failure shapes at xhigh: overconfident "done"
claims, looped file edits; no exposed thinking traces (harder to audit design reasoning).
Design Arena Code: no data found.

**Why GPT-5.6 Luna as #2:** AA **35 (xhigh, v4.3.2 — verified 2026-09-27)**, image+pdf,
2,050 req/5h — passes the floor at sustained fallback use. Family-diverse (OpenAI vs
Meta-muse). On the current scale its Coding Agent figure is 43 — presentation-relevant
strengths keep it the fallback over GPT-6 Luna (41, presentation-Elo regressions directly
design-relevant). Set to `xhigh` to match the lead's effort.

**Next best alternative:** `opencode-go/mimo-v2.6-pro` — the recorded revert (AA 46, native
omnimodal) if Muse disappoints, but only with eyes open: its overthinking is documented and
has no effort knob on the Go route.

## Fixer

**Chain:** `opencode-go/deepseek-v4.1-flash (high)` → `opencode-go/muse-spark-1.3-contributor (high)`
📖 [What the Fixer does](https://github.com/alvinunreal/oh-my-opencode-slim#07-fixer-the-last-builder)

**Why DeepSeek V4.1 Flash leads:** **198.6 tok/s (fastest in config)**,
26,000 req/5h flat standing rate (permanent, verified 2026-09-26 — well above floor).
Quality + speed + headroom in one pick; route-clean. Run at `high` (daily-lane ceiling).

**Why Muse Spark 1.3 Contributor as #2 (since Oct 11 2026):** the immediate prior fallback
(`qwen3.8-flash (medium)`) was displaced on **quality per burn**: Muse carries AA
**45 (xhigh, v4.3.2)** vs qwen's 40 and **45,300 req/5h** vs 5,400 — roughly 8× the burn
headroom at the same cheap sub-cent $0.10/$0.20 class — and it is **route-clean**, removing
the config's last route-flagged chain position. The Oct-6 snapshot had already named it "the
quality-per-burn standout — a candidate to evaluate next refresh," and this refresh did so.
Full multimodal (image+video+audio+pdf). Contributor SKU — **trains on prompts** (accepted —
see header). Run at **`high`** (the lane's daily ceiling): note Muse's AA 45 is measured at
`xhigh` and its **`high` variant is unmeasured** — accepted at fallback depth, where the slot
rarely fires. Cost of the swap: the lane loses its Alibaba-family slot (both positions now run
families used elsewhere in the config) and adds a sixth Contributor seat.

> **Privacy alternative:** `opencode-go/qwen3.8-flash (medium)` — the displaced
> non-contributor fallback (AA 40, image+video, 5,400 req/5h); retained as the revert, but its
> Go-route flags remain open (see watch-list).

**Next best alternative:** none promoted. `opencode-go/mimo-v2.6-pro` is rejected under
quality-per-burn (AA 46 at 3,250 req/5h ≈ 8× burn for ~15% quality, plus its overthinking
tax). `opencode-go/gpt-6-luna` is rejected for quality lanes (CAI regression).
`opencode-go/deepseek-v4-pro` is STILL FLAGGED (worst in the most tool-call-dense lane). The
reference floor model `opencode-go/glm-5.3-flash` remains a non-contributor option once its
serving watch clears.

---

## Council

**Seats:** alpha=`opencode-go/deepseek-v4-pro` · beta=`opencode-go/mimo-v2.6-pro` · gamma=`opencode-go/muse-spark-1.3-contributor` · synthesis=`opencode-go/gpt-5.6-luna (max)`
📖 [What the Council does](https://github.com/alvinunreal/oh-my-opencode-slim#04-council-the-chorus-of-minds)

Council needs **distinct strong models across distinct families** so a consensus verdict
isn't three opinions from one lineage. Four families (DeepSeek / Xiaomi / Meta-muse /
OpenAI), zero premium. **No change this refresh** — beta keeps MiMo-V2.6-Pro by explicit user
decision (its overthinking is acceptable in a deliberative seat):

- **alpha — deepseek-v4-pro:** the evidence judge. Best verified correctness in the pool
  (figures under Oracle #2). Its tool-call flag is tolerable here — council seats deliberate,
  they don't loop tools.
- **beta — mimo-v2.6-pro:** the strongest non-premium seat in the pool. AA Intelligence 46
  — top open-weight, still nowhere near premium-burn
  (3,250 req/5h), omnimodal so it can judge screenshots too, and family-diverse from alpha
  (Xiaomi vs DeepSeek) and gamma (Meta-muse). Its documented overthinking is tolerable
  here — a deliberative seat prices depth over time-to-answer (user decision 2026-09-27,
  reaffirmed 2026-10-06).
- **gamma — muse-spark-1.3-contributor:** highest measured ceiling in the roster (AA 48 max — but measured only above its assigned effort; xhigh 45, high unmeasured). Contributor SKU —
  **trains on prompts** (accepted — see header). Privacy
  alternative: `opencode-go/qwen3.8-flash` (AA 40, 5,400 req/5h — keeps the council
  zero-premium and family-diverse).
- **synthesis — gpt-5.6-luna (max):** a single judgment+writing call — the one place `max`
  earns its latency; 2,050 req/5h.

Seat models are plain strings (no variants) per plugin schema; synthesis carries the variant.

**Next best alternatives:** `opencode-go/glm-5.2` (880 req/5h, $60/m) clears the floor and is
researched (AA 34 max on v4.3.2) — it trails every current seat (46/45/43-scale), and the GLM
family remains under a serving-latency watch. grok-4.7 (AA 46, CAI 56) is permanently excluded
(premium-burn, 169 req/5h, user-rejected on cost); do not re-propose.

---

## Observer *(optional agent — currently disabled)*

**Chain:** `opencode-go/mimo-v2.6-flash` → `opencode-go/muse-spark-1.3-contributor (high)`
📖 [What the Observer does](https://github.com/alvinunreal/oh-my-opencode-slim#observer-the-silent-witness)

**Disabled** (`disabled_agents: ["observer"]`) per the enablement rule: the observer is
enabled only when the orchestrator chain cannot see images. Orchestrator primary
deepseek-v4.1-flash is natively image-capable (confirmed against official DeepSeek
launch specs 2026-09-27), so the system already reads screenshots.

Dormant chain: mimo-v2.6-flash (evidence under Explorer #2) leads the rescue lineup with
muse-spark-1.3-contributor (high) — full multimodal, 45,300 req/5h — behind it. If the
orchestrator ever reverts to a text-only lead, re-enable the observer
(`disabled_agents: []`) before any screenshot-heavy session.

---

## Watch-list

Things the next `/model-refresh` must **re-check** — conditions that can change on their own:
route flags clearing, serving stabilizing, benchmarks landing, previews rolling, quota facts
moving. **Intrinsic model traits do not belong here** (a model's overthinking or
supervised-worker profile is a fixed property, not a recurring check), and neither do
lane-slot concentration counts.

- **deepseek-v4.1-flash intermittent 400** — opaque `{'model':'deepseek-v4.1-flash'}` 400s
  (#51990, Sep 25–29, dupes #51434/#51477/#51201/#50761). Re-check recurs/resolves — 4 slots
  depend on this model (orchestrator primary, explorer primary, librarian fallback, fixer primary).
- **deepseek-v4-pro Go-route flags** — tool-args-as-text (#1244, open since Apr) plus Go-path
  400s (#42090/#42135/#24566). Re-check for a verified fix; council alpha only.
- **qwen3.8-flash Go-route flags** — max-truncation (#45987, open), long-ctx XML tool-call leak
  (qwen-code#8003), hermes 404 (#100854). No longer in any chain (fixer fallback moved to
  muse-spark-1.3 on 2026-10-11); tracked only as the named non-contributor privacy alternative
  at oracle/designer — re-check if a swap-back is ever contemplated.
- **claude-haiku-5-5 field reports** — NEW (released Oct 7), promoted to oracle primary
  2026-10-11 (route-clean — the sole earlier repro was a third-party client bug, not a Go-route
  defect). Re-check the first real OpenCode Go-route reports and whether its workhorse/subagent
  profile holds in the read-only oracle seat; if not, flip back to gpt-6-luna (xhigh). 3,850 req/5h.
- **GPT-Luna region 403 risk** — family "Upstream request failed" 403 (predecessor #39831).
  Re-check for luna 403 reports; now touches only the orchestrator fallback (1 slot) — the oracle
  primary moved to haiku-5.5 on 2026-10-11, so it no longer risks both lanes at once.
- **GLM-5.3-Flash serving latency** — re-admit as a hot-lane fallback if Z.ai/OpenRouter serving
  stabilizes (check per-refresh OpenRouter latency tables). The demotion is serving, not weights
  (AA 42, vision, 6,320 req/5h).
- **space-bunny benchmarks** — re-check for a first independent AA/eval page before any promotion
  (paid SKU; zero independent benchmarks as-of 2026-10-11; OpenRouter delisted the stealth
  listing Oct 5).
- **Quota-fact drift** — re-fetch `/docs/go` every refresh: multipliers (none as-of 2026-10-11),
  DeepSeek peak/off-peak windows, ZDR window (through Oct 31 2026), new SKUs (Claude Haiku 5.5,
  Step 5 Preview Free this refresh), changed req/5h rows (Kimi K2.6 re-listed in the doc but not
  in the catalog).
