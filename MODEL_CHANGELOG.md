# Model Changelog

Append-only history of model assignment changes in
[`oh-my-opencode-slim.json`](config/oh-my-opencode-slim.json), newest entry first.
`/model-refresh` inserts one entry per run directly below this header (including
"no changes" entries); `MODEL_CHOICES.md` holds the current rationale snapshot.
Never rewrite or delete prior entries.

## 2026-10-25 — periodic refresh (oracle re-ranked on quality-per-burn; council alpha off the flagged route; effort alignment)

Scope: full `/model-refresh`. Availability re-derived (`opencode models` + `opencode api GET /api/model`) and quota re-fetched (`/docs/go`); benchmarks, community reports and Go-route reliability re-researched for the models touched by this cycle (haiku, v4.1-flash, v4-pro, both lunas, step-5, gemini-4-argon, kimi-k3.1-pro, minimax-m3.1-flash, the two new free ids). Four lanes changed; every other chain kept.

**Changes:**
- **Oracle (position swap):** `opencode-go/claude-haiku-5-5 (xhigh)` → `opencode-go/muse-spark-1.3-contributor (xhigh)` as **primary**, and muse → haiku as **fallback**. Why: the chain had its stronger model in the second position on every axis — AA 45 vs 41.25 at xhigh, and **45,300 vs 3,850 req/5h** — while policy #1 puts the best model that fits quota in first position; fresh community evidence sharpened this (haiku's HN launch thread: "designed for small edits", delegating deep work to it "just falls apart"), which is the oracle's exact job description. Gained: +3.75 AA points and 12× the burn headroom in the first position, plus pdf/audio input for artifact review. Lost: the non-contributor model is no longer first (muse trains on prompts — accepted any-lane since Sep 24 2026), and muse rises to 7 chain slots / 3 of them primaries, so one Meta-contributor outage now degrades three primaries at once (each has a fallback).
- **Council alpha:** `opencode-go/deepseek-v4-pro` → `opencode-go/claude-haiku-5-5 (xhigh)`. Why: `docs/council.md` (re-fetched) confirms councillors run **read-only codebase tools**, so a tool-path defect corrupts a vote rather than being theoretical — and v4-pro's flags are unchanged-to-worse: #42090 closed **unfixed** (re-reported as #57397, open), DSV3#1244 (tool calls emitted as text) still open upstream, #58027 recurred 2026-10-22. Haiku is route-clean with zero reports since release. Gained: the last flagged seat in the council is gone and the seat gains burn headroom (3,850 vs 1,050 req/5h, ~3.7×). Lost: v4-pro's stronger pure-correctness reputation (TB2.1 87.9, SWE-bench Verified 80.6%) — it stays the named revert once a verified Go-route fix lands.
- **Librarian primary variant:** `opencode-go/muse-spark-1.3-contributor (high)` → `(xhigh)`. Why: `high` is the one rung AA has never measured for this model, while `xhigh` is where the 45 came from; at 45,300 req/5h the extra thinking tokens are economically irrelevant and the lane is network-latency-dominated anyway. Gained: the lane's primary now runs its measured, best-served effort. Lost: nothing material (slightly longer turns).
- **Council gamma variant:** `opencode-go/muse-spark-1.3-contributor` (harness default) → `(xhigh)`. Why: same unmeasured-default problem; `xhigh` is the highest variant the Go route actually exposes for this id. Note: `max` (AA 48) was attempted first and **reverted before write-back** — the live variant map is `{minimal,low,medium,high,xhigh}`, so a `max` setting would have been silently ignored.

**No change (kept):** orchestrator (`deepseek-v4.1-flash (high) → gpt-6-luna (high)`), explorer (`deepseek-v4.1-flash (high) → mimo-v2.6-flash`), designer (`muse-spark-1.3-contributor (xhigh) → gpt-5.6-luna (xhigh)`), fixer (`deepseek-v4.1-flash (high) → muse-spark-1.3-contributor (high)`), librarian fallback (`deepseek-v4.1-flash (high)`), council beta (`mimo-v2.6-pro`), council synthesis (`gpt-5.6-luna (max)`), observer (dormant — orchestrator primary still image-capable).

**Availability diff vs 2026-10-11:** NEW `opencode-go/gemini-4-argon` (low/medium/high, text+image+video+audio+pdf — renamed from a Contributor id), NEW `opencode-go/kimi-k3.1-pro` (low/high/max, text+image+video), NEW `opencode-go/minimax-m3.1-flash` (low→max, text+image+video), NEW `opencode-go/beam-free` + zen `opencode/beam-free` (low→max, text-only), NEW zen `opencode/deepseek-v4.1-flash-free` (low/high/max, text+image), zen `opencode/glm-5.3-flash-free`, `opencode/nemotron-3.5-lightning-free`, `opencode/nemotron-3.5-super-vl-free`, `opencode/nemotron-3-ultra-free`, `opencode/flint-40b-free`, `opencode/big-pickle`, `opencode/mimo-v2.6-flash-free`, NEW `opencode-go/iceman-contributor-free` (no variants, text — list membership/identity unconfirmed). REMOVED: `space-bunny`(+`-free`), `longcat-2.5-preview-free`, `deepseek-v4-flash`, `step-5-preview-free` → `step-5-preview` (now paid), `minimax-m2.7`, `exo-free`, `ling-2.6-flash-free`, `muse-spark-1.3-contributor-free`, `grok-4.6`. **`opencode-go/deepseek-v4-pro` now exposes variants {high,max}** and `muse-spark-1.3-contributor` exposes {minimal…xhigh} (no max).

**Quota diff vs 2026-10-11:** **no usage multipliers exist anywhere** — the multiplier column is gone from `/docs/go`. **DeepSeek V4.1 Flash 26,000 → 34,670 req/5h** (monthly cap now $80; peak windows 01:00–04:00 and 06:00–10:00 UTC weekdays). New rows: Gemini 4 Argon 440 (50% off through 2026-11-30), Kimi K3.1 Pro 147, MiniMax M3.1 Flash 3,880, Beam Free unlimited (limited time/region), LongCat-2.0 11,400, Kimi K2.7 Code 1,350. **DeepSeek ZDR agreement expires 2026-10-31.** Everything else flat (Muse 45,300 · MiMo-Flash 30,100 · GLM-5.3-Flash 6,320 · GPT-6 Luna 4,230 · Haiku 3,850 · MiMo-Pro 3,250 · GPT-5.6 Luna 2,050 · V4 Pro 1,050 · Step 5 430 · GLM-5.3 220 · Grok 4.7 169).

**Benchmark deltas that mattered:** AA now scores `DeepSeek V4.1 Flash 1017 (Max)` at Intel 45.10 / TB4.0 33.8% (base slug rows read 39–40; whether Go serves the 1017 snapshot is unverified); Step 5 Preview got its first independent agentic figure (TB4.0 33.3%, Intel 43.73, 430 req/5h); every genuinely stronger new model landed in the banned class (Gemini 4 Argon 52.56, Kimi K3.1 Pro 55.09, MiniMax M3.1 Flash policy-barred) — so the eligible pool did not grow this cycle.

## 2026-10-11 — periodic refresh (fixer fallback de-risked; oracle primary → claude-haiku-5-5)

Scope: full `/model-refresh`. Availability re-derived (`opencode models` + `opencode api GET
/api/model`) and quota re-fetched (`/docs/go`); only changed/new models re-researched (the two
new SKUs). Every other chain kept.

**Change:**
- **Fixer fallback:** `opencode-go/qwen3.8-flash (medium)` → `opencode-go/muse-spark-1.3-contributor (high)`.
  Why: the Oct-6 snapshot deferred this exact evaluation ("the quality-per-burn standout — a
  candidate to evaluate next refresh"). Muse wins quality-per-burn (AA 45 xhigh vs qwen 40;
  **45,300 vs 5,400 req/5h** headroom), is route-clean, and removes the config's last
  route-flagged chain position. Gained: a higher-quality, far-higher-headroom, clean-route
  fallback. Lost: the Alibaba family slot in this lane (both fixer positions now run families
  used elsewhere), and muse-spark-1.3 concentration rises to 6 slots (Contributor SKU — trains
  on prompts, accepted any-lane). The displaced `opencode-go/qwen3.8-flash (medium)` remains the
  named non-contributor privacy alternative/revert, but its Go-route flags are still open.
- **Oracle primary:** `opencode-go/gpt-6-luna (xhigh)` → `opencode-go/claude-haiku-5-5 (xhigh)`.
  Why: on quality-per-burn (the mandate's metric) haiku beats luna by ~7–9 AA points at *every*
  effort tier (xhigh 41 vs 34; max 43 vs 37) at slightly *less* burn (3,850 vs 4,230 req/5h), and
  it **clears the config's only cross-lane family overlap** (gpt-6-luna sat at both orchestrator
  fallback and oracle primary) — oracle is now {Anthropic, Meta-muse} vs orchestrator
  {DeepSeek, OpenAI}. The "workhorse, not deep reasoner" caveat did **not** count against haiku:
  luna carries the same supervised-worker profile, and the oracle seat is read-only advice.
  Gained: a stronger primary and better family isolation. Lost: a field-proven model — haiku is
  ~4 days old with no Go-route history; the fallback (muse-spark-1.3, AA 45) is stronger than
  both, and `gpt-6-luna (xhigh)` is the named flip-back if field reports disappoint.

**No change (kept):** orchestrator (`deepseek-v4.1-flash → gpt-6-luna`), explorer
(`deepseek-v4.1-flash → mimo-v2.6-flash`), librarian
(`muse-spark-1.3-contributor → deepseek-v4.1-flash`), designer
(`muse-spark-1.3-contributor (xhigh) → gpt-5.6-luna (xhigh)`), observer (dormant), council
(alpha `deepseek-v4-pro` / beta `mimo-v2.6-pro` / gamma `muse-spark-1.3-contributor`,
synthesis `gpt-5.6-luna (max)`).

**Availability diff vs 2026-10-06:** NEW `opencode-go/claude-haiku-5-5` (variants none→max,
text+image+pdf); NEW `opencode-go/step-5-preview-free` + zen `opencode/step-5-preview-free`
(low/medium/high, text+image+video); the zen free pool gained `opencode/exo-free` (listed but
dead — HTTP 410 ModelDeprecated, #54209) and lost `opencode/fledge-alpha-free`;
`opencode-go/longcat-2.0` variant map went {low,medium,high} → {} (metadata-only; AA 19). All
8 config models remain present with valid variants.

**Watched, not promoted:** `step-5-preview-free` (AA 44 independent, but coding numbers
vendor-only and community reports failures on large PRs; free window ~Oct 15 — policy #11);
`space-bunny` (still zero independent benchmarks; OpenRouter delisted Oct 5); `hy4-preview`
(still rolling). `claude-haiku-5-5` left this list — promoted to oracle primary (see Change).

**Watch-list refresh:** every Oct-6 route flag is still OPEN, no fix — `deepseek-v4.1-flash`
#51990; `deepseek-v4-pro` #1244/#42090; `qwen3.8-flash` #45987/#8003/#100854; GPT-Luna region
403; GLM-5.3-Flash serving still not stabilizing. Quota: still no multipliers anywhere; the
Kimi K2.6 row re-appears in the `/docs/go` table (1,150/$60) but is not in the catalog.

## 2026-10-06 — full refresh (mimo-v2.6-pro leaves oracle; librarian/observer fallbacks de-free'd; xhigh opened to daily lanes)

Scope: full `/model-refresh`, triggered by the user directive to move away from
opencode-go/mimo-v2.6-pro ("It's thinking a lot in loops"). Availability diff vs Sep 27:
opencode-go/glm-5.1 and opencode-go/kimi-k2.6 were removed from the catalog; the free
opencode-go/space-bunny-free became the paid opencode-go/space-bunny ($0.15/$0.60, 3,130
req/5h, $30/m cap, variants low→max) with a zen free sibling retaining the old SKU;
qwen3.8-flash gained a `none` variant; six new zen free models appeared (big-pickle,
fledge-alpha-free, ling-3.0-flash-fin-free, ling-3.1-flash-free, nemotron-3-ultra-free,
nemotron-3.5-lightning-free) — none promoted. Quota re-fetch: new **Go Plus $40/mo** tier
above the $10 Go plan; DeepSeek ZDR window extended to **Oct 31 2026**; deepseek-v4.1-flash's
"4x · Ends Sep 27" multiplier expired → flat permanent 26,000 req/5h; no per-model multipliers
anywhere in the catalog. Go-route re-verify: deepseek-v4-pro and qwen3.8-flash still flagged;
**new** intermittent opaque 400 on deepseek-v4.1-flash (#51990); gpt-5.6-luna/gpt-6-luna clean.

- **oracle: opencode-go/mimo-v2.6-pro → opencode-go/gpt-6-luna (xhigh)**; **fallback
  opencode-go/deepseek-v4-pro (high) → opencode-go/muse-spark-1.3-contributor (xhigh)**.
  Gained: a route-clean lead with a real effort ladder (mimo-pro runs bare and pays a 39–49s
  time-to-first-answer overthinking tax) and a third-family (Meta-muse) fallback that no
  longer depends on deepseek-v4-pro's unresolved Go-route tool-call flag. Lost: mimo-pro's
  AA 46 ceiling and deepseek-v4-pro's strongest-verified correctness (TB2.1 87.9 / SWE 80.6 /
  LCB 93.5) — both surrendered for route reliability; accepted the gpt-6-luna family overlap
  with the orchestrator fallback.
- **librarian: fallback opencode-go/longcat-2.5-preview-free (high) → opencode-go/deepseek-v4.1-flash (high)**
  — primary opencode-go/muse-spark-1.3-contributor (high) unchanged. Gained: a route-clean
  non-contributor rescue with image input and the config's fastest decode. Lost: the
  zero-burn free fallback (longcat had an unverified ~Oct 9 expiry, zero benchmarks, and an
  untested route — session choice was to run paid here).
- **designer: both positions `high` → `xhigh`** — remains opencode-go/muse-spark-1.3-contributor
  (xhigh) → opencode-go/gpt-5.6-luna (xhigh). Gained: runs the lead at the effort its AA 45
  measurement was actually taken at (the `high` variant is unmeasured). Lost: some latency
  headroom in an interactive lane.
- **observer (dormant): fallback opencode-go/longcat-2.5-preview-free (high) → opencode-go/muse-spark-1.3-contributor (high)**
  — primary opencode-go/mimo-v2.6-flash unchanged; observer stays disabled (orchestrator is
  image-capable).
- **council beta: opencode-go/mimo-v2.6-pro — unchanged** by explicit user decision
  ("Keep mimo in council, it's fine"); its overthinking is acceptable in a deliberative seat.
- **Policy #7 amended** in the `/model-refresh` command: `max` is now reserved for
  oracle and council only; `xhigh` is allowed on any lane (`high` remains the default).

Unchanged lanes: orchestrator (opencode-go/deepseek-v4.1-flash:high → opencode-go/gpt-6-luna:high),
explorer (opencode-go/deepseek-v4.1-flash:high → opencode-go/mimo-v2.6-flash), fixer
(opencode-go/deepseek-v4.1-flash:high → opencode-go/qwen3.8-flash:medium), council seats and
synthesis (alpha deepseek-v4-pro, beta mimo-v2.6-pro, gamma muse-spark-1.3-contributor,
synthesis gpt-5.6-luna:max).

## 2026-09-27 — full refresh + second pass (glm-5.3-flash demoted everywhere; longcat-2.5-preview-free added; designer → muse-spark-1.3)

Scope: full refresh plus a targeted oracle/designer re-analysis. New model found:
opencode-go/longcat-2.5-preview-free (2026-09-25, free/unlimited limited-time, image input,
variants low/medium/high); quota table otherwise unchanged, no multipliers. Research
confirmed glm-5.3-flash's slowness is documented (4–60 tok/s serving variance; user-observed
<10–20 tok/s dips) and mimo-v2.6-pro's overthinking is documented (cached "~17.6s TTFT" was
a TTFT/TTFAT mix-up — real time-to-first-answer 39–49s, ~59% reasoning share, no effort knob
on the Go route); go-route re-checks left deepseek-v4-pro and qwen3.8-flash still flagged.
Also confirmed via DeepSeek launch specs (2026-09-10): deepseek-v4.1-flash is natively
image-capable; deepseek-v4-flash-vision-exp is a retired alias routing to it.

- **orchestrator: opencode-go/glm-5.3-flash (high) → opencode-go/deepseek-v4.1-flash (high)**
  — fallback opencode-go/gpt-6-luna (high) unchanged. Gained: fastest model in the config
  (198.6 tok/s) with 4x the burn headroom (26,000 vs 6,320 req/5h) at equal AA class (39–40
  vs 42). Lost: flash's proven route record and always-on screenshot reading until
  v4.1-flash images are exercised on the Go route.
- **librarian: fallback opencode-go/glm-5.3-flash (high) → opencode-go/longcat-2.5-preview-free (high)**
  — primary opencode-go/muse-spark-1.3-contributor (high) unchanged. Gained: zero-burn
  image-capable non-contributor rescue. Lost: a route-proven fallback (longcat untested,
  unofficial ~Oct 9 expiry on watch).
- **council beta: opencode-go/glm-5.3-flash → opencode-go/mimo-v2.6-pro** (plain seat
  string). Gained: AA 46 vs 42, native omnimodal, 3,250 req/5h, family diversity preserved.
  Runner-up glm-5.2 rejected (AA unverified).
- **observer (dormant, still disabled): opencode-go/glm-5.3-flash (high) →
  opencode-go/mimo-v2.6-flash** (promoted from fallback) and **fallback opencode-go/mimo-v2.6-flash →
  opencode-go/longcat-2.5-preview-free (high)**. Orchestrator primary is image-capable, so
  observer stays disabled.
- **designer (second pass): opencode-go/mimo-v2.6-pro (bare) → opencode-go/muse-spark-1.3-contributor (high)**
  — fallback opencode-go/gpt-5.6-luna (high) unchanged. Gained: 4.14s route latency vs 39–49s
  TTFAT, strongest design evidence (Arena WebDev splits, pelican-SVG), 45,300 req/5h. Lost:
  verified AA at designer-usable effort (muse measured only at xhigh 45, v4.3.2 scale; high
  unmeasured) and the privacy-clean profile (muse trains on prompts — accepted per Sep 24
  decision). Recorded revert: opencode-go/mimo-v2.6-pro.
- **oracle + council beta (second pass): no change** — mimo-v2.6-pro kept by user decision
  (overthinking tax buys +4–19pts on hard tasks; tolerable in advisory/deliberative seats);
  oracle fallback opencode-go/deepseek-v4-pro (high) unchanged.
- Impact: glm-5.3-flash absent from every config position (was 4) — catalog revert if Z.ai
  serving stabilizes; MiMo concentration 4 slots, new Muse concentration 3 slots; designer
  escape hatch is its own fallback (opencode-go/gpt-5.6-luna). JSON validated after each
  lane edit.

## 2026-09-26 — Watch-lane refresh + handyman lane removed

Scope: watch-lane refresh (availability re-diffed via `opencode models opencode-go --verbose`
— no new/removed/changed models vs cache; quota economics re-fetched per the never-trust-cache
rule; go-route re-verification via @librarian for all flagged watch-list models) plus a
usage-data verdict on the custom handyman agent.

- **handyman (custom agent): removed** — the entire `agents.handyman` block was deleted
  from `oh-my-opencode-slim.json` after a session-log audit (`opencode.db`: 127 sessions,
  7,268 messages). Usage evidence: 15 assistant messages ever (Sep 20 + Sep 25, 4 sessions;
  18 bash calls) vs the orchestrator's 462 direct bash calls in the last 14 days — the
  orchestrator's delegation rule treats single isolated low-risk actions (commit, lint,
  status check) as direct work, which describes nearly the whole handyman lane, so
  delegation almost never fires. Gained: less dormant config; rare mechanical delegations
  route back to the orchestrator, which demonstrably does this work itself. Lost: the
  option of offloading batch mechanical task cycles without burning orchestrator context
  on tool output — re-add via a custom agent block if that need shows up in practice.
  deepseek-v4-flash leaves the config with the lane (remains in the catalog, cache entry
  kept); muse-spark-1.3 stays librarian primary + council gamma. JSON validated after the
  edit.
- **oracle: no changes** — mimo-v2.6-pro → deepseek-v4-pro (high) reaffirmed; user accepted
  the flagged fallback at the oracle's moderate tool exposure.
- **council: no changes** — alpha deepseek-v4-pro reaffirmed (beta glm-5.3-flash, gamma
  muse-spark-1.3-contributor, synthesis gpt-5.6-luna (max) untouched).
- **quota resolution (no config change)** — DeepSeek V4.1 Flash verified flat 26,000 req/5h
  ($60/m tier); the 4x promo display was removed from opencode.ai/go and the user confirmed
  the increase is permanent (no multiplier). The flat rate clears every floor the promo rate
  did, so no chain was touched.
- **flags unchanged** — deepseek-v4-pro, deepseek-v4-flash, qwen3.8-flash re-verified STILL
  FLAGGED (zero fixes since Apr, no new reports either; #45987 still open unrefuted).

## 2026-09-24 — 9-lane refresh: 5 new Go models onboarded, MiMo experiment lanes

Full-scope run: availability re-diffed via `opencode models opencode-go --verbose` (5 new
models: gpt-6-luna, grok-4.7, mimo-v2.6-flash, mimo-v2.6-pro, space-bunny-free); fresh
research pass via @librarian (AA benchmarks, community, Go-route sweep, req/5h burn table —
all as-of 2026-09-24); every lane reviewed with user confirmation. No removals from the
catalog; space-bunny-free quarantined (privacy contradiction blocker); grok-4.7 rejected on
burn cost. deepseek-v4-pro, deepseek-v4-flash, and qwen3.8-flash remain STILL FLAGGED after
re-verification.

- **orchestrator (fallback):** `opencode-go/deepseek-v4-pro (high)` → `opencode-go/gpt-6-luna (high)`
  — deepseek-v4-pro is STILL FLAGGED (model-side tool-args-as-text, #1244 open since Apr) and
  the orchestrator is the max tool-call exposure lane. gpt-6-luna (released 2026-09-22): AA 37
  (ties 5.6-Luna), route-clean, training Not used, 4,230 req/5h (~2x 5.6-Luna at half price).
  Gained: quota headroom + distinct family (OpenAI vs Zhipu). Lost: fallback coding strength
  (CAI 41 vs 5.6's 43, presentation-Elo regressions) — irrelevant at fallback depth.
  Fixes part of the family-diversity gap (deepseek-v4-pro had been in both orchestrator
  fallback and oracle lead).
- **oracle (primary):** `opencode-go/deepseek-v4-pro (high)` → `opencode-go/mimo-v2.6-pro`
  (plain string — no variant map). **(oracle fallback):** `opencode-go/gpt-5.6-luna (high)` →
  `opencode-go/deepseek-v4-pro (high)` — gpt-5.6-luna leaves the lane. User chose the
  MiMo experiment: AA Intelligence 46, top open-weights (1st/114), now natively omnimodal,
  MIT weights, 3,250 req/5h, privacy Not used/0d. deepseek-v4-pro (still flagged, strongest
  cached verification evidence) demoted to fallback — tolerable at the oracle's moderate
  tool exposure. Gained: ~3x verified intelligence jump on the evidence hierarchy at ~1/3
  of the previous lead's burn; lost: proven coding-agent record (no AA Coding Agent page
  yet) and a fast first token (TTFT ~17.6s, workload-dependent). Now zero family overlap
  with the orchestrator chain {Zhipu, OpenAI} vs {Xiaomi, DeepSeek}.
- **explorer (fallback):** `opencode-go/muse-spark-1.3-contributor (high)` →
  `opencode-go/mimo-v2.6-flash` (plain string — no variant map) — user chose the privacy-clean
  (Not used/0d) trial over the higher-AA training SKU: same $0.14/$0.28 class, 30,100 req/5h,
  vendor TB2.1 87.6. Lost: AA-verified quality (61 vs no AA page) and 45,300 req/5h burn;
  muse's grep-retry failure shapes also matter in a lane that runs grep/AST sweeps. Primary
  unchanged.
- **designer (primary):** `opencode-go/glm-5.3-flash (high)` → `opencode-go/mimo-v2.6-pro`
  (plain string) — verified quality jump: AA 46 vs 42, omnimodal, 3,250 req/5h clears the
  hot-lane floor. User accepted the workload-dependent TTFT (~17.6s, AA-measured, not
  Go-verified). glm-5.3-flash leaves the lane; the recorded revert if TTFT proves annoying
  in practice. grok-4.7 (premium, 169 req/5h) and gpt-6-luna (CAI + presentation-Elo
  regressions) rejected for this quality-first hot lane.
- **handyman (primary):** `opencode-go/mimo-v2.5` → `opencode-go/mimo-v2.6-flash`
  (plain string) — generational like-for-like upgrade at identical burn (30,100 req/5h,
  $0.14/$0.28): vendor TB2.1 87.6 vs v2.5's AA 22–38, same multimodal input, privacy
  Not used/0d. grep-loop failure shapes are shell-irrelevant. Fallback
  `deepseek-v4-flash (low)` unchanged.
- **observer (dormant, stays disabled) fallback:** `opencode-go/mimo-v2.5` →
  `opencode-go/mimo-v2.6-flash` — same gen-upgrade logic in a dormant rescue slot;
  zero AA verification accepted at that depth. Primary glm-5.3-flash (high) unchanged.
- **librarian: no changes** — muse-spark-1.3 (AA 61, full multimodal docs reading,
  45,300 req/5h) beats all candidates including mimo-v2.6-flash on economics + verification.
- **fixer: no changes** — deepseek-v4.1-flash (high) → qwen3.8-flash (medium); mimo-v2.6-pro
  promotion deferred to avoid triple MiMo concentration while the oracle/designer experiments
  bake in; qwen `medium` remains the only variant ceiling safe under both observed variant maps.
- **council: no changes** — alpha deepseek-v4-pro / beta glm-5.3-flash / gamma
  muse-spark-1.3-contributor / synthesis gpt-5.6-luna (max); grok-4.7 (the only
  council-eligible premium) rejected by the user on burn cost; mimo-v2.6-pro recorded as
  the future candidate after the oracle/designer bake-in.

## 2026-09-12 — Escalation agent removed (config + docs)

- **escalation (custom agent): removed** — the `agents.escalation` block
  (`opencode-go/glm-5.3 (max)` → `opencode-go/deepseek-v4-pro (max)`) was deleted from
  `oh-my-opencode-slim.json` at user request (agent existed only in this config; it is not a
  built-in). JSON validated after the edit.

Transition story: the lane was removed by explicit user decision, not by model quality or
quota findings. Consequences: hard-debug/root-cause work (bugs surviving 2+ fix attempts,
high-stakes design) now routes to the built-in `@oracle` chain —
`deepseek-v4-pro (high)` → `gpt-5.6-luna (high)` — instead of a dedicated max-effort lane.
Gained: a simpler roster; glm-5.3 (the config's only sub-floor model at 220 req/5h) leaves
every chain. Lost: the guaranteed max-effort heavyweight lane; `@oracle` now absorbs the
load at `high` effort. Both models remain available in the catalog and in other lanes
(deepseek-v4-pro still fronts oracle and council alpha), so a future re-add needs no
availability re-check beyond a normal refresh. If the lane is ever re-created, glm-5.3 (max)
is the recorded candidate.

Docs synced: `MODEL_CHOICES.md` escalation section removed (handyman is now the only custom
agent); watch-list escalation sub-floor bullet replaced with a removal note.

## 2026-09-12 — Full 10-lane refresh (all lanes reviewed)

Full-scope run: availability re-diffed via `opencode models opencode-go --verbose`; fresh
research pass (AA Intelligence Index v4.3, community, Go-route sweep, Estimated Requests
burn table) via @librarian; every lane reviewed with user confirmation. Burn policy updated
this run: **burn authority is the req/5h Estimated Requests table
(opencode.ai/docs/go#estimated-requests), never dollar cost**; **~800 req/5h floor for hot
lanes** (orchestrator/explorer/librarian/designer/fixer); premium burn only when absolutely
required and no alternatives exist (designer removed from the rare-fire premium exception;
even council ended up fully non-premium).

- **oracle (fallback):** `opencode-go/kimi-k2.6` → `opencode-go/gpt-5.6-luna (high)` — K2.6
  deprecated (AA 31, weakest in config); Luna Pareto-optimal (AA 33 high, CAI 75,
  2,050 req/5h), family-diverse vs the DeepSeek lead. Lost: nothing material (K2.6 was
  deprecated, 42 tok/s, 262k ctx).
- **explorer (primary):** `opencode-go/deepseek-v4-flash (high)` → `opencode-go/deepseek-v4.1-flash (high)`
  — released 2026-09-10: AA 40, 198.6 tok/s (fastest in config), image input, 4× usage
  multiplier → 6,500 req/5h. Kept at `high` (v4-family `max`→null wiring gap). Lost:
  community track record (2 days old).
- **explorer (fallback):** `opencode-go/muse-spark-1.2-contributor (high)` → `opencode-go/muse-spark-1.3-contributor (high)`
  — like-for-like upgrade to AA 61 at identical 45,300 req/5h; the Sep 5 hold (knowledge
  regression) was re-examined and dropped for a query-based research/recon lane.
- **librarian (primary):** `opencode-go/muse-spark-1.2-contributor (high)` → `opencode-go/muse-spark-1.3-contributor (high)`
  — same upgrade case: AA 57→61, full multimodal docs reading, unbilletable burn. Privacy
  tradeoff (prompt training) accepted for this cheap lane.
- **designer:** `opencode-go/kimi-k3` → `opencode-go/glm-5.3-flash (high)` primary;
  `opencode-go/glm-5.3 (high)` → `opencode-go/gpt-5.6-luna (high)` fallback — designer became
  a high-use lane, so the ~800 req/5h floor applies: kimi-k3 (110) and glm-5.3 (220,
  text-only) both fail it. GLM-5.3-flash (AA 42, vision, 6,320) leads; Luna (AA 33, CAI 75,
  2,050) backs. Lost: peak UI-generation quality of kimi-k3; gained a lane that can run all
  day without quota pain.
- **fixer:** `opencode-go/qwen3.8-flash (medium)` → `opencode-go/deepseek-v4.1-flash (high)` primary;
  `opencode-go/deepseek-v4-flash (high)` → `opencode-go/qwen3.8-flash (medium)` fallback —
  user-specified swap; both slots now clear the floor (6,500 / 5,400 req/5h). Qwen demoted
  for its long-task loops / >90k-ctx crawl; also corrects that it has no `high` variant
  (`medium` is the daily-lane ceiling). deepseek-v4-flash leaves fixer entirely (remains in
  handyman fallback).
- **council (synthesis):** `opencode-go/qwen3.7-max` → `opencode-go/gpt-5.6-luna (max)` —
  qwen3.7-max deprecated (AA 30, 170 req/5h); Luna max is a single deep synthesis call and
  2,050 req/5h. The standing premium-synthesis exception ended up unused.
- **council (beta):** `opencode-go/glm-5.3` → `opencode-go/glm-5.3-flash` — sub-floor burn
  (220 vs 6,320 req/5h) no longer justified; vision gain for screenshot-judging seats.
- **council (gamma):** `opencode-go/kimi-k3` → `opencode-go/muse-spark-1.3-contributor` —
  user explicitly swapped out the last premium seat (110 req/5h) for Muse Spark (AA 61,
  45,300 req/5h), accepting the prompt-training tradeoff for a manual-only lane.
- **unchanged:** orchestrator (`glm-5.3-flash (high)` → `deepseek-v4-pro (high)`), observer
  (disabled; dormant chain `glm-5.3-flash (high)` → `mimo-v2.5` reviewed and confirmed),
  escalation (`glm-5.3 (max)` → `deepseek-v4-pro (max)`; sub-floor primary accepted as
  rare-fire tradeoff), handyman (`mimo-v2.5` → `deepseek-v4-flash (low)`).

Gained: every hot lane clears the ~800 req/5h floor; four-family council with zero
premium-burn models; deprecated models (kimi-k2.6, qwen3.7-max) fully retired from chains.
Lost: peak single-shot quality at designer/council depth (kimi-k3); community track record
on the new deepseek-v4.1-flash primary picks. All edits JSON-validated after each lane.

Watch-list: v4.1-flash multiplier/wiring maturity; DSv4 family tool-call text-leak;
qwen3.8-flash no-`high` + max truncation (#45987); hy4-preview free window ended ~2026-09-11;
Muse training acceptance scope. Full list in `MODEL_CHOICES.md`.

## 2026-09-05 — Cheap-lane refresh (scope: explorer, librarian, observer only)

Scope-limited run: review and research covered only the three cheap lanes (explorer,
librarian, observer) per user instruction; all other lanes were left untouched.

- **explorer (fallback):** `opencode-go/muse-spark-1.2-contributor (high)` → `opencode-go/muse-spark-1.3-contributor (high)`
- **librarian:** no change (`opencode-go/muse-spark-1.2-contributor (high)` → `opencode-go/glm-5.3-flash (high)`)
- **observer:** no change (still disabled; chain unchanged)

Transition story: Muse Spark 1.3 (released 2026-09-02, same $0.10/$0.20 price and cheapest
burn class at 45,300 req/5h) is a like-for-like upgrade for codebase recon — AA Intelligence
61 vs 57, Terminal-Bench 2.1 85% vs 80%, MRCR long-context 98.5% vs 66.3%, and vendor-reported
20% fewer tool calls / 25% fewer tokens per task. Its regressions (AA-LCR −4, AA-Omniscience
−3) hit knowledge recall, which is why librarian **held** 1.2: the upgrade case inverts for
docs research. Explorer keeps DeepSeek V4 Flash as primary (7,600 req/5h, spotless Go record).
The 1.3 free twin was considered and rejected: it inherits the Muse Go bug class plus a 500
on `/chat/completions` requiring `/responses` pinning
([#44659](https://github.com/anomalyco/opencode/issues/44659)), while the paid SKU is already
sub-cent. Omen Alpha (released 2026-09-04) was evaluated for all three lanes and held/rejected:
11,600 req/5h makes it quota-cheap (the policy's "premium-burn" label is stale/inverted), but
it has zero independent benchmarks, an undisclosed vendor, and `low`/`high` variants only.
Gained: stronger recon fallback at identical cost. Lost: nothing material; watch the
502-truncation issue ([#2156](https://github.com/anomalyco/opencode/issues/2156)) on large
sweeps — 1.2 is the named revert option.

Watch-list: Omen Alpha identity reveal + first AA run; Muse 1.3 502 fix (#2156);
deepseek-v4-flash-vision-exp graduation; glm-5.3-flash 2x promo persistence; MiniMax ban
re-verification. Full list in `MODEL_CHOICES.md`.

## 2026-08-31 — hy3 lanes review (usage promotion ended)

Scope: hy3 lanes only (fixer, handyman); all other agents untouched.

- **handyman (primary):** `opencode-go/hy3 (low)` → `opencode-go/mimo-v2.5`
- **fixer (fallback):** `opencode-go/hy3 (high)` → `opencode-go/deepseek-v4-flash (high)`
- **fixer (primary, variant fix):** `opencode-go/qwen3.8-flash (high)` → `opencode-go/qwen3.8-flash (medium)`

Transition story: Hy3's 8x usage promotion on OpenCode Go ended 2026-08-30
([PR #46213](https://github.com/anomalyco/opencode/pull/46213)), moving it to the standard 6x
tier ($60 usage) — so the economics alone no longer forced a change. The evidence pass
(benchmarks, community reports, Go-route reliability) did: Hy3 carries open Go-route hard
gates — empty SSE streams on `stream:true` ([#43852](https://github.com/anomalyco/opencode/issues/43852)),
auto-compaction never triggering with context pinned at 196,608 raw tokens for silent
multi-million-token cost blowups ([#45168](https://github.com/anomalyco/opencode/issues/45168),
[#46137](https://github.com/anomalyco/opencode/issues/46137)), and 30s–7min time-to-first-token
([#44579](https://github.com/anomalyco/opencode/issues/44579)) — plus text-only input and the
lowest relevant intelligence (AA 42 vs DeepSeek V4 Flash 52, MiMo weaker but reliable).
Handyman primary went to MiMo V2.5 (same 6x cheapest tier, most reliable cheap model on the
Go route, most concise output, multimodal). Fixer fallback went to DeepSeek V4 Flash high
(stronger rescue capability; family-diverse from the qwen lead). The fixer variant change
corrects an invalid setting: qwen3.8-flash's variant map is `low/medium/xhigh` — `high` was
silently ignored; `medium` is the highest effort inside the daily-lane `high` ceiling, and
`max` is broken on the Go route
([#45987](https://github.com/anomalyco/opencode/issues/45987)). Gained: route reliability,
rescue quality, multimodal handyman. Lost: hy3's careful low-edit behavior and its (former)
8x subsidy; fallback burn rises from 6x to 3x tier. Hy3 is now absent from the config.

Watch-list: deepseek-v4-flash rejects `minimum`/`maximum` tool-schema keywords
([#43378](https://github.com/anomalyco/opencode/issues/43378)); qwen3.8-flash `max` truncation
([#45987](https://github.com/anomalyco/opencode/issues/45987)); hy3 fixes
([#43852](https://github.com/anomalyco/opencode/issues/43852) /
[#45168](https://github.com/anomalyco/opencode/issues/45168)) would make it viable again at
its 6x tier.

## 2026-08-31 — Changelog created (baseline)

No changes in this entry. Roster at bootstrap:

- **orchestrator:** `opencode-go/glm-5.3-flash (high)` → `opencode-go/deepseek-v4-pro (high)`
- **oracle:** `opencode-go/deepseek-v4-pro (high)` → `opencode-go/kimi-k2.6`
- **explorer:** `opencode-go/deepseek-v4-flash (high)` → `opencode-go/muse-spark-1.2-contributor (high)`
- **librarian:** `opencode-go/muse-spark-1.2-contributor (high)` → `opencode-go/glm-5.3-flash (high)`
- **designer:** `opencode-go/kimi-k3` → `opencode-go/glm-5.3 (high)`
- **fixer:** `opencode-go/qwen3.8-flash (high)` → `opencode-go/hy3 (high)`
- **observer (disabled):** `opencode-go/glm-5.3-flash (high)` → `opencode-go/mimo-v2.5`
- **council:** alpha=`opencode-go/deepseek-v4-pro` · beta=`opencode-go/glm-5.3` · gamma=`opencode-go/kimi-k3` · synthesis=`opencode-go/qwen3.7-max`
- **escalation:** `opencode-go/glm-5.3 (max)` → `opencode-go/deepseek-v4-pro (max)`
- **handyman:** `opencode-go/hy3 (low)` → `opencode-go/deepseek-v4-flash (low)`
