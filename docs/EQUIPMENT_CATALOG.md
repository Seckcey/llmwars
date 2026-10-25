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

<!-- RESEARCH: catalog body -->
_Pending: budgets, 2025–2026 developments, US/China/generic catalogs, and industrial capacity are being filled in from current research._

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
