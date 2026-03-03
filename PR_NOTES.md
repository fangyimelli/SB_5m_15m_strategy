# PR Notes

## What changed (latest)
- Removed Great FRD/FGD rectangle-gate inputs and all rectangle/great-state calculations from the indicator flow.
- Restored baseline daytype logic on daily bars:
  - FGD = yesterday DumpDay AND today close > open.
  - FRD = yesterday PumpDay AND today close < open.
  - Trade Day = yesterday was FRD or FGD (next-day trigger).
- Daytype markers (FRD/FGD/TRADE DAY) now use daily pulse `plotshape` only and are no longer hidden by `min_score_to_display`, NY-session gating, or focus mode by default.
- Added `focus_hide_daytype` (default `false`) so focus mode can optionally hide daytype markers without affecting trading logic.
- Kept `require_sb_daytype` bias gate for direction filtering while removing Great-only dependencies.
- Preserved memory hardening:
  - No BLOCKED labels on chart (dashboard-only).
  - Signal labels remain Blue1/Blue2/Blue3 only, with bounded queue (`max_signal_labels=30`) and oldest-first deletion.
  - BOS/FVG/Entry/TP/SL drawing objects are deleted before redraw.
  - Training miss diagnostics remain `plotshape` (no extra labels).
  - Tightened indicator box cap (`max_boxes_count=10`).

## Validation checklist
- [x] Great FRD/FGD inputs and rectangle gate logic removed.
- [x] FRD/FGD/Trade Day daily pulse markers restored and always visible by default.
- [x] `min_score_to_display` applied only to trading-related draw/alerts path.
- [x] Focus mode no longer hides daytype markers by default (`focus_hide_daytype=false`).
- [x] README Removed/Deprecated log updated.
