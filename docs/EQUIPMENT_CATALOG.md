# Equipment Catalog (Starter, 2026)

_Draft v0.1 · October 2026 · **Planning-grade numbers.** Verify each figure against the cited source before it goes into `packages/data`._

## 1. How the catalog works

- **Three catalogs:** a *generic* catalog for ranked mirror play (classes anchored to real systems, priced at blended estimates), a **US pack**, and a **China pack** for exhibition matches.
- **What we model:** public, budget-level facts: unit procurement cost, annual operating and support (O&S) cost, build time, production rate, crew. **What we don't:** technical performance data. Combat effectiveness is an abstract 0–10 rating set during balancing.
- **Cost basis:** US figures are then-year or base-year dollars as stated by the source. Chinese figures are **estimates** (China doesn't publish unit costs) and are shown as ranges.
- **Cost types differ.** *Flyaway* or *sail-away* cost is the platform alone. *APUC* (average procurement unit cost) adds support and initial spares. *PAUC* (program acquisition unit cost) also spreads development cost over every unit. The game uses APUC-like figures for procurement and puts development cost in R&D programs.
- **Confidence:** H = official or multiple consistent sources; M = one good source or a reasonable derivation; L = rough estimate.
- **Versioning:** each catalog is versioned (`catalog-2026.1`) and frozen per season.

## 2. Item schema (planned YAML)

```yaml
# packages/data/catalogs/generic-2026.1.yaml (planned)
- id: gen.air.fighter_5g
  name: 5th-gen multirole fighter
  anchors: [F-35A, J-35A]            # real systems this class is modeled on
  branch: air_force
  roles: [air_superiority, strike, sead]
  cost:
    unit_musd: 0                     # procurement, base-year dollars (fill from §5)
    annual_os_musd: 0                # O&S per unit per year
    personnel: 0                     # crew + maintainers, abstracted
  production:
    line: fighter
    build_years: 2
    max_per_line_year: 0
  service_life_years: 30
  ratings:                           # abstract 0–10, set in balancing
    air: 8
    strike: 7
    signature: 8                     # higher = harder to detect
    sensors: 8
    survivability: 6
    range_band: medium
  munitions: [aam_medium, standoff_strike]
  requires: [tech.stealth_5g]
  sources: []
  confidence: M
```

> **How these figures were gathered.** Compiled in October 2026 from search-result excerpts of the cited pages; full page fetching wasn't available. "Line ÷ qty" means a budget line divided by its quantity, which includes support costs and is higher than flyaway. Verify each figure at the source before locking a season catalog.

## 3. Real-world budget baselines

These numbers calibrate toplines, the *Balanced* bot's spending split, and the exhibition "real budgets" option. (The US Defense Department has also used the "Department of War" name since 2025; budget documents now carry both.)

| Measure | Figure | Conf | Source |
| --- | --- | --- | --- |
| US DoD appropriations, FY2026 (enacted 3 Feb 2026) | $839.2B, plus about $150B of one-time defense reconciliation funding (July 2025), mostly obligated in FY2026 | H | [CRS R48891](https://www.congress.gov/crs-product/R48891), [Breaking Defense](https://breakingdefense.com/2026/02/reconciliation-revealed-how-the-pentagon-plans-to-spend-all-152-billion-in-fy26/) |
| US request, FY2027 | About $1.45T for DoD: ≈$1.10T base plus $350B mandatory that needs a second reconciliation bill (not enacted as of October 2026) | M | [FY2027 Overview Book](https://comptroller.war.gov/Portals/45/Documents/defbudget/FY2027/FY2027_Budget_Request_Overview_Book.pdf), [Breaking Defense](https://breakingdefense.com/2026/04/golden-dome-out-years-and-lots-of-missiles-details-of-trumps-1-5t-defense-budget-request/) |
| China official budget, 2026 | ¥1.91T ≈ $277B, +7.0% | H | [China Daily](https://www.chinadaily.com.cn/a/202603/05/WS69a8eacea310d6866eb3bd7f.html) |
| China, SIPRI estimate for 2025 | $336B (US: $954B) | H | [SIPRI](https://www.sipri.org/media/press-release/2026/global-military-spending-rise-continues-european-and-asian-expenditures-surge) |
| China, PPP + off-budget estimate for 2024 | ≈$471B | M | [TNSR (Fravel)](https://tnsr.org/2024/06/estimating-chinas-defense-spending-how-to-get-it-wrong-and-right/) |

**US split by appropriation title** (share of total)

| Title | FY2026 enacted | FY2027 base request | Default for the *Balanced* bot |
| --- | --- | --- | --- |
| Military Personnel | 23% | 19% | 20% |
| Operations & Maintenance | 35% (≈40% including Defense Health) | 35% | 38% |
| Procurement | 20% | 23% | 22% |
| RDT&E | 17% | 20% | 18% |
| Construction | ≈2% | 3% | 2% |

Sources: [CRS R49023](https://www.congress.gov/crs-product/R49023) and the FY2027 Overview Book. The shares are research arithmetic (confidence M).

**Game implications**

- **Mirror play:** the topline is a tunable design number ($400B in Year 1 to start). Both sides share one catalog, so only relative costs matter.
- **Exhibition "real budgets" option:** the US pack gets about $1T a year and the China pack about $280B official, or $340–470B effective by outside estimates. The China pack's cheaper ships and larger shipyards close part of that gap. The default exhibition setting is purchasing-power parity, tuned in calibration.

## 4. 2025–2026 developments to model

| When | Development | Game hook | Source |
| --- | --- | --- | --- |
| Mar 2025 | F-47 6th-gen fighter awarded to Boeing; first flight planned for 2028. Press estimates ≈$300M per jet (the official figure is classified) | 6th-gen fighter R&D program | [Zona Militar](https://www.zona-militar.com/en/2026/02/27/the-u-s-air-force-and-boeing-are-making-steady-progress-toward-beginning-flight-testing-of-the-new-sixth-generation-f-47-fighter-jet-in-2028/), [19FortyFive](https://www.19fortyfive.com/2026/04/300-million-per-fighter-the-f-47-ngad-price-tag-could-kill-it-before-it-flies/) |
| Sep 2026 | Navy F/A-XX awarded to Boeing (>$20B development) | Naval 6th-gen program | [Defense News](https://www.defensenews.com/industry/techwatch/2026/09/29/us-navy-selects-boeing-to-build-next-generation-fa-xx-fighter/) |
| May–Jun 2025 | Army Transformation Initiative: M10 Booker, AH-64D, and Gray Eagle ended; HMMWV/JLTV buys halted or slowed | "Divest to invest" choices | [GlobalSecurity](https://www.globalsecurity.org/military/library/news/2025/06/mil-250611-army01.htm) |
| Nov 2025–Mar 2026 | Army goal of ≥1M drones in 2–3 years; the first "Drone Dominance" buy was ≈30,000 attack drones for ≈$150M (≈$5K each) | Cheap mass vs. exquisite platforms | [Defense One](https://www.defenseone.com/threats/2025/11/the-d-brief-november-10-2025/409438/), [DefenseScoop](https://defensescoop.com/2026/03/09/dod-drone-dominance-program-gauntlet-i-results/) |
| Nov–Dec 2025 | Constellation frigate cancelled after two hulls; FF(X) based on the National Security Cutter hull | Program cancellation and restart | [USNI News](https://news.usni.org/2026/04/28/navy-awards-282-9m-ffx-frigate-contract-to-hiis-ingalls-shipbuilding) |
| Dec 2025–Aug 2026 | "Golden Fleet" battleship BBG(X): CBO puts the lead ship at $23.4B and 15 ships at ≈$275B | Prestige megaproject | [Stars and Stripes](https://www.stripes.com/branches/navy/2026-08-05/trump-golden-fleet-455-billion-estimate-22466336.html) |
| Mar 2026 | MASC unmanned surface vessel program cancelled and replaced by a MUSV marketplace | Unmanned surface vessels as an uncertain bet | [Breaking Defense](https://breakingdefense.com/2026/03/navy-nixes-masc-program-unveils-new-musv-marketplace-amid-golden-fleet-push/) |
| Feb–Oct 2026 | B-21: capacity +25% and a second assembly line; first operational aircraft in 2027 | Buying production capacity | [AF.mil](https://www.af.mil/News/Article-Display/Article/4412198/daf-increases-b-21-raider-production-capacity-to-deliver-combat-capability-fast/) |
| Jun–Jul 2026 | CCA Increment 1 production decision (Anduril, General Atomics; target <$30M each); Increment 2 in concept refinement | Attritable wingmen; Increment 2 unlock | [DefenseScoop](https://defensescoop.com/2026/06/17/air-force-picks-anduril-general-atomics-to-build-first-operational-cca-drones/), [DefenseScoop](https://defensescoop.com/2026/07/30/air-force-cca-drone-program-increment-2-downselect/) |
| 2025–2026 | E-7 Wedgetail cut in two budget requests, then restored by Congress and a ≈$1.55B amendment | Budget politics | [Air & Space Forces](https://www.airandspaceforces.com/pentagon-finds-1-55b-for-e-7-wedgetail-mostly-in-classifed-budget/) |
| 2024–2026 | Sentinel ICBM restructured; program ≈$141B (+81% vs. the original estimate) | Overrun calibration case | [Breaking Defense](https://breakingdefense.com/2026/02/exclusive-sentinel-icbm-to-clear-key-review-this-year-go-operational-in-early-2030s/) |
| 2025–2026 | Golden Dome: DoD estimate $185B through 2035; CBO estimates $1.2T over 20 years for a notional 7,800-interceptor architecture; space-based interceptor prototype agreements up to $3.2B | Big-bet R&D with huge cost uncertainty | [SpaceNews](https://spacenews.com/golden-dome-cost-estimate-rises-to-185-billion-as-pentagon-expands-space-layer/), [CBO](https://www.cbo.gov/system/files/2026-05/62379-golden-dome.pdf) |
| Jul 2026 | $76.6B submarine awards: 9 Virginia Block VI, 5 Columbia, $5B for the industrial base | Multi-year buys; industrial investment | [USNI News](https://news.usni.org/2026/07/29/navy-awards-76-6b-for-9-virginia-5-columbia-subs-to-general-dynamics-hii) |
| Jan–Feb 2026 | Munitions framework deals: PAC-3 MSE to 2,000/yr, THAAD to 400/yr, SM-6 >500/yr, Tomahawk >1,000/yr, PrSM to 550/yr | Line expansion takes years (tripled PAC-3 output not before 2028) | [Lockheed Martin](https://news.lockheedmartin.com/2026-01-06-Lockheed-Martin-and-Department-of-War-Advance-Landmark-Acquisition-Transformation-to-Accelerate-PAC-3-R-MSE-Production), [RTX](https://finance.yahoo.com/news/rtxs-raytheon-partners-department-war-113500618.html) |
| Sep 2026 | CBO: one-half to two-thirds of US Patriot, THAAD, SM-3, and SM-6 inventories used since June 2025 in the Iran conflicts, with ≥5 years to rebuild | Calibration target for munitions depth | [CBO](https://www.cbo.gov/system/files/2026-09/62756-Iran.pdf) |
| Sep 2025 | China's parade showed a full nuclear triad, the YJ-21 hypersonic anti-ship missile, the LY-1 laser, extra-large UUVs, and the GJ-11 | China pack items | [Breaking Defense](https://breakingdefense.com/2025/09/china-parade-nuclear-weapons-xi-putin-kim-military/), [The Diplomat](https://thediplomat.com/2025/09/what-were-the-weapons-on-display-in-chinas-military-parade/) |
| Nov 2025 | Fujian (Type 003), China's first electromagnetic-catapult carrier, commissioned | China pack carrier line | [Naval News](https://www.navalnews.com/naval-news/2025/11/chinese-navy-takes-aircraft-carrier-fujian-into-active-service-in-hainan/) |
| 2024–2026 | Type 076 Sichuan (drone catapult) in final trials; commissioning expected by end of 2026 | Drone-carrier class | [SCMP](https://www.scmp.com/news/china/military/article/3362271/chinas-type-076-amphibious-assault-ship-sichuan-final-tests-drone-catapult) |
| 2026 | Type 004 (probably nuclear-powered) under construction; launch ≈2032 (analyst estimate) | Future unlock | [CSIS](https://features.csis.org/hiddenreach/china-fourth-carrier/) |
| 2024–2026 | J-36 and J-50 6th-gen prototypes flying since December 2024; operational ≈2035 (low confidence) | 6th-gen race | [19FortyFive](https://www.19fortyfive.com/2026/06/what-are-chinas-j-36-and-j-50-sixth-generation-fighters-and-how-far-ahead-are-they-of-americas-f-47-ngad/) |
| 2025–2026 | J-20 output ≈120/yr (RUSI estimate) and a J-20/J-35 capacity surge | China pack aircraft-line capacity | [FlightGlobal](https://www.flightglobal.com/fixed-wing/2026/03/china-surges-production-capacity-for-j-20-and-j-35-stealth-fighters/) |
| Dec 2025 | DoD China report: warheads in the low 600s, >1,000 by 2030 | Strategic-force growth | [FAS](https://fas.org/publication/the-pentagons-slimmed-down-2025-china-military-power-report/) |

## 5. US systems (US pack; anchors for the generic catalog)

| Class | System | Status | Planning unit cost (USD) | Build / lead time | Conf | Source |
| --- | --- | --- | --- | --- | --- | --- |
| **Ground** | | | | | | |
| Main battle tank | M1A2 SEPv3 → M1E3 | SEPv3 upgrades; M1E3 in development (production FY28) | ≈$24–30M (export reference / FY27 upgrade line) | M1E3 prototypes 24–30 months | L | [Breaking Defense](https://breakingdefense.com/2026/06/house-appropriators-release-1-trillion-defense-bill-for-fy27/), [Army Recognition](https://www.armyrecognition.com/news/army-news/2026/u-s-army-accelerates-m1e3-abrams-tank-with-474m-funding-ahead-of-2028-production) |
| Armored carrier / IFV | AMPV / XM30 | AMPV in production; XM30 in development | AMPV ≈$5.9M; XM30 n/a (≈$45B program) | XM30 low-rate production FY28 | M | [Army P-1](https://www.asafm.army.mil/Portals/72/Documents/BudgetMaterial/2027/Discretionary%20Budget/Procurement/Procurement_of_Weapons_and_Tracked_Combat_Vehicles.pdf) |
| Rocket artillery | HIMARS | In production | ≈$7.8M (FY27 line ÷ qty) | n/a | M | [Inside Defense](https://insidedefense.com/insider/army-and-lockheed-shake-undefinitized-11-billion-himars-deal) |
| Long-range missile | PrSM Increment 1 | In production | ≈$1.7M | ≈400/yr, rising to 550/yr | M | [DefenseScoop](https://defensescoop.com/2026/04/07/army-procurement-precision-strike-missile-prsm-2027/) |
| Area air and missile defense | Patriot battery + LTAMDS | In production | ≈$1.1B including missiles (≈$400M hardware) | n/a | L | [CRS IF13299](https://www.congress.gov/crs-product/IF13299) |
| Short-range air defense | M-SHORAD Increment 1 | In production | ≈$51M per system (FY27 line ÷ qty, includes integration) | n/a | L | [ExecutiveBiz](https://www.executivebiz.com/articles/army-m-shorad-air-defense-cuas-missile-fy27) |
| Hypersonic strike | Dark Eagle (LRHW) | Fielding | Missile ≥$40M; a battery is 4 launchers + 8 missiles | n/a | M | [CRS IF11991](https://www.congress.gov/crs-product/IF11991), [CBO](https://www.cbo.gov/publication/58924) |
| Ground-launched SM-6 / Tomahawk | Typhon (MRC) | In production | n/a per battery (FY27 procurement line ≈$2.7B) | 3 more batteries FY26–28 | L | [CRS IF12135](https://www.congress.gov/crs-product/IF12135) |
| Attack helicopter | AH-64E | In production | ≈$34M (APUC) | n/a | L | [GSD News](https://gsdnews.us/army-budget-request-appears-to-forego-black-hawk-apache-procurement-in-fy-27/) |
| Utility helicopter | UH-60M | In production | ≈$20–32M | n/a | L | [Military Machine](https://militarymachine.com/black-hawk-helicopter-cost) |
| Tiltrotor assault | MV-75 (FLRAA) | In development | n/a | First unit FY30 | M | [Breaking Defense](https://breakingdefense.com/2026/01/army-says-first-mv-75-tilt-rotor-helo-to-be-delivered-by-end-2026-for-testing/) |
| **Naval** | | | | | | |
| Supercarrier | Ford class | CVN-78 in service; CVN-79 to 81 building | $13.2B / $14.2B / $15.2B (CVN-79/80/81, then-year); CBO ≈$22B for later hulls | 10–11 years per ship (CBO) | M | [CRS RS20643](https://www.congress.gov/crs-product/RS20643), [CBO](https://www.cbo.gov/publication/62375) |
| Attack submarine | Virginia Block V/VI | In production | ≈$4.7–5.0B | Output ≈1.1–1.3/yr vs. a 2/yr goal | M | [CRS RL32418](https://www.everycrsreport.com/reports/RL32418.html), [USNI News](https://news.usni.org/2026/07/29/navy-awards-76-6b-for-9-virginia-5-columbia-subs-to-general-dynamics-hii) |
| Ballistic missile submarine | Columbia | In production | Class APUC ≈$10.4B; lead boat ≈$15–16B including design | Lead delivery ≈2029 (≥18 months late) | M | [CRS R41129](https://www.congress.gov/crs-product/R41129) |
| Large surface combatant | DDG-51 Flight III | In production | ≈$2.7B | Deliveries 22–58 months late (GAO) | M | [CRS RL32109](https://www.everycrsreport.com/reports/RL32109.html) |
| Frigate | FF(X) | Development → production | Lead ship ≈$1.7B including long-lead items | Delivery ≈2030 | M | [USNI News](https://news.usni.org/2026/04/28/navy-awards-282-9m-ffx-frigate-contract-to-hiis-ingalls-shipbuilding) |
| Battleship | BBG(X) | In development | Lead $23.4B; follow-on ≈$18B (CBO) | n/a | M | [Stars and Stripes](https://www.stripes.com/branches/navy/2026-08-05/trump-golden-fleet-455-billion-estimate-22466336.html) |
| Amphibious assault ship | America-class LHA | In production | $3.9B (Navy) to $4.8B (CBO) | n/a | M | [CRS R43543](https://www.congress.gov/crs-product/R43543) |
| Amphibious transport dock | San Antonio Flight II LPD | In production | ≈$2.1B | n/a | M | [CRS R43543](https://www.congress.gov/crs-product/R43543) |
| Carrier drone tanker | MQ-25A | Low-rate production | ≈$190–255M | Initial capability 2029 | M | [CRS IF12972](https://www.congress.gov/crs-product/IF12972) |
| Unmanned surface vessel | MUSV | In development (MASC cancelled) | n/a | Vessels FY27 | L | [Breaking Defense](https://breakingdefense.com/2026/03/navy-nixes-masc-program-unveils-new-musv-marketplace-amid-golden-fleet-push/) |
| **Air** | | | | | | |
| 5th-gen multirole fighter | F-35A/B/C | In production | ≈$82M average airframe (Lots 18–19, excluding engine); flyaway ≈$92M (A) to ≈$121M (B), low confidence | 191 delivered in 2025 | H/M | [Breaking Defense](https://breakingdefense.com/2025/09/lockheed-pentagon-finalize-deal-for-296-f-35s/) |
| 4.5-gen fighter | F-15EX | In production | ≈$111–125M | n/a | M | [The War Zone](https://www.twz.com/air/f-15ex-buy-was-just-doubled-by-the-usaf-which-makes-perfect-sense) |
| 6th-gen fighter | F-47 / F/A-XX | In development | ≈$300M (press estimate) | First flight 2028 | L | [19FortyFive](https://www.19fortyfive.com/2026/04/300-million-per-fighter-the-f-47-ngad-price-tag-could-kill-it-before-it-flies/) |
| Stealth bomber | B-21 | Low-rate production | ≈$700–750M | First operational aircraft 2027 | M | [Air & Space Forces](https://www.airandspaceforces.com/producing-new-b-21-bomber-will-cost-20-billion-through-2027/) |
| Collaborative combat aircraft | CCA Increment 1 (FQ-42A, FQ-44A) | In production | <$30M target | First production airframe July 2026 | M | [DefenseScoop](https://defensescoop.com/2026/06/17/air-force-picks-anduril-general-atomics-to-build-first-operational-cca-drones/) |
| Tanker | KC-46A | In production | ≈$165M | n/a | M | [The Aviationist](https://theaviationist.com/2025/11/30/kc-46-lot-12-contract/) |
| Airborne early warning | E-7 / E-2D | E-7 prototypes; E-2D in production | E-7 ≈$724M; E-2D ≈$367M | E-7 production from FY28 | M/L | [Defense Daily](https://www.defensedaily.com/dod-cites-e-7-wedgetail-unit-cost-increase-from-588-million-to-724-million/air-force/), [Air & Space Forces](https://www.airandspaceforces.com/pentagon-finds-1-55b-for-e-7-wedgetail-mostly-in-classifed-budget/) |
| Tactical airlift | C-130J | In production | ≈$111M | n/a | L | [Military Machine](https://militarymachine.com/c-130-cost) |
| Medium-altitude drone | MQ-9A | In service (production ended) | $30–50M; successor target ≈$10M | n/a | M | [Military Times](https://www.militarytimes.com/news/pentagon-congress/2026/09/28/heavy-losses-of-mq-9-reaper-to-factor-in-future-plans-to-divest-replace-aircraft/) |
| **Space** | | | | | | |
| Navigation satellite | GPS IIIF | In production | ≈$255M | n/a | M | [SpaceNews](https://spacenews.com/space-force-orders-two-more-gps-iiif-satellites-for-509-7-million/) |
| Missile warning (GEO/polar) | Next-Gen OPIR | Development / production | n/a per satellite (program ≈$14B) | First launch no earlier than October 2026 | L | [Breaking Defense](https://breakingdefense.com/2025/06/first-next-gen-opir-missile-warning-launch-pushed-to-2026/) |
| Missile tracking (proliferated LEO) | SDA Tranche 3 tracking | In production | ≈$49M average | Tranche 3 no earlier than 2029 | M | [Spaceflight Now](https://spaceflightnow.com/2025/12/20/space-development-agency-awards-roughly-3-5-billion-to-4-companies-for-72-missile-tracking-and-warning-satellites/) |
| Medium / heavy launch | Falcon 9 / Falcon Heavy | In production | List $74M / $97M; national-security missions ≈$121–143M average | n/a | M | [Wikipedia](https://en.wikipedia.org/wiki/Falcon_9), [SpaceNews](https://spacenews.com/spacex-lands-majority-of-u-s-national-security-launches-awarded-for-fiscal-year-2026/) |
| Heavy launch | Vulcan | In production | ≈$214M average (FY26 national-security missions) | n/a | M | [Space Systems Command](https://www.ssc.spaceforce.mil/Newsroom/Article/4348694/space-systems-command-releases-national-security-space-launch-phase-3-lane-2-fy) |
| Space-based interceptor | Golden Dome SBI | In development | ≈$22M average (CBO notional); published estimates range from $1.3M to >$110M | Demo 2028 | L | [CBO](https://www.cbo.gov/system/files/2026-05/62379-golden-dome.pdf), [FPRI](https://www.fpri.org/article/2026/09/nothing-new-under-the-golden-dome-space-based-interceptors-in-the-trump-administration/) |
| **Strategic** | | | | | | |
| ICBM | Sentinel (LGM-35A) | In development (restructured) | ≈$214M per missile including program share (2020 dollars, low confidence); program ≈$141B | Initial capability early 2030s | M/L | [Breaking Defense](https://breakingdefense.com/2026/02/exclusive-sentinel-icbm-to-clear-key-review-this-year-go-operational-in-early-2030s/) |
| Nuclear cruise missile | LRSO | In development | ≈$13–14M (older estimate) | Production decision February 2027 | L | [Air & Space Forces](https://www.airandspaceforces.com/lrso-production-decision-2027/) |
| **Munitions and interceptors** | | | | | | |
| Area SAM interceptor | PAC-3 MSE | In production | ≈$4M | 620 delivered in 2025; target 2,000/yr | M | [CBO](https://www.cbo.gov/system/files/2026-09/62756-Iran.pdf) |
| Naval SAM | SM-6 | In production | $4.3M (APUC) | 36–39 months lead time | H | [SM-6 SAR](https://www.esd.whs.mil/Portals/54/Documents/FOID/Reading%20Room/Selected_Acquisition_Reports/PB_2027_MSARs/SM-6_MSAR_FY2027_PBv3.pdf) |
| Exo-atmospheric BMD interceptor | SM-3 Block IIA | In production | ≈$28M | ≈39 months lead time | M | [CBO](https://www.cbo.gov/system/files/2026-09/62756-Iran.pdf) |
| Terminal BMD interceptor | THAAD | In production | ≈$12M | 96/yr, rising to 400/yr | M | [CBO](https://www.cbo.gov/system/files/2026-09/62756-Iran.pdf) |
| Land-attack cruise missile | Tomahawk | In production | ≈$2–3.8M (sources conflict) | 34 months lead time | L | [Defence Blog](https://defence-blog.com/u-s-navy-requests-3-billion-for-785-tomahawk-missiles/) |
| Stealthy air-launched cruise missile | JASSM-ER | In production | ≈$1.7M (FY24 unit) to ≈$3.1M (FY27 line ÷ qty) | n/a | L | [Wikipedia](https://en.wikipedia.org/wiki/AGM-158_JASSM) |
| Long-range anti-ship missile | LRASM | In production | ≈$3.2M (FY24) to ≈$5.3M (FY27 line ÷ qty) | n/a | L | [Air Force FY27 procurement book](https://www.af.mil/Portals/1/documents/Secretariat%20of%20the%20AF/SAF-FM/Budget%20-%202027/Budget%20docs/FY27%20Air%20Force%20Aircraft%20%20Missile%20Procurement.pdf) |
| Guided rocket | GMLRS | In production | ≈$219K | 14,000/yr | M | [Defense Daily](https://www.defensedaily.com/lockheed-has-reached-production-goal-to-build-14000-gmlrs-annually/army/) |
| Artillery shell | 155mm HE | In production | ≈$3K (guided Excalibur ≈$113K) | 36,000/month (March 2026) | L | [Military Times](https://www.militarytimes.com/news/your-military/2026/07/14/manufacturing-woes-hamper-us-155-mm-ammo-production/) |
| Medium-range air-to-air missile | AIM-120D | In production | n/a | Target ≥1,900/yr | n/a | [RTX](https://finance.yahoo.com/news/rtxs-raytheon-partners-department-war-113500618.html) |
| **Drones and unmanned** | | | | | | |
| Loitering munition | Switchblade 600 | In production | n/a per munition | n/a | L | [The Defense Post](https://thedefensepost.com/2026/08/27/us-aerovironment-switchblade-600-order/) |
| Counter-drone interceptor | Coyote Block 2 | In production | ≈$100–125K | n/a | L/M | [The War Zone](https://www.twz.com/drastic-increase-in-army-coyote-drone-interceptor-purchase-plans) |
| One-way attack drone | LUCAS | In production | ≈$35K | n/a | M | [The War Zone](https://www.twz.com/news-features/cost-to-link-lucas-kamikaze-drones-to-starlink-highlights-pentagons-ever-growing-dependence-on-spacex) |
| Small attack / FPV drone | Drone Dominance buys | In production | ≈$5K average (goal ≈$2K) | n/a | M | [DefenseScoop](https://defensescoop.com/2026/03/09/dod-drone-dominance-program-gauntlet-i-results/) |

## 6. China systems (China pack)

China doesn't publish unit costs. Every figure below is an outside estimate at market exchange rates or an export-price proxy. "n/a" means nothing credible was found yet.

| Class | System | Status | Estimated unit cost (USD) | Notes | Conf | Source |
| --- | --- | --- | --- | --- | --- | --- |
| **Ground** | | | | | | |
| Main battle tank | ZTZ-99A / Type 15 | In service | ≈$5M (VT-4 export proxy) | Export proxy only | L | [Janes](https://www.janes.com/defence-intelligence-insights/defence-news/thailand-looks-to-procure-more-vt-4-tanks-from-china) |
| Infantry fighting vehicle | ZBD-04A | In service | n/a | | n/a | |
| Rocket artillery | PCH-191 / PHL-16 | In service | n/a | | n/a | |
| Long-range SAM | HQ-9B/C | In service | n/a | Exported to several countries | L | [Militarnyi](https://militarnyi.com/en/news/egypt-to-purchase-chinese-hq-9b-air-defense-system/) |
| Utility helicopter | Z-20 | In service | n/a | | n/a | |
| **Naval** | | | | | | |
| Carrier (catapult) | Type 003 Fujian | In service (Nov 2025) | ≈$7B (Indian press estimate) | 3.4 years from launch to commissioning | L | [Naval News](https://www.navalnews.com/naval-news/2025/11/chinese-navy-takes-aircraft-carrier-fujian-into-active-service-in-hainan/) |
| Carrier (probably nuclear) | Type 004 | Building | n/a | Launch ≈2032 (analyst estimate) | L | [CSIS](https://features.csis.org/hiddenreach/china-fourth-carrier/) |
| Large destroyer | Type 055 | In service | ≈$0.9B (¥6B including R&D, 2017) | Second batch of 6 building | L/M | [Wikipedia](https://en.wikipedia.org/wiki/Type_055_destroyer) |
| Destroyer | Type 052D | In service | $600–900M (aggregator; likely high) | | L | [Defense Watch](https://thedefensewatch.com/defense-systems/type-052d-destroyer/) |
| Frigate | Type 054B | In production | n/a; Type 054A/P export proxy ≈$300–350M | | L | [Quwa](https://quwa.org/daily-news/profile-pakistans-new-hangor-submarine/) |
| Amphibious assault | Type 075 / Type 076 | In service / trials | n/a | Type 076 has a drone catapult | L | [SCMP](https://www.scmp.com/news/china/military/article/3362271/chinas-type-076-amphibious-assault-ship-sichuan-final-tests-drone-catapult) |
| Attack submarine | Type 093B / Type 095 | In service / in development | n/a | | L | [Naval News](https://www.navalnews.com/naval-news/2026/01/chinese-navy-annual-review-2025-part-ii-subs-logistics-rd/) |
| Ballistic missile submarine | Type 096 | In development | n/a | | L | [Wikipedia](https://en.wikipedia.org/wiki/Type_096_submarine) |
| Conventional (AIP) submarine | Type 039C | In service | n/a; Hangor export proxy ≈$250–500M | | L | [Quwa](https://quwa.org/daily-news/profile-pakistans-new-hangor-submarine/) |
| **Air** | | | | | | |
| Stealth air-superiority fighter | J-20A/S | In production | ≈$100–110M (range $30–120M) | ≈120/yr (RUSI estimate) | L | [FlightGlobal](https://www.flightglobal.com/fixed-wing/2026/03/china-surges-production-capacity-for-j-20-and-j-35-stealth-fighters/) |
| 5th-gen multirole fighter | J-35 / J-35A | In production | $50M advertised; $70–100M other estimates | | L | [Quwa](https://quwa.org/pakistan/air-force/combat-aircraft/what-the-reports-say-about-pakistans-j-35-stealth-fighter-deal/) |
| 4.5-gen strike fighter | J-16 | In service | n/a | | n/a | |
| 6th-gen fighter | J-36 / J-50 | Prototypes | n/a | Operational ≈2035 (low confidence) | L | [19FortyFive](https://www.19fortyfive.com/2026/06/what-are-chinas-j-36-and-j-50-sixth-generation-fighters-and-how-far-ahead-are-they-of-americas-f-47-ngad/) |
| Bomber / missile carrier | H-6K / H-6N | In service | n/a | | n/a | |
| Stealth bomber | H-20 | In development | n/a | No confirmed airframe or first flight as of mid-2026 | L | [19FortyFive](https://www.19fortyfive.com/2026/07/china-announced-the-h-20-stealth-bomber-in-2016-a-decade-later-there-is-no-confirmed-aircraft-and-no-first-flight/) |
| Transport / tanker | Y-20 / YY-20 | In production | ≈$160M ($160–250M floated) | | L | [Aviation Week](https://aviationweek.com/defense/aircraft-propulsion/y-20-revolutionizes-chinas-airlifter-tanker-capacity) |
| Airborne early warning | KJ-500 | In service | n/a | | n/a | |
| Stealth combat drone | GJ-11 | In service (limited) | n/a | ≈10–20 built (low confidence) | L | [The Defense Post](https://thedefensepost.com/2025/09/24/china-gj-11-ucav/) |
| ISR drones | WZ-7 / WZ-8 | In service | n/a | | n/a | |
| **Space** | | | | | | |
| ISR / navigation satellites | Yaogan / BeiDou | In service | n/a | BeiDou: 50 satellites in orbit | M | [CGTN](https://news.cgtn.com/news/2026-08-01/China-s-BeiDou-Navigation-Satellite-System-completes-in-orbit-upgrade-1PfkEk924Kc/p.html) |
| LEO megaconstellations | Guowang / Qianfan | In production | n/a | About 200+ satellites each in orbit by October 2026 | L/M | [Wikipedia](https://en.wikipedia.org/wiki/Guowang) |
| Launch | Long March family | In production | LM-5 ≈$100–160M; LM-2D ≈$30M | 92 launches in 2025 | L | [Wikipedia](https://en.wikipedia.org/wiki/Long_March_5) |
| **Strategic and Rocket Force** | | | | | | |
| ICBM | DF-41 (DF-61 and DF-5C also reported) | In service | n/a | Warheads in the low 600s, >1,000 by 2030 (DoD) | M (warheads) | [FAS](https://fas.org/publication/the-pentagons-slimmed-down-2025-china-military-power-report/) |
| Intermediate-range missile | DF-26 / DF-27 | In service / n/a | n/a | | n/a | [CSIS Missile Threat](https://missilethreat.csis.org/missile/dong-feng-26-df-26/) |
| Anti-ship ballistic / hypersonic | DF-21D / DF-17 | In service | DF-21D ≈$10–15M (low confidence) | | L | [RUSI](https://rusi.org/in-the-news/britain%E2%80%99s-newest-warships-risk-chinese) |
| SLBM | JL-3 | In service | n/a | | L | [The Diplomat](https://thediplomat.com/2025/09/what-were-the-weapons-on-display-in-chinas-military-parade/) |
| Hypersonic anti-ship missile | YJ-21 | In service | n/a | | n/a | [CNN](https://www.cnn.com/world/live-news/china-military-parade-xi-jinping-09-03-25-intl-hnk) |

## 7. Building the generic catalog and filling gaps

1. **Generic (mirror) classes use the US planning cost** for each class in §5, because it's the best documented. Both sides share one catalog, so only relative costs matter, and the topline sets the scale.
2. **China cost-gap rule (a balancing assumption, not data).** Where no estimate exists, price a China pack item from its US analog with a category factor. The few paired estimates above suggest ships at about 0.3–0.5× (Type 055 vs. DDG-51 ≈0.33×; Fujian vs. Ford ≈0.5×) and aircraft at about 0.8–1.3× (J-20 vs. F-35 ≈1.1–1.3×). Factors for missiles, ground systems, and space are still TBD. Set the final factors in calibration and publish them each season.
3. **O&S rules of thumb** (assumptions to replace with Selected Acquisition Report data in Phase 1):

| Category | Annual O&S as % of unit cost | Basis |
| --- | --- | --- |
| Combat aircraft | ≈10% | F-35: ≈$6.6M per aircraft per year in 2012 dollars (GAO), roughly $9M today, against ≈$82–92M unit cost |
| Large ships and submarines | 3–5% (ship only; air wings counted separately) | Assumption |
| Ground vehicles | 5–8% | Assumption |
| Satellites | 2–4% (operations and ground segment) | Assumption |
| Munitions | ≈1% (storage and maintenance) | Assumption |
| Attritable drones | 5–10% | Assumption |

4. **Personnel per unit and cost per service member:** not researched yet. Phase 1 sources: DoD military personnel composite pay rates for the US pack; outside estimates for the China pack.

## 8. Industrial capacity

| Indicator | Figure | Conf | Source |
| --- | --- | --- | --- |
| China vs. US naval-relevant shipbuilding capacity | ≈232× by tonnage (a leaked 2023 Office of Naval Intelligence slide; the Navy said it was built from public sources). Mostly commercial tonnage, so the warship output ratio is much smaller | M | [The War Zone](https://www.twz.com/alarming-navy-intel-slide-warns-of-chinas-200-times-greater-shipbuilding-capacity) |
| Share of global shipbuilding | China >50% (USTR, April 2025); US ≈0.04% of tonnage (CRS) | M | [CRS IF12666](https://www.everycrsreport.com/files/2026-08-26_IF12666_1aacdd58a4f8f1cc88913d8d75c81df7f0284616.html) |
| US submarine output | ≈1.1–1.3 Virginia-class per year vs. a 2/yr goal, now expected in the 2030s | M | [USNI News](https://news.usni.org/2026/05/12/virginia-subs-will-hit-2-a-year-build-rate-in-2030s-cno-caudle-says) |
| PLA Navy size | >370 battle-force ships (2024 DoD China report); about 435 projected by 2030 (low confidence) | M/L | 2024 DoD China Military Power Report |
| F-35 deliveries | 98 (2023), 110 (2024), 191 (2025, boosted by clearing a backlog) | H | [Defense News](https://www.defensenews.com/air/2026/01/15/lockheed-delivered-record-191-f-35s-as-it-cleared-out-tr-3-backlog/) |
| J-20 production | ≈120/yr by end-2025 (RUSI estimate), up from ≈20 in 2020; imagery suggests capacity for 300–400 fighters a year across all types | L/M | [FlightGlobal](https://www.flightglobal.com/fixed-wing/2026/03/china-surges-production-capacity-for-j-20-and-j-35-stealth-fighters/) |

**US munitions production (current → target)**

| Munition | Current | Target | Source |
| --- | --- | --- | --- |
| 155mm shells | 36,000/month (March 2026) | 100,000/month (original October 2025 goal, slipping) | [Military Times](https://www.militarytimes.com/news/your-military/2026/07/14/manufacturing-woes-hamper-us-155-mm-ammo-production/), [CBS News](https://www.cbsnews.com/news/defense-watchdog-155mm-artillery-plant-production-goals/) |
| PAC-3 MSE | 620 delivered in 2025 | 2,000/yr; tripled output not before 2028 | [Lockheed Martin](https://news.lockheedmartin.com/2026-04-10-Lockheed-Martin-Secures-First-Contract-for-PAC-3-R-MSE-Accelerated-Production,-Strengthening-the-Arsenal-of-Freedom) |
| THAAD | 96/yr | 400/yr | [Lockheed Martin](https://news.lockheedmartin.com/2026-01-29-Lockheed-Martin-and-U-S-Department-of-War-Sign-Framework-Agreement-to-Quadruple-THAAD-Interceptor-Production-Capacity) |
| GMLRS | 14,000/yr | Army seeks 133,014 through 2034 | [Defense News](https://www.defensenews.com/news/your-military/2026/08/18/us-army-requests-133000-gmlrs-rockets-by-2034/) |
| PrSM | ≈400/yr | 550/yr | [Lockheed Martin](https://www.lockheedmartin.com/en-us/news/features/2026/lockheed-martin-and-the-us-department-of-war-further-expand-prsm-production.html) |
| SM-6 | ≈125/yr (before the framework deal) | >500/yr | [CSIS](https://www.csis.org/analysis/how-quickly-can-dod-rebuild-and-recast-munitions-industrial-base) |
| Tomahawk | n/a | >1,000/yr | [CSIS](https://www.csis.org/analysis/rebuilding-us-missile-inventory-multiyear-project) |

**Interceptor stocks (a calibration data point).** CSIS estimates US Patriot stocks fell from ≈2,330 to 759–827 and THAAD from ≈452 to 239–257 (July 2026; not official counts; low confidence). Source: [Visual Capitalist summary](https://www.visualcapitalist.com/america-shrinking-patriot-thaad-missile-inventories/).

**Build-time notes**

- Ford-class carriers take 10–11 years per ship, vs. 7–8 for the Nimitz class (CBO).
- Fujian took 3.4 years from launch to commissioning; construction started years before launch.
- DDG-51 Flight III deliveries run 22–58 months late. Virginia-class boats run 24–36 months late, and the lead Columbia 12–18 months late ([GAO testimony](https://armedservices.house.gov/uploadedfiles/2026-04-22_oakley_testimony.pdf)).

**What this means for the game.** US pack: small, expensive shipyard lines; munitions lines that can grow but take 2–4 years to do it; strong aerospace lines. China pack: much larger and cheaper naval lines, and fast-growing aircraft capacity. Starting line capacities for every pack are set in calibration.

**Known gaps to close in Phase 1**

- Unit costs for AIM-120D and IFPC.
- O&S data for everything except the F-35.
- Absolute keel-to-delivery times for US destroyers and submarines.
- Most Chinese unit costs.
- FY2027 appropriations and second-reconciliation outcomes.

**Modeling cautions**

- Don't mix budget-line averages with flyaway or contract-average costs in the same class.
- Treat FY2027 quantities funded through reconciliation as at risk (for example, 53 of 85 F-35s).
- Use Chinese export prices only as proxies.

## 9. Sources to verify and update against

| Source | Use it for |
| --- | --- |
| GAO *Weapon Systems Annual Assessment* | Program costs, schedules, overruns (calibrates the overrun model) |
| DoD budget justification books (P-1 procurement, R-1 RDT&E) | Unit costs and quantities by year |
| Selected Acquisition Reports (SARs) | APUC/PAUC, O&S estimates |
| CBO (shipbuilding plan analyses, Golden Dome and missile-defense estimates) | Long-run costs, independent estimates |
| CRS reports | Program summaries with cited figures |
| DoD *Military and Security Developments Involving the PRC* (annual) | PLA force structure and trends |
| Defense press (Breaking Defense, Defense News, USNI News, Air & Space Forces Magazine, The War Zone, SpaceNews) | Recent milestones and contract values |
| Think tanks (CSIS, RAND, IISS summaries) | Chinese cost estimates, capacity comparisons |

US government works are generally public domain. IISS *Military Balance* data is copyrighted and licensed, so cite it but don't copy its tables into the repo.
