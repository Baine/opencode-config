# Model Choices — Why Each Agent Runs What It Runs

Reasoning behind every model assignment in [`oh-my-opencode-slim.json`](config/oh-my-opencode-slim.json). Snapshot rewritten **Oct 25 2026** from a full `/model-refresh`; evidence cited from Artificial Analysis (Intelligence Index, Terminal-Bench 4.0), OpenCode GitHub route reports, community field reports, and the OpenCode Go Estimated Requests table (burn authority — see principles). Transition history lives in [`MODEL_CHANGELOG.md`](./MODEL_CHANGELOG.md); raw findings in [`MODEL_EVIDENCE_CACHE.md`](./MODEL_EVIDENCE_CACHE.md).

Paid Contributor SKUs are already sub-cent, so paid entries are kept for route stability. **Privacy disclosure:** Contributor SKUs (Muse Spark) may train on prompts — usable on any lane per user decision (Sep 24 2026); every Contributor seat in this file names its non-training privacy alternative.

## General principles

1. **Burn authority = the Estimated Requests table** at <https://opencode.ai/v2/docs/console/go/#estimated-requests> (req/5h per model). **Never** reason from dollar prices or monthly dollar caps — quota economics are request-rate economics on a shared pool. Key burn facts (full table: MODEL_EVIDENCE_CACHE.md §Quota Economics, as-of 2026-10-25): DeepSeek V4.1 Flash is **34,670 req/5h** (re-rated up from 26,000 this refresh), Muse Spark 1.3 Contributor 45,300, MiMo-V2.6-Flash 30,100, GLM-5.3-Flash 6,320 (floor reference), GPT-6 Luna 4,230, Hy3 4,300, Claude Haiku 5.5 3,850, MiMo-V2.6-Pro 3,250, GPT-5.6 Luna 2,050, DeepSeek V4 Pro 1,050, Step 5 Preview 430, Gemini 4 Argon 440, Grok 4.7 169, Kimi K3.1 Pro 147, Qwen3.8 Max 160. **Burn modifiers ship as a price discount, not a multiplier** (re-verified on the v2 quota page 2026-10-25): Gemini 4 Argon is 50% off through 2026-11-30 (≡ 2× in the old multiplier units; list $4.00/$20.00 → $2.00/$10.00), Beam Free is free for a limited time, Muse Spark contributors are "heavily discounted" with no stated percent — nothing else is discounted, and a discount lapses back to 1× on its expiry date.
2. **Burn floor for hot lanes** — any lane the user actively works through (orchestrator, explorer, librarian, designer, fixer) needs ≥~800 req/5h at every chain position. Sub-floor models are only acceptable where calls are genuinely rare.
3. **Premium-burn models are excluded from every lane including council.** As of this refresh that bans Gemini 4 Argon ($2/$10, 440 req/5h), Kimi K3.1 Pro ($2/$10, 147 req/5h), Kimi K3, Qwen3.8 Max, Grok 4.7 — all currently format-strong and all unusable here.
4. **Quality-first for judgment lanes** (orchestrator/oracle/designer/fixer), **cheap-but-fast for volume lanes** (explorer/librarian). **Quality is bought efficiently**: on quality per burn (AA score vs req/5h), never raw quality or raw cheapness — a modest quality gain at a disproportionate burn multiple is rejected; run the cheaper model and iterate instead (only a missing capability like vision or catastrophic-failure risk overrides — never the burn floor or premium ban).
5. **Effort ceilings** — `high` default everywhere; `max` is reserved for oracle (when a deep call warrants it) and council; **`xhigh` is allowed on any lane** (policy #7 amended 2026-10-06). Variants are set **only** where the model's live variant map has the entry — `muse-spark-1.3-contributor` tops out at `xhigh` on the Go route (no `max` is exposed), and the `deepseek-v4.1-flash` family maps `max`→null, so both run at or below `xhigh`.
6. **Two-deep chains** — primary + one fallback, family-diverse within each lane and across orchestrator vs oracle. The same model/family across different agents' chains is fine. Current cross-check: orchestrator {DeepSeek, OpenAI} vs oracle {Meta-muse, Anthropic} — **no overlap**.
7. **Provider inclusion + Go-route reliability hard gate** — only `opencode-go/...` (paid Go pool) and free zen (`opencode/...`) models are eligible for any chain; all other providers are out of scope by default (policy amended Sep 24 2026). Proxy wiring (variants, tool calls, thinking blocks) matters more than raw benchmarks, and it is re-verified per model every refresh.
8. **Evidence hierarchy** — independent benchmarks (AA Intelligence Index) > community role-specific reports > vendor claims. "No data found" is recorded as unverified, never guessed.

---

## Orchestrator

**Chain:** `opencode-go/deepseek-v4.1-flash (high)` → `opencode-go/gpt-6-luna (high)`
📖 [What the Orchestrator does](https://github.com/alvinunreal/oh-my-opencode-slim#01-orchestrator-the-embodiment-of-order)

**Why DeepSeek V4.1 Flash leads:** the orchestrator delegates constantly, so decode speed compounds across every specialist spawn — latency is the lane's dominant tax, and this model is the fastest in the chain set (~198.6 tok/s on the base snapshot, 169.1 tok/s on the newer dated row). Burn is **34,670 req/5h** — re-rated upward this refresh from 26,000, i.e. the best headroom of any quality-tier model — with peak windows 01:00–04:00 and 06:00–10:00 UTC weekdays, off-peak otherwise. Native image input (official spec), so the lane can read screenshots itself. Run at `high`, not `max` — the flash family's `thinkingLevelMap` maps `max`→null and routing needs snappy turns, not depth.

**Route caveat (accepted, flagged):** the opaque `{'model':'deepseek-v4.1-flash'}` 400 remains **unresolved**. #51990 was closed 2026-10-20 *as a duplicate, not as fixed*; the same signature reproduced on 2026-10-24 (#58744, on the `-free` id, Windows-only, fine on macOS), and two duplicate reports stay open (#51201, #50761). New Oct 2026 turn-level reports add the same family: silent stream truncation without `finish_reason`/`[DONE]` on ~6–9% of tool-attached calls at `reasoning_effort: max` (#57033, closed not-planned) and reasoning-only turns with no visible output at ~2% (#58686, closed as duplicate). Mitigations in place: this lane runs `high` (never `max`), the orchestrator is supervised and resumable, and its fallback is route-clean-ish. Re-checked every refresh — see watch-list.

**Why GPT-6 Luna as #2:** AA Intel **34 (xhigh) / 37 (max)** at **4,230 req/5h**, text+image+pdf input, family-diverse (OpenAI vs DeepSeek), training not used. Known limits, accepted at fallback depth: AA Coding Agent Index 41 (regression vs 5.6-Luna's 43 on the current scale), presentation-Elo regressions (GDPval −75, Briefcase −45), verbose at max (~51k output tokens/task), and a supervised-worker profile with documented unsupervised-drift failures — it runs under supervision, which is exactly its validated use case. Go-route status: **low-medium** (a 2026-10-19 country/region-restriction report #57046, China-located, closed not-planned; no 403 flood). Fires only on primary failure.

**Next best alternative:** `opencode-go/muse-spark-1.3-contributor (xhigh)` is the best quality-per-burn model in the pool (AA 45, 45,300 req/5h, 235 tok/s) and was considered for this lane again this refresh; it loses on **family separation** (it would put a Meta model in both orchestrator and oracle) and because it already carries six chain slots — promoting it to a seventh concentrates primaries in one contributor family. `opencode-go/claude-haiku-5-5 (xhigh)` was considered and rejected on burn (3,850 vs 34,670 req/5h for the most frequently invoked lane) plus its documented small-edit workhorse profile. `opencode-go/mimo-v2.6-pro` (AA 46) fails on time-to-first-answer (39–49s, no effort knob). `opencode-go/glm-5.3` (AA 45) is 220 req/5h premium-burn — banned.

## Oracle

**Chain:** `opencode-go/muse-spark-1.3-contributor (xhigh)` → `opencode-go/claude-haiku-5-5 (xhigh)`
📖 [What the Oracle does](https://github.com/alvinunreal/oh-my-opencode-slim#03-oracle-the-guardian-of-paths)

**Why Muse Spark 1.3 Contributor leads:** the lane exists for hard judgment calls on artifacts the orchestrator hands over, and this is the strongest policy-eligible model that fits quota on both axes at once — AA Intelligence **45 (xhigh, v4.3.2)** at **45,300 req/5h**, with full multimodal input (image+video+audio+pdf) so it can actually *read* the diagram, screenshot, or PDF it is asked to judge. 235.2 tok/s means the escalation returns fast. Contributor SKU — **trains on prompts** (accepted, see header), region-limited by Meta's geographic policy.

**Why Claude Haiku 5.5 as #2:** the route-clean, non-training rescue at **3,850 req/5h** — AA **41.25 (xhigh) / 43.40 (max)**, TB4.0 29.3% xhigh, 234.2 tok/s, image+pdf input, training not used. It is the strongest non-contributor option in the pool and the named privacy alternative for this lane. Its profile is a fast workhorse/subagent rather than a deep reasoner (AA Coding Agent Index 0.414, #22/24; HN launch-thread reports describe it as "designed for small edits" and note that delegating genuinely deep work to it "falls apart"), which is precisely why it sits at fallback depth for an advisory seat rather than leading. **Route-clean** — zero new Go-route reports through 2026-10-25.

**Alternatives considered:** `opencode-go/gpt-6-luna (xhigh)` — AA 34 xhigh / 37 max at 4,230 req/5h, route-clean-ish, same supervised-worker/read-only fit, but **7–11 points below the fallback** at comparable-or-higher burn; retained as a flip-back only if both current positions disappoint. `opencode-go/gpt-5.6-luna (xhigh)` — AA 35 at 2,050 req/5h, degraded route, and the synthesizer already covers that family. `opencode-go/step-5-preview` — AA 43.73 with the first genuine independent agentic figure (TB4.0 33.3%), 228.7 tok/s, but **430 req/5h** is a 105× burn step for −1.3 points against the incumbent, and it is still labelled *preview* (policy #11). `opencode-go/deepseek-v4.1-flash` — AA 45.10 on the new 1017 row, but moves a medium-high tool-path flag into a lane that reads files and returns advice. `opencode-go/deepseek-v4-pro` — strongest cached correctness figures (TB2.1 87.9, SWE-bench Verified 80.6%, LiveCodeBench 93.5%) but **disqualified by the Go-route hard gate**: `/v1/responses` and `/v1/messages` are flagged high, DSV3#1244 (tool calls emitted as text) is still open upstream, and #58027 re-reported the same signature on 2026-10-22. Do not re-promote without a verified fix. `opencode-go/gemini-4-argon` (AA 52.56) and `opencode-go/kimi-k3.1-pro` (AA 55.09) are the strongest models in the pool and both are **premium-burn banned**.

> **Privacy alternative:** `opencode-go/claude-haiku-5-5 (xhigh)` — in place as the fallback, so this chain already has a fully non-contributor rescue without any swap.

## Explorer

**Chain:** `opencode-go/deepseek-v4.1-flash (high)` → `opencode-go/mimo-v2.6-flash`
📖 [What the Explorer does](https://github.com/alvinunreal/oh-my-opencode-slim#02-explorer-the-eternal-wanderer)

**Why DeepSeek V4.1 Flash leads:** scouts live and die on latency — this is the fastest model in the config, with image input so it can read UI screenshots while mapping a codebase. Burn **34,670 req/5h** (re-rated up this refresh) puts it far above the hot-lane floor. Deliberately run at `high`, not `max` — the v4-flash family's `thinkingLevelMap` maps `max`→null, and its only variants are low/high/max. Same accepted route caveat as the orchestrator (opaque 400, turn-level truncation reports), which matters less here because explorer output is grist the orchestrator re-reads rather than an artifact of record.

**Why MiMo-V2.6-Flash as #2:** same cheap $0.14/$0.28 class, **30,100 req/5h**, image+video+audio input, privacy not used/0 days, vendor TB2.1 87.6. Accepted as an unverified trial in the low-risk fallback slot: **no AA page — zero independent benchmarks**, and its documented failure shapes (nested conditional tool calls, cyclic grep-retry loops) matter for a lane that runs grep/AST sweeps. No new direct evidence either way this refresh; it stays because it is route-clean and has 30k req/5h of headroom.

> **Upgrade path:** `opencode-go/muse-spark-1.3-contributor (xhigh)` = recorded swap-back if mimo-v2.6-flash misbehaves; `opencode-go/glm-5.3-flash (high)` = non-contributor option once its serving watch clears.

## Librarian

**Chain:** `opencode-go/muse-spark-1.3-contributor (xhigh)` → `opencode-go/deepseek-v4.1-flash (high)`
📖 [What the Librarian does](https://github.com/alvinunreal/oh-my-opencode-slim#05-librarian-the-weaver-of-knowledge)

**Why Muse Spark 1.3 leads:** research is network-latency-dominated, so TTFT doesn't disqualify it — and its strengths matter for docs: AA **45** at `xhigh`, full multimodal reading (image+video+audio+pdf) for screenshot- and diagram-bearing documentation, and **45,300 req/5h** making it effectively unbilletable at $0.10/$0.20. It runs at **`xhigh`** because that is the only effort AA has measured for this model — `high` is unmeasured and `max` is **not exposed on the Go route** (variant map tops out at `xhigh`), so the measured rung is also the sensible one. Contributor SKU — **trains on prompts** (accepted, see header).

> **Privacy alternative:** the in-place fallback `opencode-go/deepseek-v4.1-flash (high)` is non-contributor (not used / 0 days) — this chain has a fully non-contributor rescue already in place without any swap.

**Why DeepSeek V4.1 Flash as #2:** AA Intel 39–40 on the base snapshot (45.10 on the newer 1017 row), native image input, **34,670 req/5h**, and the fastest decode in the config — a route-clean, non-contributor rescue that reads diagrams. Run at `high` (daily ceiling; `max`→null in this family).

**Next best alternative:** `opencode-go/hy3 (high)` — AA 23, 4,300 req/5h, route-PASS, text-only; the reliability-first cheap pick if the v4.1-flash 400 flare worsens.

## Designer

**Chain:** `opencode-go/muse-spark-1.3-contributor (xhigh)` → `opencode-go/gpt-5.6-luna (xhigh)`
📖 [What the Designer does](https://github.com/alvinunreal/oh-my-opencode-slim#06-designer-the-guardian-of-aesthetics)

**Why Muse Spark 1.3 leads:** the strongest design-adjacent evidence in the roster — Arena WebDev design splits (Reference-Based Design 1655, Gaming 1724), praised visual output (pelican-SVG test, HN 2026-09-03), AA **45** at xhigh, TB2.1 85%, 235.2 tok/s decode, full multimodal reading for screenshot/video-bearing UI work, and **45,300 req/5h** burn headroom (far above the hot-lane floor). MiMo-Pro's documented overthinking (time-to-first-answer 39–49s, no effort knob) disqualifies it in the most latency-sensitive interactive lane; Muse answers in ~4.1s route-measured. Contributor SKU — **trains on prompts** (accepted, see header). Run at **`xhigh`**: the only measured rung, and `max` is not servable on this route.

**Tradeoffs accepted:** no separate AA Design Arena Code composite exists for it ("no data found"); known failure shapes at xhigh are overconfident "done" claims, looped file edits, and no exposed thinking traces (harder to audit design reasoning). Its share of the config (six chain slots, three of them primaries) is the concentration cost of using the best contributor-tier model broadly.

> **Privacy alternative:** `opencode-go/gpt-5.6-luna (xhigh)` — in place as the fallback, non-contributor; `opencode-go/qwen3.8-flash` (AA 40, 5,400 req/5h) is route-flagged and not promoted while that stands.

**Why GPT-5.6 Luna as #2:** AA **35 (xhigh, v4.3.2)**, image+pdf, **2,050 req/5h** — passes the floor at sustained fallback use, family-diverse (OpenAI vs Meta-muse). On the current scale its Coding Agent figure is 43; presentation-relevant strengths keep it ahead of GPT-6 Luna (41, plus presentation-Elo regressions directly design-relevant) for this lane. Set to `xhigh` to match the lead's effort.

**Next best alternative:** `opencode-go/mimo-v2.6-pro` — the recorded revert (AA 46, native omnimodal) if Muse disappoints, but only with eyes open: its overthinking is documented and has no effort knob on the Go route.

## Fixer

**Chain:** `opencode-go/deepseek-v4.1-flash (high)` → `opencode-go/muse-spark-1.3-contributor (high)`
📖 [What the Fixer does](https://github.com/alvinunreal/oh-my-opencode-slim#07-fixer-the-last-builder)

**Why DeepSeek V4.1 Flash leads:** quality + speed + headroom in one pick — the fastest decode in the config, **34,670 req/5h** (re-rated up this refresh, well above the 800 hot-lane floor). Run at `high` (daily-lane ceiling; `max`→null in this family). This is the lane the medium-high route flag touches most consequentially — the truncation reports (#57033, ~6–9% of tool-attached calls at `reasoning_effort: max`) are exactly the failure mode that hurts an edit loop, which is why this chain runs `high` and why the fallback is a full-quality model rather than another flash variant.

**Why Muse Spark 1.3 Contributor as #2:** on **quality per burn** it is the standout fallback — AA **45 (xhigh, v4.3.2)** at **45,300 req/5h**, i.e. ~8× the headroom of the other candidate at the same cheap sub-cent class, route-clean, full multimodal (image+video+audio+pdf). Contributor SKU — **trains on prompts** (accepted, see header). Run at **`high`** at fallback depth, where the slot rarely fires; its measured rung is `xhigh` (no `max` on the Go route), so a promotion would also raise effort.

> **Privacy alternative:** `opencode-go/qwen3.8-flash (medium)` — the displaced non-contributor fallback (AA 40, image+video, 5,400 req/5h); retained as the revert, but its Go-route flags remain open (see watch-list).

**Next best alternative:** none promoted. `opencode-go/mimo-v2.6-pro` is rejected under quality-per-burn (AA 46 at 3,250 req/5h for ~1 point over Muse at a sixteenth of the headroom, plus its overthinking tax). `opencode-go/gpt-6-luna` is rejected for quality lanes (CAI regression). `opencode-go/step-5-preview` (43.73) is sub-floor at 430 req/5h. `opencode-go/deepseek-v4-pro` remains route-flagged and is the worst possible pick in the most tool-call-dense lane.

---

## Council

**Seats:** alpha=`opencode-go/claude-haiku-5-5 (xhigh)` · beta=`opencode-go/mimo-v2.6-pro` · gamma=`opencode-go/muse-spark-1.3-contributor (xhigh)` · synthesis=`opencode-go/gpt-5.6-luna (max)`
📖 [What the Council does](https://github.com/alvinunreal/oh-my-opencode-slim#04-council-the-chorus-of-minds)

Council needs **distinct strong models across distinct families** so a consensus verdict isn't three opinions from one lineage. Three seat families here — Anthropic / Xiaomi / Meta-muse — plus an OpenAI synthesizer, and **zero premium-burn**. Per `docs/council.md`, seat quality outranks seat count: the spread between the strongest and weakest reviewer model exceeds the whole spread across same-model setups, so no seat is allowed to be a weak model or a broken route.

- **alpha — claude-haiku-5-5 (xhigh):** the route-clean reviewer. Seat sessions get **read-only codebase tools** (read/glob/grep, permission-enforced), so a tool-path defect in a seat is live rather than theoretical — and this seat is now the only one in the council with a clean Go-route record. AA 41.25 xhigh / 43.40 max, TB4.0 29.3%, 3,850 req/5h, image+pdf. It replaces DeepSeek V4 Pro, whose `/v1/responses`+`/v1/messages` paths are flagged high (#42090 closed unfixed, re-reported as #57397; DSV3#1244 tool-calls-as-text still open upstream; #58027 recurred 2026-10-22 at the dominant signature). Trade accepted: Pro holds the better pure-correctness reputation, so it is the named revert once a verified Go-route fix lands.
- **beta — mimo-v2.6-pro:** the strongest non-premium seat in the pool. AA Intelligence 46 — top open-weight, nowhere near premium burn (**3,250 req/5h**), omnimodal so it can judge screenshots too, family-diverse from alpha (Xiaomi vs Anthropic) and gamma (Meta-muse). Its documented overthinking is tolerable here — a deliberative seat prices depth over time-to-answer (user decision 2026-09-27, reaffirmed 2026-10-06 and 2026-10-25).
- **gamma — muse-spark-1.3-contributor (xhigh):** highest measured ceiling in the roster at the rung the Go route actually serves (AA 45 xhigh; the AA `max` row at 48 is **not servable here** — the live variant map stops at `xhigh`). Contributor SKU — **trains on prompts** (accepted, see header). Privacy alternative: `opencode-go/qwen3.8-flash` (AA 40, 5,400 req/5h — keeps the council zero-premium and family-diverse).
- **synthesis — gpt-5.6-luna (max):** a single judgment+writing call — the one place `max` earns its latency; **2,050 req/5h**, non-premium (policy #13). AA 35 xhigh / 37 max.

Seat `model` values stay plain strings per plugin schema; `variant` is a separate field (alpha and gamma carry it), and synthesis carries its variant inside the chain entry.

**Next best alternatives:** `opencode-go/deepseek-v4.1-flash` (45.10 on the 1017 row, 34,670 req/5h) would be the cheapest strong seat but carries the medium-high route flag into the one place a silent tool failure corrupts a vote. `opencode-go/gpt-6-luna (xhigh)` (AA 34) is the fallback judge if haiku disappoints. `opencode-go/glm-5.2` (880 req/5h) clears the floor but trails every seat on quality and sits in a family under a serving-latency watch. `opencode-go/gemini-4-argon` (52.56) and `opencode-go/kimi-k3.1-pro` (55.09) are premium-burn banned; `opencode-go/minimax-m3.1-flash` (42.71, 3,880 req/5h) is banned by policy #10 until verified Go-route fixes exist.

---

## Observer *(optional agent — currently disabled)*

**Chain:** `opencode-go/mimo-v2.6-flash` → `opencode-go/muse-spark-1.3-contributor (high)`
📖 [What the Observer does](https://github.com/alvinunreal/oh-my-opencode-slim#observer-the-silent-witness)

**Disabled** (`disabled_agents: ["observer"]`) per the enablement rule: the observer is enabled only when the orchestrator chain cannot see images. Orchestrator primary deepseek-v4.1-flash is natively image-capable (confirmed against the live API: `input=[text+image]`), so the system already reads screenshots.

Dormant chain: mimo-v2.6-flash (evidence under Explorer #2) leads the rescue lineup with muse-spark-1.3-contributor (high) — full multimodal, 45,300 req/5h — behind it. If the orchestrator ever reverts to a text-only lead, re-enable the observer (`disabled_agents: []`) before any screenshot-heavy session.

---

## Watch-list

Things the next `/model-refresh` must **re-check** — conditions that can change on their own: route flags clearing, serving stabilizing, benchmarks landing, previews rolling, quota facts moving. **Intrinsic model traits do not belong here** (a model's overthinking or small-edit workhorse profile is a fixed property), and neither do lane-slot concentration counts.

- **Quota-fact drift** — re-fetch `/v2/docs/console/go/` every refresh: burn modifiers now appear as **discounts with expiry dates** rather than multipliers (Gemini 4 Argon 50% off through 2026-11-30 ≡ 2×; Beam Free "limited time"; Muse Spark "heavily discounted"), and new SKUs (Kimi K3.1 Pro, MiniMax M3.1 Flash, Beam Free; `-free` previews renamed/delisted) can move rows between refreshes.
- **Unproven newcomers** — `opencode-go/beam-free` (AA "Beam 1.0": Intel 30.33, TB4.0 3.0%) has zero OpenCode route reports; no promotions until field evidence exists. `kimi-k3.1-pro` and `minimax-m3.1-flash` also have zero route reports (untested ≠ clean); MiniMax additionally stays banned by policy #10 until verified Go-route fixes.
