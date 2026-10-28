# Model Evidence Cache
> Updated: 2026-10-25. Maintained by /model-refresh. Manual edits allowed.

## Availability Snapshot (as-of 2026-10-25)

Source: `opencode models` + `opencode api GET /api/model` (re-derived every refresh; never trusted from cache). Only `opencode-go/...` and free zen (`opencode/...`) are in scope (policy #3). Format: `provider/model: variants={…} in=[modalities]`.

**Go pool (paid):**
- `opencode-go/deepseek-v4.1-flash`: variants={low,high,max} in=[text+image] — `max`→null in family thinkingLevelMap
- `opencode-go/deepseek-v4-pro`: variants={high,max} in=[text]
- `opencode-go/claude-haiku-5-5`: variants={none,low,medium,high,xhigh,max} in=[text+image+pdf]
- `opencode-go/muse-spark-1.3-contributor`: variants={minimal,low,medium,high,xhigh} in=[text+image+video+pdf+audio] — **no `max` exposed**
- `opencode-go/muse-spark-1.2-contributor`: variants={minimal,low,medium,high,xhigh} in=[text+image+video+pdf+audio]
- `opencode-go/gpt-6-luna`: variants={none,low,medium,high,xhigh,max} in=[text+image+pdf]
- `opencode-go/gpt-5.6-luna`: variants={none,low,medium,high,xhigh,max} in=[text+image+pdf]
- `opencode-go/mimo-v2.6-flash`: variants={} in=[text+image+audio+video]
- `opencode-go/mimo-v2.6-pro`: variants={} in=[text+image+audio+video]
- `opencode-go/mimo-v2.5`: variants={} in=[text+image+audio+video]
- `opencode-go/mimo-v2.5-pro`: variants={} in=[text]
- `opencode-go/gemini-4-argon`: variants={low,medium,high} in=[text+image+video+audio+pdf]
- `opencode-go/kimi-k3.1-pro`: variants={low,high,max} in=[text+image+video]
- `opencode-go/kimi-k3`: variants={max} in=[text+image+video]
- `opencode-go/kimi-k2.7-code`: variants={} in=[text+image+video]
- `opencode-go/minimax-m3.1-flash`: variants={low,medium,high,xhigh,max} in=[text+image+video]
- `opencode-go/minimax-m3`: variants={none,thinking} in=[text+image+video]
- `opencode-go/glm-5.3`: variants={low,high,max} in=[text]
- `opencode-go/glm-5.3-flash`: variants={low,high,max} in=[text+image+video]
- `opencode-go/glm-5.2`: variants={high,max} in=[text]
- `opencode-go/qwen3.8-max`: variants={none,low,medium,xhigh} in=[text+image+video]
- `opencode-go/qwen3.8-flash`: variants={none,low,medium,xhigh} in=[text+image+video]
- `opencode-go/qwen3.7-plus`: variants={none,high,max} in=[text+image+video]
- `opencode-go/grok-4.7`: variants={low,medium,high,xhigh} in=[text+image+pdf]
- `opencode-go/hy3`: variants={none,low,high} in=[text]
- `opencode-go/hy4-preview`: variants={none,high} in=[text]
- `opencode-go/longcat-2.0`: variants={} in=[text]
- `opencode-go/step-5-preview`: variants={low,medium,high} in=[text+image+video] — now PAID, still labelled preview
- `opencode-go/beam-free`: variants={low,medium,high,xhigh,max} in=[text]
- `opencode-go/iceman-contributor-free`: variants={} in=[text] — identity/list membership unconfirmed

**Zen free pool:**
- `opencode/deepseek-v4.1-flash-free`: variants={low,high,max} in=[text+image]
- `opencode/glm-5.3-flash-free`: variants={low,high,max} in=[text+image+video]
- `opencode/mimo-v2.6-flash-free`: variants={} in=[text+image+audio+video]
- `opencode/nemotron-3.5-super-vl-free`: variants={none,minimal,low,medium,high,xhigh} in=[text+image]
- `opencode/nemotron-3.5-lightning-free`: variants={} in=[text]
- `opencode/nemotron-3-ultra-free`: variants={} in=[text]
- `opencode/flint-40b-free`: variants={low,medium,high,xhigh,max} in=[text]
- `opencode/beam-free`: variants={low,medium,high,xhigh,max} in=[text]
- `opencode/big-pickle`: variants={} in=[text]

**Diff vs 2026-10-11:** NEW gemini-4-argon, kimi-k3.1-pro, minimax-m3.1-flash, beam-free (both pools), iceman-contributor-free, deepseek-v4.1-flash-free, glm-5.3-flash-free, nemotron-3.5-lightning-free, nemotron-3.5-super-vl-free, nemotron-3-ultra-free, flint-40b-free, big-pickle. REMOVED space-bunny(+free), longcat-2.5-preview-free, deepseek-v4-flash, step-5-preview-free (→ step-5-preview), minimax-m2.7, exo-free, ling-2.6-flash-free, muse-spark-1.3-contributor-free, grok-4.6. CHANGED deepseek-v4-pro `{}`→{high,max}.

## Quota Snapshot (as-of 2026-10-25; source: https://opencode.ai/v2/docs/console/go/ — re-fetched every refresh)

**Estimated Requests, Go plan (req/5h · /week · /month):**

| Model | 5h | Week | Month |
|---|---|---|---|
| Muse Spark 1.3 Contributor | 45,300 | 113,300 | 226,600 |
| Muse Spark 1.2 Contributor | 45,300 | 113,300 | 226,600 |
| DeepSeek V4.1 Flash | 34,670 | 86,670 | 173,330 |
| MiMo-V2.6-Flash | 30,100 | 75,200 | 150,400 |
| MiMo-V2.5 | 30,100 | 75,200 | 150,400 |
| LongCat-2.0 | 11,400 | 28,600 | 57,200 |
| GLM-5.3-Flash | 6,320 | 15,790 | 31,580 |
| Qwen3.8 Flash | 5,400 | 13,500 | 27,000 |
| Hy3 | 4,300 | 10,750 | 21,500 |
| Qwen3.7 Plus | 4,300 | 10,800 | 21,600 |
| GPT 6 Luna | 4,230 | 10,560 | 21,130 |
| MiniMax M3.1 Flash | 3,880 | 9,690 | 19,380 |
| Claude Haiku 5.5 | 3,850 | 9,620 | 19,230 |
| MiMo-V2.6-Pro | 3,250 | 8,150 | 16,300 |
| MiMo-V2.5-Pro | 3,250 | 8,150 | 16,300 |
| MiniMax M3 | 3,200 | 8,000 | 16,000 |
| GPT 5.6 Luna | 2,050 | 5,100 | 10,250 |
| Kimi K2.7 Code | 1,350 | 3,380 | 6,750 |
| Hy4 preview | 1,350 | 3,380 | 6,770 |
| DeepSeek V4 Pro | 1,050 | 2,600 | 5,200 |
| GLM-5.2 | 880 | 2,150 | 4,300 |
| Gemini 4 Argon | 440 | 1,080 | 2,120 |
| Step 5 Preview | 430 | 1,070 | 2,130 |
| GLM-5.3 | 220 | 540 | 1,080 |
| Grok 4.7 | 169 | 423 | 845 |
| Qwen3.8 Max | 160 | 400 | 810 |
| Kimi K3.1 Pro | 147 | 368 | 735 |
| Kimi K3 | 110 | 250 | 490 |
| Beam Free | unlimited | unlimited | unlimited |

**Diff vs 2026-10-11:** **DeepSeek V4.1 Flash 26,000 → 34,670** (monthly cap now $80; peak windows 01:00–04:00 + 06:00–10:00 UTC Mon–Fri, off-peak all other hours incl. weekends). **The usage-multiplier column has been replaced by a price-discount column with expiry dates** — Gemini 4 Argon **50% off through 2026-11-30** (list $4.00/$20.00 → $2.00/$10.00; ≡ **2×** in the old multiplier units), Beam Free "free for a limited time" (no date), Muse Spark 1.3/1.2 Contributor "heavily discounted" (no percent or date given); **no other model carries a discount**. New rows: Gemini 4 Argon 440, Kimi K3.1 Pro 147, MiniMax M3.1 Flash 3,880, Beam Free unlimited, LongCat-2.0 11,400, Kimi K2.7 Code 1,350, Hy4 preview 1,350. Everything else flat.

**Pricing/discount facts (volatile):** Gemini 4 Argon $2/$10 (50% off list, discount ends **2026-11-30**), $30/m cap. Step 5 Preview now paid $1.00/$2.70. DeepSeek V4.1 Flash / V4 Pro $0.15/$0.60 off-peak class. Muse Spark Contributor $0.10/$0.20. Claude Haiku 5.5 $0.10/$0.50. MiMo-V2.6-Flash $0.14/$0.28. Kimi K3.1 Pro and Gemini 4 Argon sit in the premium-burn class by policy #4 (≥$2/1M or top-tier limits).

## Stable Quota Facts (90-day TTL; last re-verified 2026-10-25)

- **Muse Spark 1.3/1.2 Contributor:** discounted pricing in exchange for permission to train future Meta models; availability limited to regions permitted by Meta's Geographic Use Policy; requires the workspace opt-in for paid training endpoints. Training-on-prompts accepted on any lane by user decision (Sep 24 2026).
- **DeepSeek:** ZDR agreement renewed monthly — current agreement valid **through 2026-10-31** (renewal to be re-verified next refresh). Prompts not used for training; 0-day retention footnote applies to V4.1 Flash / V4 Pro.
- **OpenAI (GPT-6/5.6 Luna):** abuse-monitoring logs generated for all API usage, retained up to 30 days; training not used.
- **Anthropic (Claude Haiku 5.5):** requests retained 30 days per Anthropic data policies.
- **MiMo (Xiaomi):** not used for training, 0-day retention.
- **Beam Free:** free for a limited time; unavailable in a long list of regions (BY, CN/HK/MO, CU, IR, KP, RU, SY, VE, etc.); 30-day retention.
- **Endpoints:** Go is served through `/zen/go/...` with `/v1/chat/completions`, `/v1/responses`, `/v1/messages` paths; Responses/Anthropic paths are where most tool-schema wiring defects appear.
- **Quota shape:** one combined 5h/week/month pool shared across all models; per-model req/5h figures are burn rates against that shared cap, not separate budgets.
- **Limit framing (v2 page, re-verified 2026-10-25):** limits are expressed as **monthly dollar amounts per model** (Go plan), with 5h = 20% of that monthly limit and week = 50%; token prices are flat between Go and Go Plus, and **Go Plus** carries ~3× the allowances at the same prices (not the subscribed plan). Every response reports its own charge as `opencode.allowance`. Extra Usage can spill to Console balance.

## Per-Model Research

### opencode-go/muse-spark-1.3-contributor
- as-of: 2026-10-25 (benchmarks carried from 2026-09-27 AA reading; no new AA page found — slug returns empty row)
- benchmarks: AA Intelligence **45** (xhigh, v4.3.2); AA `max` row 48 exists but **not servable on Go** (variant map stops at xhigh); TB2.1 85% (xhigh); 235.2 tok/s; full multimodal (image+video+audio+pdf); $0.10/$0.20. Arena WebDev design splits: Reference-Based Design 1655, Gaming 1724.
- community: praised visual/output quality (pelican-SVG HN 2026-09-03); "best cost/intelligence at xhigh" (VentureBeat); no OpenCode-specific field reports this cycle.
- go-route: clean. Only standing risk is contributor-tier availability (region + workspace opt-in) and rate limits.
- failure shapes (intrinsic, not re-checkable): overconfident "done" claims, looped file edits; no exposed thinking traces.

### opencode-go/claude-haiku-5-5
- as-of: 2026-10-25
- benchmarks: AA Intel **43.40 max / 41.25 xhigh / 37.82 high / 34.x medium** (1M ctx, $0.10/$0.50); TB4.0 **32.8% max / 29.3% xhigh / 21.7% high** (independent AA rows; vendor claims ~39.2); AA Coding Agent Index **0.414 (#22/24)** carried from 2026-10-11 (no updated composite retrievable — AA `/agents` is client-rendered); 234.2 tok/s median output; FrontierCode value present but provisional attribution; SWE-bench Verified/Pro + LiveCodeBench: **no data found**.
- community: HN launch thread 2026-10-07 (`https://news.ycombinator.com/item?id=49996437`, 489 comments) — `giancarlostoro`: "designed for small edits", delegating to Haiku "just falls apart"; `dannyw`: "noticeably smarter than GPT-6 Luna… in some coding benchmarks beats Sonnet 5"; pricing complaints about the 100k cheap-tier cutoff. Verdict: fast workhorse/subagent, not a deep reasoner. Design Arena: no data found.
- go-route: **clean** — zero issues in `repo:anomalyco/opencode` for Oct 2026; no thinking-replay/signed-thinking reports; the only prior repro was third-party client `OnesoftQwQ/opencode-go-copilot#137` (fixed v1.12.2).

### opencode-go/deepseek-v4.1-flash
- as-of: 2026-10-25
- benchmarks: NEW AA row `DeepSeek V4.1 Flash 1017 (Max)` Intel **45.10**, TB4.0 **33.8%**, 169.1 tok/s; older base slugs read Intel 39–40 at 198.6 tok/s. **Whether the Go id serves the 1017 snapshot is unverified** (OpenRouter lists `deepseek-v4.1-flash-1017` separately). SWE-bench/LiveCodeBench/FrontierCode: no data found. Kimi vendor blog: DS-1017 CAD-with-tools 81.6 vs K3.1 Pro 92.7 (vendor, 2026-10-23).
- community: HN launch 2026-09-10 (`/item?id=49639090`, 574 comments) + 1017 update story; no new themes on truncation/reasoning-only turns beyond the issue reports.
- go-route: **FLAGGED medium-high.** #51990 closed 2026-10-20 as duplicate (not fixed); dupes #51434/#51477 closed not_planned; **#51201 and #50761 OPEN**; #58744 OPEN 2026-10-24 (`{"model":"deepseek-v4.1-flash-free"}` 400, Windows only, fine on macOS); #55584 repetition loop + `reasoning_text`-must-be-passed-back 400 (closed not_planned); **#57033 streams end mid-generation without `finish_reason`/`[DONE]` on ~6–9% of tool-attached calls at `reasoning_effort: max`** (closed not_planned); #58686 reasoning-only turn with no visible output ~2% (duplicate); #58529 free-id infinite loop (duplicate); #57983 503-on-Responses (duplicate); #58175 prompt-cache claim retracted by reporter (own plugin).

### opencode-go/deepseek-v4-pro
- as-of: 2026-10-25
- benchmarks: cached correctness figures TB2.1 87.9, SWE-bench Verified 80.6%, LiveCodeBench 93.5% (carried, vendor-scale); no AA page.
- community: overthinking/latency complaints at Pro depth (carried).
- go-route: **FLAGGED high on `/v1/responses` + `/v1/messages`**, low-medium on chat-completions. DSV3#1244 (tool calls emitted as plain text) **still OPEN** since 2026-04-24; #42090 (`Responses 400 tools[N].function missing name`, web_search rejected) closed 2026-10-15 **not_planned — not fixed**, re-reported as **#57397 (OPEN, 2026-10-20)**: Responses oa-compat path turns `tool_search`/`web_search` into nameless function tools (plain function tools unaffected); #58027 (2026-10-22) text-instead-of-tool-call recurrence, closed duplicate; #42135/#42091 closed not_planned; #35689 the only genuine completed fix; the PR cited as a DSML fix (#54686) is unrelated.

### opencode-go/gpt-6-luna
- as-of: 2026-10-25
- benchmarks: AA Intel 34 (xhigh) / 37 (max); CAI 41; presentation-Elo regressions (GDPval −75, Briefcase −45); verbose at max (~51k output tokens/task); 4,230 req/5h; text+image+pdf.
- community: supervised-worker profile with unsupervised-drift failures (carried).
- go-route: **FLAGGED low-medium** — #57046 (2026-10-19) `country/region restriction` for `opencode-go/gpt-6-luna`, closed not-planned 2026-10-20; predecessor 403 family (#39831) closed completed and not recurring; #58934 (SchemaError, OPEN 2026-10-25) explicitly states it is client-side encoding and model-independent — not counted.

### opencode-go/gpt-5.6-luna
- as-of: 2026-10-25
- benchmarks: AA Intel 35 (xhigh) / 37 (max), v4.3.2; CAI 43; 2,050 req/5h; text+image+pdf.
- community: same luna family presentation strengths; used as council synthesizer.
- go-route: **degraded/low-medium** (long jobs); same geo-family risk as gpt-6-luna, no specific 403 flood found.

### opencode-go/mimo-v2.6-flash
- as-of: 2026-10-25 (carried from 2026-09-22; not re-researched)
- benchmarks: **no AA page — zero independent benchmarks**; vendor TB2.1 87.6; 30,100 req/5h; text+image+audio+video; $0.14/$0.28.
- community: documented nested-conditional tool calls and cyclic grep-retry loops (one field report of a 30-min grep loop vs deepseek-v4.1 completing the same search).
- go-route: clean (no OpenCode reports).

### opencode-go/mimo-v2.6-pro
- as-of: 2026-10-25 (carried)
- benchmarks: AA Intel 46; 3,250 req/5h; omnimodal; no effort knob.
- community: documented overthinking, time-to-first-answer 39–49s (intrinsic); acceptable only in deliberative seats.
- go-route: clean.

### opencode-go/step-5-preview
- as-of: 2026-10-25
- benchmarks: AA Intel **43.73**, **TB4.0 33.3% (first independent agentic measurement)**, 228.7 tok/s, $1.00/$2.70, 1M ctx; CAI/SWE/LiveCodeBench: no data found.
- community: free-week launch posts (HN 2026-10-08, `/item?id=50007811`); prior cached "fails on large PRs" not re-confirmed; no new hands-on agentic reports.
- go-route: no reports found; now PAID and still labelled *preview* → policy #11 expiry + 430 req/5h sub-floor.

### opencode-go/gemini-4-argon
- as-of: 2026-10-25
- benchmarks: AA `Gemini 4 Argon (High)` Intel **52.56**, TB4.0 **57.1%**, 80.75 tok/s, $2/$10, 1M ctx, text+image+video+audio+pdf; CAI/Design Arena/FrontierCode: no data found.
- community: HN 2026-09-30 (1707 pts) + 2026-10-13 relaunch (1466 pts); no citable coding-specific finding.
- go-route: **FLAGGED medium (availability)** — deterministic 429/`Endpoint is unavailable` outage 2026-10-21/22 (#57851, #57884, #57967, #58071) all closed by 2026-10-25; no thinking-strip/leak or variant defects. **Banned by policy #4 regardless** (premium-burn, 440 req/5h, discount ends 2026-11-30).

### opencode-go/kimi-k3.1-pro
- as-of: 2026-10-25
- benchmarks: AA Intel **55.09**, TB4.0 **67.2%**, 57.53 tok/s, $2/$10, 1M ctx, text+image+video. Vendor: TB4.0 65.7%, CAD-with-tools 92.7 (Claude-Code harness).
- community: HN launch 2026-10-23 (`/item?id=50212065`, 665 comments) — cyber-data stripping debate; no hands-on agentic reliability reports.
- go-route: **no reports found** (untested ≠ clean); nearest neighbour #57372 notes kimi-k3 Responses-path fidelity gaps. **Banned by policy #4** (147 req/5h, $2/$10).

### opencode-go/minimax-m3.1-flash
- as-of: 2026-10-25
- benchmarks: AA Intel **42.71**, TB4.0 **25.8%**, 112.76 tok/s, $0.15/$0.60, 1M ctx, text+image+video; 428B total / 23B active MoE (HN FAQ quote).
- community: HN launch 2026-10-19 (`/item?id=50154540`); no new confirmation/refutation of the M3 Go-proxy thinking/tool failures.
- go-route: **no reports found**; **banned by policy #10** (no MiniMax via Go) until verified fixes exist.

### opencode-go/beam-free (zen twin: opencode/beam-free)
- as-of: 2026-10-25
- benchmarks: AA mapping → "Beam 1.0" (slug `reflection-beam`): Intel **30.33**, TB4.0 **3.0%**, FrontierCode 0.243 (provisional), 363.1 tok/s, $0.40/$1.20; unlimited free, limited time/regions, 30-day retention, text-only.
- community: no field reports.
- go-route: no reports found; identity mapping medium-confidence.

### opencode-go/iceman-contributor-free
- as-of: 2026-10-25
- benchmarks: AA mapping uncertain → "Muse Spark Open (Xhigh)": Intel **38.87**, TB4.0 **8.1%**, 150.9 tok/s, $1.25/$4.25 (map is a guess; id absent from the published Go model list).
- community/go-route: no reports; **list membership unconfirmed** — treat as unusable until confirmed.

### Other catalog models — carried forward, not re-researched this cycle (out of every chain)
- `opencode-go/glm-5.3-flash` (as-of 2026-10-11): AA 42, 6,320 req/5h, text+image+video — hot-lane serving-latency watch stands.
- `opencode-go/glm-5.3` (as-of 2026-10-11): AA 45 max, 220 req/5h → premium-burn class, excluded.
- `opencode-go/glm-5.2` (as-of 2026-10-11): AA 34 max, 880 req/5h.
- `opencode-go/qwen3.8-flash` (as-of 2026-10-11): AA 40, 5,400 req/5h, image+video; route flags open (#45987 max-truncation, qwen-code#8003 long-ctx XML tool-call leak, #100854 hermes 404).
- `opencode-go/qwen3.8-max` (as-of 2026-10-11): AA 43, 160 req/5h → premium-burn banned.
- `opencode-go/qwen3.7-plus` (as-of 2026-10-11): 4,300 req/5h, no independent benchmark of note.
- `opencode-go/grok-4.7` (as-of 2026-10-11): AA 46, CAI 56, 169 req/5h → permanently excluded (premium-burn, user-rejected).
- `opencode-go/kimi-k3` (as-of 2026-10-11): AA 44 max, 110 req/5h → premium-burn banned.
- `opencode-go/kimi-k2.7-code` (as-of 2026-10-11): 1,350 req/5h, no AA page.
- `opencode-go/minimax-m3` (as-of 2026-10-11): banned by policy #10.
- `opencode-go/mimo-v2.5` / `-pro` / `mimo-v2.5-pro` (as-of 2026-10-11): superseded by v2.6; no chain use.
- `opencode-go/muse-spark-1.2-contributor` (as-of 2026-10-11): superseded by 1.3; same 45,300 req/5h.
- `opencode-go/hy3` (as-of 2026-10-11): AA 23, 4,300 req/5h, text-only, route-PASS — reliability-first cheap option.
- `opencode-go/hy4-preview` (as-of 2026-10-11): text-only, preview — expiry watch.
- `opencode-go/longcat-2.0` (as-of 2026-10-11): 11,400 req/5h, text-only, no independent benchmark.
- zen free ids (`glm-5.3-flash-free`, `deepseek-v4.1-flash-free`, `mimo-v2.6-flash-free`, nemotron family, `flint-40b-free`, `big-pickle`): no per-model research beyond availability; the paid `opencode-go` twins are preferred per policy #2 (route stability at sub-cent cost).

## Doc Snapshots
- README Pantheon: fetched 2026-10-11 — **not re-fetched 2026-10-25** (inside the 90-day window, no role guidance in dispute). Role anchors used: orchestrator/delegation, oracle read-only judgment, designer aesthetics, fixer last-builder, librarian docs+pixels, explorer fast scout, council chorus.
- council.md: fetched 2026-10-25 — **changed vs prior understanding**: councillors DO have read-only codebase tools (read/glob/grep), permission-enforced, and seat sessions receive only their dispatch prompt. Key doctrine: "seat quality outranks seat count"; a seat that fails over to a weak model dilutes the council; measured finding-level overlap 52–62% same-model vs 37–46% cross-model; no intra-council critique stage (ADR-002). Seat `model` accepts a string or an ordered fallback chain; `variant` is a separate per-councillor field; synthesis model lives under `presets.<name>.council.model`.
