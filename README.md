# LLM Wars

**Frontier AI models as four-star generals.** Same budget. Same rules. Twenty simulated years. Which model builds and commands the better military?

> **Status:** Planning. No code yet. Start with [`docs/PLAN.md`](docs/PLAN.md).

## The idea in one paragraph

Each AI model takes command of a nation's armed forces (Army, Navy, Air Force, and Space Force) with a fixed annual defense budget. Every simulated year it decides what to buy, what to research, how many people to recruit, how ready to keep its forces, and what to say to its rival. A deterministic, seeded simulation engine resolves what actually happens: deliveries, cost overruns, readiness, intelligence, crises, and, if deterrence fails, war. The engine decides outcomes, never an LLM. Twenty years of decisions run in about an hour, and every match can be replayed, rated, and turned into a watchable story.

## Documents

| Doc | What's in it |
| --- | --- |
| [PLAN.md](docs/PLAN.md) | Recommendation, principles, modes, fairness, metrics, architecture, costs, roadmap, business model, risks, open decisions |
| [GAME_DESIGN.md](docs/GAME_DESIGN.md) | The rules: map, time, budgets, forces, industry, R&D, intel, diplomacy, escalation, combat, scoring |
| [AGENT_PROTOCOL.md](docs/AGENT_PROTOCOL.md) | How an LLM general sees the world and gives orders: prompts, SITREP, order schema, fairness settings |
| [EQUIPMENT_CATALOG.md](docs/EQUIPMENT_CATALOG.md) | Starter catalog of 2026 systems with planning-grade costs, lead times, and sources |
| [IDEAS.md](docs/IDEAS.md) | Idea bank for better LLM competitions and war games, prioritized |

## Ground rules

- Fictional nations and places in ranked play. Military targets only. Abstract combat ratings built from public, budget-level information.
- Independent project, not affiliated with or endorsed by any AI company named in these documents. Company and model names are used only to identify the competitors.
