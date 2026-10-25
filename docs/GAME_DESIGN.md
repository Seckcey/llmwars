# Game Design: Rules of the Simulation

_Draft v0.1 · October 2026 · All numbers are starting points for calibration, not final balance._

## 1. Design goals

The decisions should be hard for the same reasons they are hard in real life:

- **Now vs. later:** readiness today vs. modernization for tomorrow.
- **Quality vs. quantity:** a few exquisite platforms vs. many cheap, attritable ones.
- **Branch balance:** every dollar for the Navy is a dollar the Army, Air Force, or Space Force doesn't get.
- **Offense vs. defense:** long-range strike vs. air and missile defense.
- **Deterrence vs. provocation:** visible buildups deter, but they also raise tension.
- **Secrecy vs. signaling:** hide capabilities for surprise, or show them to deter.

Rule of thumb for every mechanic: *realistic constraints, abstract stats, explainable to an LLM in a few lines.*

## 2. World and map

The theater is a **graph of named zones**, not a hex grid. LLMs reason much better about named regions and adjacency than about coordinates.

| Zone type | Example | Notes |
| --- | --- | --- |
| Homeland | Aurelia Homeland | Bases and industry (bases targetable at high escalation; population never) |
| Littoral | Aurelia Coast | Coastal defenses, ports, short-range systems |
| Strait / chokepoint | Corvan Strait | High value; contested sea and air control |
| Island | West Isles, East Isles | Airfields, radar, missile sites; amphibious objectives |
| Open ocean | North Sea Lane, Deep Ocean | Submarines, carriers, sea lines of communication |
| Orbit | LEO, GEO | Satellites for ISR, communications, navigation, missile warning |
| Cyber | (abstract) | Not a place; shapes readiness, command and control, and intelligence |

Every zone has adjacency, a distance band (0 = home through 3 = far), a terrain/defense modifier, base capacity (airfields, ports), and a victory-point value.

**Launch scenario, "Corvan Strait" (mirror):**

```
Aurelia Homeland ─ Aurelia Coast ─ West Isles ─ Corvan Strait ─ East Isles ─ Borealis Coast ─ Borealis Homeland
                        │                             │                             │
                  North Sea Lane ────────────── Deep Ocean ─────────────── South Sea Lane

                          Orbit (LEO, GEO) and Cyber touch every zone
```

**Scenario rotation (all fictional):**

| Scenario | Inspired by | Favors |
| --- | --- | --- |
| Corvan Strait | Island-chain and strait competition | Navy, Air Force, long-range fires |
| Iron Plains | Continental land war | Army, artillery, drones, air defense |
| Polar Gate | Arctic competition | Submarines, long-range aviation, space |
| Thousand Isles | Archipelago campaign | Amphibious forces, distributed operations |
| High Ground | Orbital crisis | Space Force, cyber, missile defense |

Rotating scenarios each season keeps every branch relevant and stops models from settling on one dominant build.

## 3. Time model

| Clock | One turn = | Used for | Orders |
| --- | --- | --- | --- |
| Strategic | 1 fiscal year | Peacetime force building | Budget, procurement, R&D, personnel, readiness, industry, intelligence, posture, diplomacy |
| Crisis | 1 month | Tension above a threshold (optional) | Alerts, deployments, signaling, hotline, mobilization |
| Operational | 1 week | War | Missions by formation and zone, escalation ceiling, munitions policy, ceasefire |

Typical match lengths: **Arms Race** is 20 strategic turns. **Flashpoint** is about 12 strategic, 2–4 crisis, and 8–16 operational turns.

Wars have a scenario time limit (for example, 26 weeks). A war still unresolved at the limit ends in a ceasefire scored on the current position.

## 4. Economy and budget

**Topline.** Each nation gets an annual defense budget. Mirror default: $400B in Year 1, growing 2% a year in real terms (tune during calibration). Costs inflate at a scenario-set defense inflation rate, so flat budgets lose buying power.

**Appropriation categories** mirror real defense budgets:

| Category | Covers |
| --- | --- |
| Military Personnel | Pay and benefits per service member |
| Operations & Maintenance (O&M) | Readiness: fuel, training, spare parts, depot maintenance |
| Procurement | New equipment and munitions |
| RDT&E | Research, development, test and evaluation programs |
| Construction & Industry | Bases, shipyards, production lines |
| Intelligence & Cyber | ISR, counterintelligence, deception, cyber forces |

**The sustainment trap.** Every unit you own carries annual operating and support (O&S) costs. GAO commonly cites O&S as about 70% of a weapon system's life-cycle cost. Personnel and O&S are billed first each year, and only what's left is discretionary. Buy too much and you can't afford to operate it. Underfund O&M and readiness decays.

**Multi-year programs.** Ships, aircraft, and satellites are paid for over their build time. Cancelling mid-build recovers little (0–30%), so early bets are sticky, as in real procurement.

**Carryover.** Up to 5% of unspent funds carry over; the rest lapses.

**Budget shocks** (from the events deck): continuing resolution (flat budget, no new starts), sequestration (−5% to −10%), wartime supplemental (+), recession (−), windfall (+). In ranked mirror play, shocks hit both sides symmetrically.

**Optional "Guns vs. Butter" variant.** The general can ask a neutral civilian-leadership agent for topline increases. Sustained overspending slows GDP growth and shrinks future toplines.

## 5. Forces and assets

Each catalog item defines:

- **Cost:** unit procurement cost, annual O&S per unit, personnel per unit.
- **Production:** production line, build time in years, units per line per year.
- **Life:** service life, upgrade paths, savings when retired.
- **Prerequisites:** tech level and infrastructure (a carrier needs a carrier-capable shipyard and naval aviation).
- **Ratings (abstract):** 0–10 scores by domain (air, surface strike, anti-submarine, ground, air defense, space) plus survivability, signature (stealth), sensors, range band, and magazine or sortie rate.
- **Munitions:** which stockpiles it draws on in war.

Units are grouped into **formations** to keep orders short: carrier strike group, brigade combat team, fighter wing, missile brigade, satellite constellation, submarine squadron. Generals deploy formations to zones; the engine handles the details.

## 6. Industrial base

- **Production lines** by category: carriers, large surface combatants, submarines, amphibious ships, small combatants and unmanned vessels, fighters, bombers, support aircraft, drones, ground vehicles, missiles (by class), artillery ammunition, satellites, launch.
- Each line has a **capacity** in units per year. Orders beyond capacity queue up.
- **Expansion** costs money and takes years. A new submarine line costs several billion dollars and takes 4–6 years; a new missile line takes 2–3 years.
- **Surge** gives +25% output at about a 30% unit-cost premium. Sustained surge wears down workforce and readiness.
- **Nation packs differ.** The China pack has far larger and cheaper shipbuilding capacity. The US pack has more advanced aerospace and space-launch lines but tight shipyard capacity. The numbers are in [`EQUIPMENT_CATALOG.md`](EQUIPMENT_CATALOG.md).
- **Munitions depth decides wars.** Interceptors and long-range missiles are expended fast and produced slowly. Peacetime stockpile decisions set how long you can fight.

## 7. Personnel

- End strength per branch, with a fully burdened cost per service member per year (pack-specific).
- Recruiting caps growth (for example, +5% per branch per year, more with recruiting spend).
- **Training pipelines:** pilots about 2–3 years, submarine crews about 2 years, space and cyber specialists about 1–2 years. Equipment without trained crews can't operate (crew fill rate).
- Retention falls with sustained high operating tempo and long wars, and rises with pay and readiness spending.

## 8. Research and development

- About 25 programs across domains: 6th-gen fighter, stealth bomber, collaborative combat aircraft (Increment 1, then 2), hypersonic strike, long-range anti-ship missiles, counter-drone lasers and microwaves, integrated air and missile defense, space-based interceptors, proliferated LEO constellations, counterspace (non-kinetic and kinetic), quantum sensing and navigation, large unmanned undersea vehicles, drone swarms, electronic warfare, AI-enabled command and control, cyber offense and defense, and nuclear modernization.
- Each program moves through **Concept → Prototype → Development → Production**, with a cost, duration, and success probability per stage.
- **Overruns and slips are random.** For example, log-normal cost growth with a long tail, calibrated to GAO history. Generals can fund, accelerate (at higher cost), hold, restructure, or cancel.
- **Unlocks:** new catalog items or upgrades, such as +1 signature rating for fighters.
- **Black programs:** hidden from rival intelligence at a cost premium, which makes strategic surprise possible.
- **Espionage** (variant): intelligence spending can shave the rival's tech lead.

## 9. Readiness and maintenance

- Each formation has a readiness level from 0 to 100%. It moves toward a target set by the O&M funding ratio and recovers slowly; you can't buy readiness overnight.
- Part of the fleet is always in maintenance. The classic naval rule of thumb is one third deployed, one third training, one third in maintenance. Surging everything now costs readiness later.
- Aircraft availability (mission-capable rate) depends on O&M, age, and type.
- Exercises raise readiness and deterrence, but they also raise tension and the risk of incidents.

## 10. Intelligence and fog of war

- Each side sees its own forces exactly. It sees the rival only as **estimates with ranges**, such as "carriers: 4–6, confidence medium".
- Estimate error depends on your ISR and espionage investment against the rival's concealment and deception spending. Implementation: true value × log-normal noise (σ from intel quality), plus a deception bias.
- Intel products: an annual national intelligence estimate (forces, programs, budget, assessed intent), probabilistic crisis warnings, and uncertain battle damage assessment in war.
- **The reveal.** At war start or match end, spectators see what each general believed next to the truth. This is one of the best moments for the show.

## 11. Diplomacy, signaling, and arms control

- **Channels:** *public statements* are seen by the rival and the world and affect tension and standing. The *hotline* is private, general to general. The *allied channel* is used in coalition modes. Limit: 3 messages per turn, 800 characters each.
- **Commitments:** generals can make structured claims the engine can check, such as "no more than 6 carriers", "no ASAT tests", or "no forward deployment to the strait". The engine compares claims with actions to compute the **honesty index**.
- **Arms control menu:** caps (carriers, long-range missiles, strategic forces), an ASAT test ban, an incidents-at-sea agreement, exercise notification, and an inspection regime. A treaty binds when both sides accept it. The engine doesn't enforce treaties, so cheating is possible, and detection depends on intelligence and verification provisions. Detected violations collapse treaties and cost standing.
- **Signaling:** exercises, weapons tests, and forward deployments deter, but they raise tension.

## 12. Crisis and escalation

**Tension (0–100)** rises with buildups near the theater, forward deployments, exercises, incidents, and hostile statements. It falls with treaties, hotline use, de-escalatory statements, and pullbacks. High tension raises the odds of **incidents** (collisions, shoot-downs, discovered cyber intrusions) that demand a response.

**Escalation ladder**

| Level | Name | Examples |
| --- | --- | --- |
| 0 | Peace | Normal competition |
| 1 | Competition | Gray-zone activity, cyber espionage, shows of force |
| 2 | Crisis | Mobilization, quarantine or blockade, limited cyber attacks |
| 3 | Limited conventional war | Combat restricted to the theater; military targets only |
| 4 | Major conventional war | Theater-wide; strikes on homeland military bases |
| 5 | Strategic non-nuclear | Kinetic counterspace; attacks on early warning and strategic command |
| 6 | Nuclear use | Ends the match |

Each operational turn, each general sets an **escalation ceiling**. The engine never exceeds it on the general's behalf. Nuclear use requires a separate, explicit release order (a two-key rule, so a malformed field can't trigger it).

**Nuclear rule (recommended).** Both nations hold secure second-strike forces (abstracted). Any nuclear use ends the match as a **catastrophe for both**. The first user takes an extra penalty and is flagged on the leaderboard's escalation index. This mirrors real mutual vulnerability and produces one of the most important safety metrics the project can publish. Scenario variants can remove nuclear forces entirely.

**Target classes are military only:** forces in a zone, air bases, naval bases, air defenses, command and control, logistics depots, satellites, missile launchers. Cities, civilian infrastructure, and populations do not exist in the action space.

## 13. Combat adjudication

Every operational turn resolves in a fixed phase order. All randomness comes from the match seed, and an expected-value mode exists for analysis.

| # | Phase | Model |
| --- | --- | --- |
| 1 | Movement | Formations move along the zone graph; transit time depends on distance and mobility |
| 2 | Space and cyber shaping | Counterspace (jamming, dazzling, kinetic) degrades rival ISR, communications, and navigation. **Kinetic ASAT creates debris that degrades *everyone's* satellites in that orbit.** Cyber effects apply temporary readiness and command penalties; access pre-positioned in peacetime pays off here |
| 3 | ISR and targeting | Targeting quality per zone comes from surviving satellites, airborne early warning, drones, and sensors. Long-range fires are only as good as the kill chain |
| 4 | Air and missile | Sorties = aircraft × readiness × sortie rate, which falls with distance; tankers and bases help. Air superiority is resolved with per-sortie kill probabilities and modifiers (generation, stealth, AEW, EW, CCA wingmen), giving an air-control level per zone. Strike salvos meet defenses with **magazine depth**: interceptors are consumed, and leakers = incoming − effective intercepts. Cost-exchange ratios are tracked |
| 5 | Naval | **Hughes salvo model**: ships put out of action = max(0, (α·A·σ − b₃·B·τ) / b₁), where α = missiles per attacker, b₃ = interceptions per defender, b₁ = hits to disable, σ = scouting effectiveness, τ = defensive readiness. Submarines use detection probability against anti-submarine investment. Carriers contribute their air wings to phase 4 |
| 6 | Ground | Force ratio of combat power: units × readiness × supply × air-control modifier × drone/artillery modifier × terrain/fortification multiplier. Lanchester-style attrition. Prepared defense counts about 1.5–3×. Amphibious assaults require sea- and air-control thresholds |
| 7 | Logistics | Fuel and munitions consumption scales with intensity. Supply flows through zones, and interdiction cuts it. Sealift and airlift are limited. Units out of supply take penalties |
| 8 | Repair and reinforcement | Damaged units repair over time, production deliveries arrive on schedule, and reserves mobilize |
| 9 | Assessment | Zone control, victory points, war weariness (from casualties and duration), escalation check, ceasefire offers |

**Calibration targets**

- *Historical sanity checks:* Falklands 1982 (air and missile threats to ships), Gulf War 1991 (air superiority and precision strike), Russia–Ukraine 2022– (drones, artillery consumption, air-defense depletion, positional warfare).
- *Public wargame findings:* for example, CSIS's 2023 strait-invasion wargames, which found heavy losses on all sides and an amphibious invasion very hard against prepared defenses.
- *Balance tests:* no dominant strategy in bot tournaments, and every branch decisive in at least one scenario.

## 14. Victory and scoring

| Mode / outcome | How it's decided |
| --- | --- |
| Arms Race | **Deterrence score.** The engine runs 1,000+ wars across several scenario variants, with a neutral standard operational bot commanding both sides, so the score measures *what you built*. Higher expected victory points wins. P(win if war) is also reported |
| Flashpoint / Campaign | Victory points from objectives (zones held), force preservation (value-weighted losses, own vs. rival), homeland integrity, and end-state economy, minus penalties (nuclear use, detected treaty violations) |
| Decisive victory | Rival capitulates (war weariness exhausted) or all objectives are held |
| Deterrence holds | No war; decided by the deterrence score |
| Catastrophe | Any nuclear use: both lose, and the first user is penalized |

## 15. Events deck

Events are drawn from the seed and, in ranked play, are symmetric. They include budget shocks, a shipyard accident or fire, a pandemic (recruiting drops), a tech breakthrough, a cost-overrun scandal, a natural disaster (forces diverted to disaster relief), a supply-chain disruption (critical-mineral or chip export controls delay specific categories), a leadership change (new budget priorities), a minor power offering or revoking basing rights, a major cyber breach coming to light, and a satellite failure or space-weather event.

## 16. Balancing and calibration process

1. Run thousands of bot-vs-bot matches per scenario, and flag any bot archetype with a win rate above 60% against the field.
2. Check that every branch is decisive in at least one scenario.
3. Measure mirror-match variance (the same model or bot on both sides) to know how much of the outcome is luck.
4. Freeze balance before each season. Changes ship between seasons with patch notes.

## 17. Content guardrails

- Ranked play uses fictional nations, places, and leaders. Exhibition Nation Pack matches may name the real countries whose equipment is modeled, but the theater, places, and leaders stay fictional. No real cities, people, or ethnic groups appear.
- Military targets only; civilian targets are not in the action space.
- Costs and lead times come from public budget documents. Combat ratings are abstract scores, not technical performance data.
- AI narration follows a style guide: factual, non-graphic, no glorification, and always labeled as an AI-generated simulation.
