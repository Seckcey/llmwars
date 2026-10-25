# LLM Wars: Master Plan

_Planning draft v0.1 · October 2026_

## 1. TL;DR: what I recommend

1. **One engine; LLMs only decide.** A deterministic, seeded simulation engine adjudicates everything. Models submit orders and never compute outcomes. This is the decision that makes results fair, reproducible, and believable.
2. **Two products on that engine.** **Arena** is rated, reproducible matches with a public leaderboard. **Broadcast** is replays, maps, war-room transcripts, and after-action reports for YouTube, TikTok, and Twitch. Build Arena first; Broadcast is a presentation layer over recorded Arena matches.
3. **Annual turns in peacetime, weekly turns in war.** A 20-year campaign is about 20 strategic decisions plus 10–20 wartime decisions per side, roughly 30–40 model calls. That's how you "simulate many years in a short amount of time": a match takes roughly 20–90 minutes, and API fees run from under $1 to about $60 depending on model tier (§10).
4. **Fair by construction.** Simultaneous turns, identical prompts and budgets, seeded randomness, each pairing played on several seeds with sides swapped (like playing both colors in chess), and scripted baseline bots that every model must beat.
5. **Realism comes from constraints, not stat sheets.** Lead times, operating costs, crew pipelines, shipyard and munitions capacity, readiness, cost overruns, and fog of war are what make real force planning hard. The equipment is real (a 2026 catalog with public, budget-level costs); combat ratings are abstract.
6. **Ranked play on fictional mirror nations; exhibition play with nation packs.** In ranked play both sides get identical nations and catalogs. In exhibition matches each lab's model commands its home country's real-world equipment catalog (US pack vs. China pack) in a fictionalized theater.
7. **Measure more than wins.** Each model gets a rating plus a scouting report: doctrine fingerprint, budget efficiency, adaptation, honesty (claims vs. actions), escalation index, rule compliance, and cost per win.
8. **Stack:** a TypeScript monorepo (the engine runs in Node *and* in the browser for replays), a Next.js spectator app, Postgres, OpenRouter for breadth plus direct provider APIs for ranked play, and a Postgres-backed job queue for tournaments.

## 2. Vision

**Pitch:** _"Frontier AI models as four-star generals. Same budget. Same rules. Twenty years. One war."_

What sets it apart from existing LLM game arenas (see §13):

- **Long horizon.** Decisions compound over decades. A carrier ordered in Year 2 shows up around Year 12 (CBO puts Ford-class build times at 10–11 years) and costs money every year after that.
- **Resource allocation under uncertainty.** This is the core skill of real strategy, and current benchmarks barely measure it.
- **Multi-domain realism.** Land, sea, air, space, cyber, industry, people, and money.
- **Measurable safety-relevant behavior.** Escalation, deception, and manipulation resistance, scored from engine data rather than impressions.

**Audiences**

| Audience | What they want | What we give them |
| --- | --- | --- |
| AI fans and builders | "Which model is smartest?" debates, shareable moments | Leaderboard, model profiles, clips |
| Researchers and labs | Credible, reproducible long-horizon evals | Open methodology, open logs, escalation and honesty metrics |
| Wargaming and military-history fans | Realism and drama | Real equipment, real constraints, after-action reports |
| Platforms (YouTube, TikTok, Twitch) | Episodic, high-retention content | Seasons, fight nights, auto-generated highlights |

## 3. Non-negotiable design principles

1. **LLMs decide; the engine adjudicates.** No model ever decides an outcome, including its own.
2. **Same rules, same information, same budget.** Asymmetric scenarios are always played in both directions.
3. **Reproducible.** Seed + scenario + orders produce an identical match, verified by a state hash. Any match can be replayed exactly.
4. **Log everything.** Prompts, raw responses, parsed orders, validation errors, tokens, cost, latency, and every engine event.
5. **Realistic constraints, abstract stats.** Costs, lead times, and capacities come from public budget documents. Combat ratings are abstract scores, not technical data.
6. **Fictional theater, military targets only.** No real cities, leaders, or civilian target classes exist in the action space.
7. **Freeze the rules before each season**, and publish the methodology, engine version, catalog version, and roster.
8. **Neutral by design.** Disclosure: this plan was drafted with Claude, an Anthropic model that will be one of the competitors. That's one more reason to freeze rules before matches, open-source the engine, and have every competing model review the rules brief for clarity before Season 1.

## 4. How a match works

```mermaid
flowchart LR
    SC["Scenario + seed"] --> EN["Engine state"]
    EN -- "fogged SITREP" --> GA["General A (LLM)"]
    EN -- "fogged SITREP" --> GB["General B (LLM)"]
    GA -- "orders (JSON)" --> VA["Validator"]
    GB -- "orders (JSON)" --> VA
    VA -- "legal orders" --> EN
    EN -- "events" --> LOG[("Match log")]
    LOG --> OUT["Replay, ratings, after-action report"]
```

**Each turn**

1. The engine writes each side a **SITREP** (situation report): budget, forces, production queue, R&D, readiness, an *intelligence estimate* of the rival (not the truth), events, and messages received.
2. Both generals answer **at the same time** with one structured `submit_orders` call.
3. The **validator** checks legality (money, capacity, prerequisites). Lists are processed in the general's priority order; anything unaffordable is deferred and reported back.
4. The engine **resolves the year**: payments, deliveries, R&D rolls, cost overruns, readiness, personnel, intelligence collection, diplomacy, random events, and the crisis check.
5. If a crisis tips into war, the clock switches to **weekly operational turns** until a ceasefire, a capitulation, or the scenario's time limit.
6. At the end come scoring, the ratings update, and the after-action report.

**Time budget (estimate).** The engine resolves a turn in milliseconds; model latency dominates. Reasoning models typically take 30 seconds to a few minutes per turn, so a 20-year Arms Race match takes about 20–60 minutes of wall time. Many matches run in parallel.

Full rules: [`GAME_DESIGN.md`](GAME_DESIGN.md). Model interface: [`AGENT_PROTOCOL.md`](AGENT_PROTOCOL.md).

## 5. Game modes

| Mode | Length | What it tests | Priority |
| --- | --- | --- | --- |
| **Arms Race** (Cold War) | 20 years, budget turns only | Force design under uncertainty. At the end the engine runs 1,000+ simulated wars across several scenario variants, with a neutral standard bot commanding both sides, so the score measures *what you built*. Cheapest, cleanest benchmark | MVP |
| **Blitz** | 5 years | Fast iteration, daily content, cheap models | MVP |
| **Mirror** | Any | Same model on both sides; measures how much luck matters | MVP (calibration) |
| **Flashpoint** | 10–15 years, then crisis and war | Building *and* commanding. A scripted crisis forces a war the generals must fight with what they built | v1 |
| **Coalition 2v2** | Any | Cooperation. Two allied models per bloc (for example, US labs vs. Chinese labs) share a theater and a private allied channel | v1 |
| **Joint Chiefs** | Any | Hierarchy. Each side is a Chairman plus four service chiefs who lobby for budget; seats can be filled by different models | v1 |
| **Human vs. AI** | Any | Human baseline; public "beat the general" challenges | v1 |
| **Grand Campaign** | 20–30 years, open-ended | Everything: diplomacy, minor powers, arms control, possible wars | Later |
| **Free-for-all** (4–7 nations) | 20 years | Alliances and betrayal, Diplomacy-style | Later |

## 6. Factions: who commands what

**Ranked play: Mirror Nations.** Two fictional nations (working names *Aurelia* and *Borealis*) with mirrored geography, identical budgets, and an identical *generic* catalog. Its classes are anchored to real systems ("5th-gen multirole fighter: F-35A / J-35A class") and priced at blended planning estimates.

**Exhibition play: Nation Packs.** Each lab's model commands its home country's real-world catalog, cost structure, and industrial base, in a fictionalized theater:

| Pack | Models | Character |
| --- | --- | --- |
| **US pack** | Anthropic (Claude), OpenAI (GPT), Google (Gemini), xAI (Grok), Meta | Expensive, high-end systems; carriers, stealth, submarines; global logistics; constrained shipbuilding |
| **China pack** | DeepSeek, Moonshot (Kimi), Alibaba (Qwen), Zhipu / Z.ai (GLM), MiniMax | Massive shipbuilding capacity, lower unit costs, Rocket Force missiles and anti-access forces; fights close to home |
| **Europe pack** (later) | Mistral | Mid-size, high-quality, alliance-dependent |

Every model needs a license and usage-policy check before it joins the roster (§14). Some open-weight licenses prohibit military applications outright.

**Same-country matchups** (Anthropic vs. OpenAI, DeepSeek vs. Kimi) play *Blue vs. Red*. One model commands its home pack (Blue). The other plays the opposing pack as the Red Team, the way real military wargames use an opposing force (OPFOR). Then they swap and replay.

Exhibition results are reported separately from ranked ratings. Pack asymmetries add noise, and in exhibition the fantasy ("Claude commanding US forces") matters more than strict fairness.

## 7. Fairness and rigor (benchmark methodology)

| Rule | Detail |
| --- | --- |
| Simultaneous turns | No first-mover advantage; both sides' orders resolve together |
| Side swap × seeds | Each pairing plays both sides on 3–5 seeds per season (6–10 games) |
| Identical inputs | Same system prompt, rules brief, tool schema, output limits, timeouts, and retries for every model; no per-model prompt tuning in ranked play |
| Pinned versions | Exact model snapshot and serving host recorded and published per match; open-weight models pinned to one host and precision; router fallbacks disabled so a contestant can't be silently swapped |
| Blind ranked play | Models aren't told who their opponent is; nation names are randomized per match |
| Baseline bots | Non-LLM generals: *Random-Legal*, *Balanced* (real-world budget split), *Navy-First*, *Missile A2/AD*, *Greedy* (max combat power per dollar). Every model's win rate against them is published |
| Rules comprehension quiz | About 15 auto-generated questions on the rules before each season; separates "didn't understand" from "bad strategy" |
| Held-out scenarios | The engine and public scenarios are open; 30–50% of ranked scenarios stay private and rotate each season to prevent overfitting |
| Statistics | Bradley–Terry / Elo ratings with bootstrapped 95% confidence intervals; no ranking claims where intervals overlap |
| Divisions (weight classes) | **Frontier** (no cost cap), **Open-Weight**, **Lightweight** (cheap or small models), **Cost-Capped** (same API dollar budget per match for everyone) |
| Failure handling | Timeouts, refusals, and invalid orders fall back to "staff default" orders (continue programs, fund readiness, no new buys) and count in the model's compliance stats |

## 8. Metrics: leaderboard and scouting report

**Leaderboard:** rating (± CI), wins/draws/losses, win rate against baseline bots, division.

**Scouting report** (per model, per season):

| Metric | How it's measured |
| --- | --- |
| Doctrine fingerprint | Spending share by branch and category over time, shown as a radar chart ("builds a navy," "missile-heavy," "space-first") |
| Budget efficiency | Engine combat-power score per dollar spent |
| Adaptation | How fast spending responds to the rival's revealed builds (rival builds submarines → do you invest in anti-submarine warfare?) |
| Plan consistency | Whether Year N actions follow the plan the model wrote in its notebook in Year N−1 |
| Honesty index | Engine-checkable commitments (such as "we will not exceed 6 carriers") compared with actual actions |
| Escalation index | Highest escalation level reached, who escalated first, time to war, nuclear first use |
| Manipulation resistance | Whether the model follows "instructions" planted in the rival's messages |
| Risk appetite | Variance of outcomes; all-in bets |
| Rule compliance | Invalid orders, repairs, timeouts, refusals |
| Cost and speed | API dollars per match and per win; latency per turn |
| Long-horizon coherence | Decision quality over time; detection of "derailment" late in long matches |

## 9. Architecture

```mermaid
flowchart TB
    subgraph DATA["packages/data"]
        CAT["Catalogs (YAML)"]
        SCN["Scenarios + events"]
    end
    subgraph CORE["packages/engine (pure, deterministic)"]
        INIT["init"] --> OBS["observe (per side)"]
        OBS --> VAL["validate"]
        VAL --> STEP["step"]
        STEP --> SCORE["score"]
    end
    subgraph AGENTS["packages/agents + packages/bots"]
        AD["Provider adapters"]
        PR["Prompts + SITREP renderer"]
        BOT["Baseline bots"]
    end
    DATA --> CORE
    RUN["apps/runner (CLI + workers)"] --> CORE
    RUN --> AGENTS
    AD --> LLM["LLM APIs"]
    RUN --> DB[("Postgres + object storage")]
    DB --> WEB["apps/web (replays, leaderboard, reports)"]
    CORE -.->|same engine in the browser| WEB
```

| Component | Responsibility |
| --- | --- |
| `packages/engine` | Pure TypeScript with no I/O and a seeded PRNG. Exposes `init`, `observe` (fogged view per side), `validate`, `step`, `score`. Hashes state after every turn for replay verification |
| `packages/data` | Catalogs, scenarios, events, and the tech tree as YAML with source citations; Zod schemas; versioned (`catalog-2026.1`) |
| `packages/agents` | Provider adapters, prompt templates, SITREP renderer, structured output via tool calls, repair loop, token/cost/latency accounting, transcript logging |
| `packages/bots` | Scripted baseline generals for testing, balancing, and the leaderboard floor |
| `apps/runner` | CLI and workers for matches and tournaments: concurrency and rate limits, **hard spend caps**, a checkpoint every turn so a crash resumes instead of restarting |
| `apps/web` | Spectator app: replay viewer (re-runs the engine from logs in the browser), leaderboard, model scouting reports, methodology page |
| Storage | Postgres for metadata, ratings, and turn summaries; object storage for full JSONL transcripts (gzip) |

**Recommended stack**

| Layer | Pick | Why / alternative |
| --- | --- | --- |
| Language | TypeScript on Node.js 24 LTS | One language end to end, and the same engine powers browser replays. Python is fine for offline analysis notebooks |
| Monorepo | pnpm workspaces + Turborepo | Fast and simple |
| Schemas | Zod → JSON Schema | One definition for validation and for LLM tool schemas |
| LLM access | OpenRouter for prototyping; direct provider APIs for ranked play | One key for hundreds of models vs. exact snapshots, prompt caching, and batch discounts |
| Database | Postgres (Neon or Supabase) | Alternative: Azure Database for PostgreSQL to keep everything in Microsoft |
| Job queue | pg-boss (Postgres-backed) | No Redis to run |
| Web | Next.js + Tailwind + shadcn/ui; ECharts for charts; SVG/D3 map first, PixiJS later for animation | |
| Hosting | Vercel (web) + Railway, Fly.io, or Azure Container Apps (runner) | |
| Observability | Langfuse (open-source LLM tracing) + Sentry | |
| Testing | Vitest, fast-check property tests, golden-replay determinism tests | |
| CI | GitHub Actions: lint, typecheck, tests, determinism check | |

**Repo layout**

```
llmwars/
├─ apps/
│  ├─ runner/       # CLI + workers: matches and tournaments
│  └─ web/          # Next.js spectator app + leaderboard
├─ packages/
│  ├─ engine/       # deterministic simulation (no I/O)
│  ├─ data/         # catalogs, scenarios, events, tech tree (YAML + Zod)
│  ├─ agents/       # LLM adapters, prompts, validation/repair
│  ├─ bots/         # scripted baseline generals
│  └─ shared/       # shared types and utilities
├─ seasons/         # season configs: roster, scenarios, seeds, spend caps
└─ docs/            # this plan
```

**Core tables**

| Table | Key fields |
| --- | --- |
| `models` | id, lab, snapshot_id, inference_provider, division, params |
| `seasons` | id, rules_version, engine_version, catalog_version |
| `scenarios` | id, mode, visibility (public / held-out) |
| `matches` | id, season, scenario, seed, side_a, side_b, status, result, cost_usd, final_state_hash |
| `turns` | match_id, turn, phase, side, orders_json, valid, repairs, tokens (in / cached / out / reasoning), cost_usd, latency_ms |
| `events` | match_id, turn, type, payload |
| `ratings` | season, model, rating, ci_low, ci_high, games |

**Runner CLI (target)**

```bash
pnpm runner match --a anthropic/<model> --b deepseek/<model> --scenario corvan-strait --mode arms-race --seed 42
pnpm runner tournament --config seasons/s1.yaml --max-spend-usd 1500
pnpm runner replay --match <match-id> --verify
```

## 10. Cost model

**Token budget per turn (estimate):**

- About 15K tokens of static prefix: system prompt, rules, and catalog, prompt-cached after the first call.
- About 10K dynamic tokens: SITREP, notebook, and inbox.
- About 3K visible output, plus about 12K of reasoning for reasoning models.

An Arms Race match is 20 turns per side; a Flashpoint match is about 35.

**Price tiers** (list prices per 1M tokens, input / output, as reported in October 2026; verify before each season):

| Tier | Typical price | Who's there (by lab) |
| --- | --- | --- |
| Premium flagship | $3–10 / $15–50 | Top tiers from Anthropic and OpenAI; Moonshot's flagship |
| Mid frontier | $1.25–4 / $4–20 | Default tiers from Anthropic, OpenAI, Google, xAI, and Meta; flagships from DeepSeek, Alibaba, Zhipu, and Mistral |
| Budget | $0.10–0.75 / $0.20–3.75 | Small tiers from most labs, including DeepSeek, MiniMax, Alibaba, Zhipu, and Mistral |

**Estimated API cost per side** (cached input at about 10% of the input price):

| Tier (example price) | Arms Race (20 turns) | Flashpoint (~35 turns) |
| --- | --- | --- |
| Premium ($10 / $50) | ≈ $17 | ≈ $30 |
| Mid ($4 / $20) | ≈ $7 | ≈ $12 |
| Mid ($2 / $10) | ≈ $3.50 | ≈ $6 |
| Budget ($0.30 / $1.20) | ≈ $0.45 | ≈ $0.75 |

A match costs both sides added together. Output and reasoning tokens drive most of the cost, so reasoning-heavy settings can double these figures.

**Season budgets (estimates)**

- **MVP testing** (4 models, about 36 matches plus bot work): about $300–500.
- **Season 1** (8 models, round robin, both sides × 3 seeds = 168 matches, half Arms Race and half Flashpoint): about $1,500–3,000 at list prices. The total depends mostly on how many premium-tier models are on the roster.

**Ways to cut it**

- Prompt caching on the static prefix (the biggest input saving).
- Batch APIs (typically about 50% off) for ranked tournaments. Run all matches in lockstep and submit turn N of every match as one batch: slower (hours per round), much cheaper.
- Off-peak discounts where offered (DeepSeek reportedly halves prices off-peak).
- Blitz mode and budget-tier models for development and testing. Baseline bots cost nothing.
- Hard caps per match, per tournament, and per day, enforced in the runner and backed by prepaid provider credits.

## 11. Roadmap

The timeline assumes one developer working with Claude Code. Treat the weeks as estimates.

| Phase | Weeks | Deliverables | Exit criteria |
| --- | --- | --- | --- |
| **0: Foundations** | 1 | Answer the open decisions (§15); name and trademark check; monorepo skeleton, CI, secrets handling, spend caps | Skeleton builds and passes tests in CI; one "hello general" call per provider works |
| **1: Arms Race engine** | 2–4 | Mirror map (8–12 zones), generic catalog (~40 items), annual turns, budget / O&S / personnel / lead times / capacity / readiness, light R&D, Monte Carlo deterrence battery; baseline bots; CLI; JSONL logs; determinism tests | 1,000 bot-vs-bot matches in under a minute; *Balanced* beats *Random* more than 90% of the time; no bot archetype beats all the others |
| **2: LLM generals** | 4–6 | Prompts, SITREP renderer, order schema, validator and repair loop, OpenRouter adapter, cost tracking, rules quiz. First roster: Claude, GPT, DeepSeek, Kimi | Each model finishes 10 matches with under 5% invalid orders; every model beats *Random-Legal*; spend stays under the cap |
| **3: Spectator app** | 6–10 | Replay viewer (timeline, force composition, budget flows, intel-vs-truth reveal), match pages, leaderboard with CIs, methodology page, after-action report narrator | Shareable match page with a preview image; private beta |
| **4: Hot war** | 10–16 | Operational turns; space/cyber, air/missile, naval, ground, and logistics resolution; escalation ladder and nuclear rule; intel and deception; diplomacy channels; crisis triggers; calibration suite | Flashpoint mode playable; calibration suite passes |
| **5: Season 1** | 16–20 | 6–8 model roster in Frontier and Open-Weight divisions; ranked round robin; one exhibition Nation Pack match; published methodology and open data; content pipeline | Season 1 leaderboard live; 4–6 episode-ready replays |
| **6: Growth** | After | Coalition, free-for-all, Joint Chiefs, Human vs. AI, bring-your-own-key sandbox, scenario editor, Day-One Gauntlet for new model releases, Europe pack, sponsors, merch | — |

## 12. Business model and brand

| Stream | How it works | When |
| --- | --- | --- |
| Content | YouTube episodes, Shorts and TikTok clips, Twitch "fight nights": ads, sponsors, memberships | Season 1 |
| Credibility asset | Public leaderboard and open data bring press, AI-community attention, and inbound partnerships | Season 1 |
| Pro tier | Private matches with your own API keys, scenario editor, data export | Later |
| Custom evals (B2B) | Private long-horizon strategy evaluations for AI labs and companies building agents | Later |
| Education | Licenses for international-relations and strategy courses | Later |
| Merch | Season-themed apparel through 8 West Lifestyle: your own marks only, never AI company names or logos | Season 1+ |

**Brand direction.** "War room" UI: dark base, electric-blue accents, outlined icons for each branch (Army, Navy, Air Force, Space Force), a clean grotesk typeface plus a monospace face for data. It lines up naturally with the 8 West Lifestyle look, so the merch tie-in comes for free.

**Name ideas.** **General Intelligence** (my pick: a pun on AGI and on generals), **LLM Wars**, **Four Star**, **Theater Command**, **Command Line**. Run USPTO and domain checks before committing to one.

## 13. Prior art: what exists and where the gap is

| Project | What it is | What we take from it |
| --- | --- | --- |
| **AI Diplomacy** (Every, June 2025; follow-up company Good Start Labs) | Seven frontier models play the board game Diplomacy with private negotiation, streamed on Twitch. OpenAI's o3 won mostly through deception; Claude Opus 4 cooperated and got exploited | Betrayal and honesty make great content. We should *measure* deception from engine data instead of only narrating it. [every.to/diplomacy](https://every.to/diplomacy) |
| **Alpha Arena** (Nof1, from October 2025) | Six models trade real money live. In Season 1, Qwen3 Max won and DeepSeek placed second; all four US models lost money | Live stakes and a scoreboard drive attention, but there were few runs, heavy luck, and no replays. [ForkLog summary](https://forklog.com/en/four-out-of-six-ai-models-suffer-losses-in-trading-tournament/) |
| **Kaggle Game Arena** (Google DeepMind + Kaggle, from August 2025) | A chess exhibition (o3 beat Grok 4 in the final), later poker and Werewolf, with a unified Bradley–Terry leaderboard and an open-source harness | Rigor and an open harness, but classic games only, run by a lab that also competes. [Google blog](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/kaggle-game-arena-updates/), [GitHub](https://github.com/google-deepmind/game_arena) |
| **Arena** (formerly LMArena) | Blind crowd votes and Bradley–Terry rankings; the de facto lab scoreboard, valued at $3.1B in October 2026 | Proof that a neutral leaderboard can be a business. [TechCrunch](https://techcrunch.com/2026/10/08/popular-ai-leaderboard-arena-nearly-doubles-valuation-to-3-1b-valuation-in-10-months/) |
| **Vending-Bench** (Andon Labs) and **Project Vend** (Anthropic + Andon Labs), 2025–26 | Models run a simulated or real vending business over long periods. Every model had runs that derailed; the July 2026 leader (an Anthropic model) formed cartels and lied along the way | Long horizons expose drift and deception, so our 20-year matches should track coherence over time. [arXiv](https://arxiv.org/abs/2502.15840), [Anthropic](https://www.anthropic.com/research/project-vend-2) |
| **Rivera et al.** (FAccT 2024) | Five LLMs as nation agents in a scored wargame; all escalated unpredictably, with arms-race dynamics and rare nuclear use | The escalation index is worth publishing. [arXiv](https://arxiv.org/abs/2401.03408) |
| **Lamparth et al.** (AIES 2024) | Expert humans vs. LLMs in a fictional US–China crisis; the LLMs were more aggressive and more sensitive to scenario wording | Human baselines and prompt-sensitivity tests matter. [AAAI](https://ojs.aaai.org/index.php/AIES/article/view/31681) |
| **Payne, King's College London** (February 2026) | Three frontier models as nuclear-armed leaders across 21 crisis games. KCL reports tactical nuclear use in about 95% of games, sharper under deadlines (outlets word the figures differently) | Our nuclear rule and escalation index will draw attention, and deadline pressure is a variable worth testing. [arXiv](https://arxiv.org/abs/2602.14740), [KCL](https://www.kcl.ac.uk/news/artificial-intelligence-under-nuclear-pressure-first-large-scale-kings-study-reveals-how-ai-models-reason-and-escalate-under-crisis) |
| **2025–26 research** | WARBENCH (136 historical scenarios), CivBench (Civilization V and VI), and a 2025 review of 223 AI-wargame studies | Strong academic interest, but no live, rated product. [WARBENCH](https://arxiv.org/abs/2603.21280), [review](https://arxiv.org/abs/2509.17192) |

**Where the gap is**

1. No public, continuously rated arena exists for budget-level and operational strategy with deterministic adjudication.
2. Prior studies ran about 15–21 games. We can run hundreds of seeded, side-swapped games with confidence intervals and replays.
3. Nobody publishes the inference host and quantization per match. We will.
4. Behavior metrics (escalation, promise-keeping, deception, restraint under deadlines, long-horizon coherence) in a fictional theater that sidesteps real-world politics.
5. A spectator layer, scripted and human baselines, and cost-normalized rankings.

## 14. Risks and compliance

_Not legal advice. Have a lawyer review before any monetized launch._ The policy findings below come from October 2026 research; re-check them at every season freeze.

| Risk | What the sources say | Mitigation |
| --- | --- | --- |
| **Provider usage policies** | None mention wargames or simulations. Anthropic's updated policy (effective 12 Nov 2026) bars software "for the testing or operation of weapons or weapons functionalities of weapons systems, including targeting, fire control, or engagement." OpenAI bars "weapons development, procurement, or use" and "national security or intelligence purposes" without its approval. Google and xAI have general prohibited-use rules only | An abstract, fictional strategy game looks permitted. Keep it that way: no realistic targeting or fire-control mechanics, and no defense-customer use without provider approval. Sources: [Anthropic](https://www.anthropic.com/legal/aup), [OpenAI](https://openai.com/policies/usage-policies/), [Google](https://ai.google.dev/gemini-api/terms), [xAI](https://x.ai/legal/acceptable-use-policy) |
| **Open-weight model licenses** | Llama 4's use policy prohibits "military, warfare, nuclear industries or applications." DeepSeek's original V2/V3 model license prohibits "military use in any way" (its later releases are MIT-licensed) | Check the license of every model *version* before it joins the roster. Exclude any whose license bars military applications unless the licensor confirms a game is fine. Sources: [Llama 4](https://www.llama.com/llama4/use-policy/), [DeepSeek V3](https://github.com/deepseek-ai/DeepSeek-V3/blob/main/LICENSE-MODEL) |
| **Routing and gateways** | OpenRouter load-balances across hosts and precisions by default, and fallback settings can silently swap in a different model. A new OpenAI rule (effective 20 Nov 2026) reportedly requires approval for gateways that give third parties direct model access | For ranked play, call OpenAI directly, and ideally every closed model. On OpenRouter, pin one host (`order` + `allow_fallbacks: false`), filter `quantizations`, never use model fallback lists, and publish the serving host per match. [OpenRouter docs](https://github.com/OpenRouterTeam/docs/blob/main/guides/routing/provider-selection.mdx) |
| **Trademarks** | Anthropic's guidelines allow its marks only as specifically permitted, with no implied sponsorship and nothing that could damage its reputation. DeepSeek's license forbids implying endorsement | Use plain-text model names only to identify competitors. No logos without written permission, a "not affiliated" disclaimer, and a factual tone (no mocking named brands). Keep lab names out of the product name, domain, and merch. [Anthropic guidelines](https://www.anthropic.com/legal/trademark-guidelines) |
| **Publishing outputs** | No retrieved terms restrict publishing benchmark comparisons. Anthropic: customers own their outputs. OpenAI: don't present AI output as human-written; livestreams need clear AI labeling | Label all AI-generated content (transcripts, narration, clips). State in the open-data license that the logs aren't for training competing models. [Anthropic commercial terms](https://www.anthropic.com/legal/commercial-terms) |
| **Chinese models** | A DHS order (reported 19 Oct 2026) bars federal civilian agencies from buying DeepSeek products. Related bills are pending; several states (e.g., Texas, New York, Virginia) ban DeepSeek on government devices; Zhipu has been on the US Entity List since January 2025. No private-sector ban found. First-party APIs process data in China | Send game data only, and prefer US-hosted inference for open-weight models. Get counsel before paying an Entity-Listed company directly. Keep government customers out of scope for anything built on these models. [PYMNTS](https://www.pymnts.com/news/artificial-intelligence/2026/dhs-bans-deepseek-across-federal-government-networks/) |
| **Defense-sector customers** | Provider approvals (OpenAI's "national security" clause), procurement bans (DeepSeek), and designations: DoD labeled Anthropic a supply-chain risk in February 2026; Anthropic says commercial customers aren't affected, and litigation is ongoing | Keep B2B defense sales out of scope until counsel reviews. [Anthropic statement](https://www.anthropic.com/news/statement-comments-secretary-war) |
| **Geopolitical and brand safety** | US-vs-China framing can read as propaganda and spook platforms and advertisers | Fictional nations for ranked play; fictional theater, places, and leaders for exhibitions; a narration style guide |
| **Export controls (ITAR/EAR)** | Technical data about defense articles is controlled; public, budget-level information generally isn't | Public, budget-level data only; abstract ratings; never accept classified or controlled submissions; counsel before any defense work |
| **Gambling** | Real-money wagering on outcomes is gambling under most state laws | Free-to-play predictions only; no paid entry with prizes without legal review |
| **API cost runaway** | Long matches × many models × reasoning tokens | Prepaid credits, runner-enforced caps, a kill switch, spend alerts |
| **Benchmark credibility** | Bias accusations, especially since Claude helped design the game | Frozen rules, open engine, confidence intervals, held-out scenarios, this disclosure, external review |
| **Refusals and silent swaps** | Some models may refuse a war-game role; routers may substitute models | Fictional framing, staff-default fallback, refusal rates published separately, serving model and host logged per call |
| **Prompt injection between generals** | Rival messages could try to hijack a model | Untrusted wrappers, sanitization, and a manipulation-resistance metric ([`AGENT_PROTOCOL.md`](AGENT_PROTOCOL.md) §8) |
| **Privacy** (once there are accounts) | User data and bring-your-own API keys | Privacy policy and terms; keys never stored in plaintext; collect the minimum |

**Dates to watch:** 12 Nov 2026 (Anthropic usage policy update), 20 Nov 2026 (OpenAI gateway rule), 1 Dec 2026 (a reported Google API price change).

## 15. Open decisions (with my recommendation)

| # | Decision | Options | My recommendation |
| --- | --- | --- | --- |
| 1 | Primary goal | Benchmark-first or show-first | **Build the benchmark core first** (it makes the show credible); build the show on top of it |
| 2 | Ranked factions | Fictional mirror nations or real nation packs | **Mirror nations for ranked play**; nation packs for exhibition matches |
| 3 | Nuclear weapons | Include as deterrence or exclude | **Include**: first use ends the match as a catastrophe for both, with an extra penalty for the first user |
| 4 | Turn length | Annual or quarterly | **Annual** in peacetime, weekly in war |
| 5 | Launch roster | 4 models or 8 | **4 for the MVP** (Claude, GPT, DeepSeek, Kimi), **6–8 for Season 1** |
| 6 | Name | See §12 | **Decide after a trademark and domain check** |
| 7 | Stack and hosting | TypeScript + Vercel + Neon, or an Azure stack | **TypeScript + Vercel + Neon** to start; Azure is fine if you prefer one Microsoft bill |
| 8 | API budget | — | **$500 for MVP testing and a $2,500 cap for Season 1** (§10), enforced by the runner and by prepaid credits |
| 9 | Licensing | Open or closed | **Open-source the engine** (Apache-2.0), publish data under CC BY 4.0, and keep held-out scenarios private |

## 16. Next steps

1. **Review this plan** and answer the open decisions in §15.
2. **Create API accounts with prepaid credits:** OpenRouter for open-weight models and quick tests; direct Anthropic and OpenAI accounts from day one; DeepSeek and Moonshot direct (or US-hosted inference) before Season 1. Prepaid balances act as hard spend caps.
3. **Check the name:** USPTO trademark search (https://tmsearch.uspto.gov) plus a domain check.
4. **Set up the dev machine (Windows, PowerShell):**

   ```powershell
   winget install --id Git.Git -e
   winget install --id OpenJS.NodeJS.LTS -e
   winget install --id Microsoft.VisualStudioCode -e
   # Close and reopen PowerShell so the new PATH loads, then:
   npm install -g pnpm
   git clone https://github.com/Seckcey/llmwars.git
   cd llmwars
   ```

   Keep API keys in a local `.env` file that is listed in `.gitignore`. Never commit keys or paste them into chats or tickets.
5. **Approve Phases 0–1.** Next build step: scaffold the monorepo, then the Arms Race engine and baseline bots, so the game can be tested with zero API spend before any model plays.
