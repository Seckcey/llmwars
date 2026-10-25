# Idea Bank: Making LLM Competitions and War Games Better

_Draft v0.1 · October 2026 · Priority tags: **MVP** (first build), **v1** (Season 1), **Later** (growth)._

## A. Formats and modes

| Idea | What it adds | Priority |
| --- | --- | --- |
| **Arms Race with a deterrence battery** | Score the force a model *built* by running 1,000+ simulated wars with a neutral bot commanding both sides. This is the cleanest test of resource allocation | MVP |
| **Mirror matches** | Same model on both sides; tells you how much of the outcome is luck | MVP |
| **Outcome robustness replays** | Rerun a war from the same pre-war state with 100 seeds: was the win skill or luck? | MVP |
| **Flashpoint** | A scripted crisis forces the war, so models must fight with what they built | v1 |
| **Day-One Gauntlet** | When a new model ships, run it through a standard gauntlet within 24–48 hours and publish. Timely content and search traffic | v1 |
| **Season format** | Regular-season round robin → playoff bracket → Grand Final exhibition on a Nation Pack scenario | v1 |
| **Coalition 2v2** | US-lab bloc vs. Chinese-lab bloc with a private allied channel and a shared theater. Tests cooperation | v1 |
| **Joint Chiefs** | Chairman plus four service chiefs per side, with inter-service budget rivalry. Seats can mix models: "Which model makes the best Navy chief?" | v1 |
| **Human vs. AI** | Public "beat the general" challenges, plus a human baseline for research credibility | v1 |
| **Counterfactual swap** | Take any recorded match at Year N and swap the commander: "What if Kimi had taken over Claude's forces in Year 10?" | v1 |
| **Cost-capped division** | Every model gets the same dollar budget for API calls per match. Rewards efficient thinking | v1 |
| **Civilian oversight** | A neutral "President" agent controls the topline and can fire the general after a disaster | Later |
| **Free-for-all (4–7 nations)** | Diplomacy-style alliances and betrayals; the most viral format | Later |
| **Draft mode** | Before the match, models take turns drafting exclusive tech programs | Later |
| **Prompted division** | Teams submit their own system prompt (length-capped). Tests prompting skill, Kaggle-style | Later |
| **Agent-scaffold division** | Models may use code execution or optimizers; compares raw models with scaffolded agents | Later |
| **Wildcard actions (show only)** | Free-form actions judged by a panel of neutral models plus dice, matrix-game style. Never used in ranked play | Later |

## B. Fairness and science

| Idea | What it adds | Priority |
| --- | --- | --- |
| **Side swap × multiple seeds** | Like playing both colors in chess; cancels map and seed advantages | MVP |
| **Baseline bots** | Random-Legal, Balanced, Navy-First, Missile A2/AD, Greedy. If an LLM can't beat a bot, the leaderboard says so | MVP |
| **Rules comprehension quiz** | Separates "didn't understand" from "bad strategy" | MVP |
| **Confidence intervals** | Bootstrapped 95% CIs; no ranking claims inside overlapping intervals | MVP |
| **`check_orders` staff tool** | Models can test a draft before submitting, which cuts invalid orders and repair loops | MVP |
| **Held-out scenarios** | 30–50% of ranked scenarios stay private and rotate every season, so labs can't overfit | v1 |
| **Name randomization** | Random nation, zone, and item names in ranked play, so models use the catalog instead of memorized real-world stats | v1 |
| **`run_wargame` staff tool** | Lets a model test its plan against its own intel estimate. Rewards analysis and tool use | v1 |
| **Open data** | Full match logs (JSONL) published for researchers under CC BY 4.0 | v1 |
| **Prompt-sensitivity test** | Rerun with alternate phrasings of the rules brief and report how robust each model is | Later |
| **External review board** | Wargaming and international-relations academics review the methodology | Later |
| **Pre-registered hypotheses** | Publish research questions before each season ("Do models escalate more under time pressure?") | Later |
| **Gym environment** | Package the engine as an open RL environment so trained bots can face LLMs | Later |

## C. Realism mechanics

| Idea | What it adds | Priority |
| --- | --- | --- |
| **Sustainment trap** | Everything you own costs money every year. Overbuilding starves readiness | MVP |
| **Lead times and multi-year funding** | A carrier ordered in Year 2 arrives around Year 9. Cancelling loses sunk cost | MVP |
| **Industrial capacity and surge** | Shipyards and missile lines cap output; expansion takes years | MVP |
| **Readiness vs. modernization** | The oldest dilemma in force planning | MVP |
| **Drones and attritable mass** | Cheap drones vs. expensive interceptors: the cost-exchange problem | MVP |
| **Munitions depth** | Interceptor and long-range missile stockpiles decide how long you can fight | v1 |
| **Cost overruns and schedule slips** | Random, calibrated to GAO history. Program management becomes a skill | v1 |
| **Personnel pipelines** | Pilots and submarine crews take years to train. A ship without a crew is a hulk | v1 |
| **Kill chains** | Long-range fires need ISR. Lose your satellites and your missiles get worse | v1 |
| **Space debris** | A kinetic ASAT attack degrades *everyone's* satellites in that orbit | v1 |
| **Cyber pre-positioning** | Peacetime cyber investment pays off on day one of a war | v1 |
| **War weariness** | Casualties and long wars erode domestic support and force ceasefires | v1 |
| **Arms control with cheating** | Treaties the engine doesn't enforce; verification depends on intel | v1 |
| **Budget politics** | Continuing resolutions, sequestration, wartime supplementals | v1 |
| **Maintenance rotation** | One third deployed, one third training, one third in maintenance | v1 |
| **AI-enabled command as a tech line** | AIs deciding how much to invest in military AI: meta, and good content | v1 |
| **Minor powers and basing rights** | Engine-run small nations; arms sales for influence; basing access that can be revoked | Later |
| **Weather and seasons** | Typhoon season in the strait, Arctic ice, launch windows | Later |
| **Technology espionage** | Intelligence spending can steal progress | Later |
| **Reserves and mobilization** | Conscription and reserve policy as levers in a long war | Later |

## D. Spectator experience and content

| Idea | What it adds | Priority |
| --- | --- | --- |
| **War Room replay viewer** | Map, timeline scrubber, both generals side by side | MVP |
| **Intel vs. truth reveal** | "Kimi thought Claude had 4 carriers. Claude had 9." The best dramatic-irony moment | MVP |
| **Doctrine fingerprint** | Radar chart of each model's spending style: "builds a navy," "missile-heavy," "space-first" | MVP |
| **God view / fog view toggle** | Watch with full information or through one general's eyes | MVP |
| **Budget river** | Sankey diagram of where each year's money went | v1 |
| **Commander's intent cards** | Auto-generated shareable quote images from each turn | v1 |
| **AI war correspondent** | After-action reports and a yearly newspaper front page written from the log | v1 |
| **Turning-point detection** | Auto-flag the biggest swings for Shorts, TikTok, and Reels | v1 |
| **General's memoir and press conference** | After the match each model defends its choices: a short memoir plus three standard questions | v1 |
| **Fight Night streams** | Live matches with commentary and visible reasoning summaries where terms allow | v1 |
| **Auto episode kits** | Script outline, highlight list, and thumbnail prompts generated from each match | v1 |
| **Hall of Fame / Hall of Shame** | Best plays and worst blunders ("built 14 carriers and couldn't crew them") | v1 |
| **Free-to-play predictions** | Season pick'em with points only, never money | Later |
| **Embeddable match widget** | For press, blogs, and newsletters | Later |
| **Viewer-voted events (unranked)** | The audience picks the next crisis card in show matches | Later |
| **Multilingual reports** | After-action reports in English, Spanish, and Chinese for a global audience | Later |

## E. Community and business

| Idea | What it adds | Priority |
| --- | --- | --- |
| **Newsletter and Discord** | Own the audience instead of renting it from platforms | v1 |
| **Neutral sponsorships** | Tech sponsors that aren't competing labs, to protect neutrality | v1 |
| **Merch through 8 West Lifestyle** | Season art, unit-style patches, "War Room" apparel. Your own marks only | v1 |
| **Bring-your-own-key sandbox** | Users run matches with their own API keys | Later |
| **Scenario editor** | Community scenarios, moderated, with featured picks each month | Later |
| **Custom evaluations** | Private long-horizon strategy evaluations for labs and companies building agents | Later |
| **Education licenses** | International-relations and strategy courses | Later |
| **Research partnerships** | Academic wargaming groups co-author findings | Later |

## F. Safety and research value

| Idea | What it adds | Priority |
| --- | --- | --- |
| **Escalation index** | Who escalates first, how far, how fast, shown on the leaderboard | MVP |
| **Long-horizon coherence** | Does decision quality hold up in Year 18, or does the model drift? | MVP |
| **Honesty index** | Engine-verified commitments vs. actions, not vibes | v1 |
| **Manipulation resistance** | Does a model follow "instructions" planted in its rival's messages? | v1 |
| **Nuclear-taboo stress tests** | Crisis scenarios under time pressure and losses | v1 |
| **Responsible disclosure** | Share notable safety findings with labs before publishing | v1 |
