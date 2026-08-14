# Changelog

All notable changes to the TeslaMate Ecos Dashboards are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.4.0] - 2026-08-15

### Changed
- **Visual overhaul**: historical sparklines added to stat panels across all dashboards, with a consistent modern color palette (blue/orange/green/purple/teal/amber).
- **Battery Capacity & Degradation**: `Battery Health` is now a gauge; uniform widget sizes and logical ordering; replaced `Total Energy Charged` with `Max Rated Range`.
- **Charging Analysis**: weighted efficiency (`SUM/SUM`) across all efficiency panels; `Charging Cost per Month` is cost-only (removed the TL-vs-kWh axis mix).
- **Charging Pattern**: modern hex colors and frequency-ordered slices.

### Fixed
- **Units**: custom `kWh`/`kW` → proper Grafana codes (`kwatth`/`kwatt`); counts → `none`; cost panels → `currencyTRY`.
- **Drive Efficiency / Vehicle State / Finance**: missing `base_url` variable (broke the TeslaMate dashboard link).
- **Battery**: `Capacity by Firmware Version` missing CTE alias (`CROSS JOIN rated_efficiency re`).
- **Charging Habits**: `Charging Cost per Month` mixed TL and kWh on a single axis.

### Removed
- **Data Quality** dashboard (empty integrity checks; low value for a single-car setup).

### Documentation
- Refreshed all dashboard screenshots and added new-dashboard sections to the README.

## [2.3.0] - 2026-08-14

### Added
- **Drive Efficiency** (new dashboard): consumption by speed and temperature, monthly distance/duration, drives by day-of-week, elevation.
- **Vehicle State** (new dashboard): current status, online/offline/asleep time distribution, recent state transitions.
- **Data Quality** (new dashboard): incomplete drives/charges and missing-field detection.
- **Finance** (new dashboard): charging cost, cost per kWh/km, total cost of ownership (optional purchase price variable).
- **Battery Capacity & Degradation**: `Capacity by Firmware Version` and `Firmware Update History` panels.
- **Charging Habits**: `Free / Home Energy` stat.

## [2.2.0] - 2026-08-14

### Added
- **Battery Capacity & Degradation**: `Capacity vs Odometer` — degradation by mileage (500 km bands).

## [2.1.0] - 2026-08-14

### Added
- **Charging Analysis**: `Charging Power Curve (by SOC)` — average charging power per 5% state-of-charge bucket.
- **Charging Analysis**: `Charging Power by Temperature` — cold-weather charging behaviour.
- **Charging Analysis**: `DC Fast Charger Comparison` — per-operator peak/average power, energy and duration.
- **Charging Habits**: `Charging Cost by Location` — per-location cost using geofence unit price (TL/kWh) and recorded session cost.

## [2.0.0] - 2026-08-14

### Fixed
- **Battery Capacity & Degradation**: `30-Day Avg` was a ~5-minute rolling window (`ROWS 29 PRECEDING` over per-minute rows) instead of 30 days — now `RANGE INTERVAL '30 days'`.
- **Battery Capacity & Degradation**: degradation rate used two different methods (session-based vs per-minute) giving contradictory values (-5.15 vs -1.33 kWh/yr); unified on session-based `rated_battery_range_km × efficiency / usable_battery_level` (-1.10 kWh/yr).
- **Battery Capacity & Degradation**: hero `Battery Health` was the last per-minute reading, inconsistent with the `Current Capacity`/`Reference` stats beside it; now `current_capacity / reference`.
- **Battery Capacity & Degradation**: reversed `Degradation Rate` thresholds (no-degradation showed red).
- **Battery Capacity & Degradation**: `Capacity Over Time` mixed two capacity estimation methods; unified on `rated_battery_range_km × efficiency / usable_battery_level`.
- **Phantom Drain**: `Worst Drain Days` "Park Hours" was the average gap, not the total — now `SUM(gap_hours)`.
- **Charging Analysis**: `Daily Energy Added` DC series used `end_date` while AC used `start_date` — unified.
- **Charging Analysis**: `Efficiency vs Temperature` lumped `<0°C` into `0-5°C` and `>30°C` into `25-30°C` — added proper edge buckets.
- **Charging Analysis**: efficiency was a mix of weighted (`SUM/SUM`) and unweighted (`AVG(ratio)`) — unified on weighted everywhere.
- **Charging Habits**: `AC vs DC Energy Trend` used `end_date` — unified to `start_date`.

### Removed
- Dead dashboard variables that were defined but never used: `temp_unit` (all), `efficiency` (Battery/Charging Analysis/Charging Habits), `length_unit`/`preferred_range` (where unused), `nominal_capacity` (Charging Analysis, where it had a different time-dependent definition).
- Meaningless sparklines on single-value stat panels.

### Changed
- **Charging Habits**: AC/DC colors standardized to hex (`#73BF69`/`#F79520`) to match Charging Analysis.
- **Charging Habits**: `Total Sessions` and `Avg Energy / Session` stat panels configured (neutral gray, no sparkline, `kWh` unit).
- Detail rows are now collapsible.
- README: fixed `LICENSE` description (MIT → GPLv3) and corrected the variables table.

## [1.4.0] - 2026-08-07

### Changed
- **Phantom Drain**: dashboard redesign — streamlined to 12 panels with improved UI.
- **Phantom Drain**: fixed state lookup, date grouping, unit handling (lengthkm/velocitykmh/kWh).

## [1.3.0] - 2026-06-15

### Changed
- **Phantom Drain**: streamlined to 12 panels with improved UI quality.

## [1.2.1] - 2026-06-15

### Fixed
- **Phantom Drain**: missing `xField` on barchart panels and SQL errors.

## [1.2.0] - 2026-06-15

### Added
- **Phantom Drain**: 7 new panels — worst days, state timeline, scatter, monthly trend, hourly drain, seasonal.

## [1.1.3] - 2026-06-14

### Added
- **Charging Efficiency**: AC/DC efficiency, monthly stats, duration/SOC analysis, top/bottom sessions.
