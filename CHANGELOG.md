# HaaS TCO Calculator — Changelog

---

## v0.8 FINAL — 2026-04-21

- Release status: Marked as final release version for production landing flow

### UX / Insights
- **Insight chip typography**: Increased body font size (0.70 → 0.82 rem), highlight font size (0.80 → 0.90 rem), icon size (0.90 → 1.05 rem), added `letter-spacing: 0.01em` for readability
- **Amortisation chip** (3 states): "cheaper from day one" / "amortises in X years vs Standard HP" / "doesn't amortise within lifetime (gap: €X)" — replaces flat static text
- **Low-heat warning chip** (`.insight-chip.warning`): Appears when total heat demand < 8,000 kWh/a; explains CAPEX/kWh dominance in orange (`--gas` color) to be transparent about Passivhaus regime where gas wins on CAPEX

### State Management (Preset / Scenario Bug Fix)
- **Root cause**: `applyPreset()` was resetting `activeScenario`, and scenario selection was visually clearing preset highlights — presets and scenarios are orthogonal axes
- **Fix**: `applyPreset()` no longer resets `activeScenario`
- **Fix**: All building config listeners (building class, area, persons sliders) now call `activePreset = -1; renderPresets()` on change, visually de-highlighting the preset when the user manually adjusts building parameters
- **Preset note**: When no building preset is selected (`activePreset === -1`), a small italic note `(no building type selected — using default 18,000 kWh/a)` appears next to the preset bar so the user understands the active heat demand assumption

### Calibration
| Parameter | Before | After | Reason |
|---|---|---|---|
| Passivhaus heat demand | 6,250 kWh/a | 5,500 kWh/a | More accurate Passivhaus target |
| Passivhaus `self_consumption_hp` | not set | 0.50 | Parity with EP for fair comparison |
| Pessimistic lifetime | 12 yr | 13 yr | More realistic minimum scenario |
| Pessimistic carbon price | 45 €/t | 55 €/t | Better reflects current trajectory |
| Pessimistic HP electricity spread | EP+4 ct | EP+2 ct | Tighter spread in adverse market |
| Optimistic HP electricity spread | EP+4 ct | EP+6 ct | Wider spread in optimistic market |
| Gas service cost | 350 €/yr | 200 €/yr | Corrected to market-realistic value |

### Tech Cards (Visual)
- **Image placeholders added** to all three tech cards (ElephantONE, Standard HP, Gas Boiler)
- Placeholder dimensions: full card width × 180 px, dashed border, dark gradient background, camera icon
- Header layout changed from `row` (side-by-side title + image) to `column` (title above, image below as banner) to maximise visible image area
- Recommended image spec: **landscape 4:3 ratio**, PNG with transparent background, 1600×1200 px for hi-res delivery

### Sidebar Panel Order
- **Value Stacking (EP only)** section moved from last position to directly above the ElephantONE section, grouping EP-specific parameters together for logical flow

---

## v0.7 — prior session

- Bilingual EN / Traditional Chinese (ZH) support via `STRINGS` object and `T()` / `Tf()` helpers
- HP color changed from blue to orange (`--hp: #F5A623`) for brand consistency
- Flip cards introduced: front (summary cost) / back (full CAPEX + OPEX + energy breakdown)
- Sparklines per card: savings vs gas with zero-line, break-even dot, dual fill (positive/negative)
- Value Stacking panel: §14a EnWG, §22 EnFG, dynamic tariff, VPP toggle (mutually exclusive with §14a)
- HaaS / Buy finance mode toggle
- Light mode (collapsed parameters) / Full mode toggle
- CO₂ panel and derived values panel
- Scenario bar: Pessimistic / Balanced / Optimistic one-click presets
- Building preset bar: Passivhaus / GEG 2020 / Bestand 1980 / Altbau <1978
- Cumulative chart: Savings vs Gas and Absolute Costs toggle
- LCOH breakdown chart: €/kWh / €/year / Lifetime € views with legend toggle
- Savings cards: annual + lifetime savings EP vs Gas, HP vs Gas
- Verification table (collapsible): compares live output against hardcoded expected values

---

## v0.1 – v0.6 — initial builds

- Single-file HTML + CSS + JS architecture
- Dark theme: `--bg: #0B1120`, `--surface: #111927`, `--text: #E2E8F0`
- Canvas 2D charts (Chart.js via CDN)
- Three-way TCO/LCOH calculation: ElephantONE vs Standard HP vs Gas Boiler
- Subsidy modelling (BEG rate), PV self-consumption credit, carbon cost
- Parameter sidebar with range sliders and collapsible sections
- StaticCrypt password protection for distribution versions (v0.6, v0.7)
