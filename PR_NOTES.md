# PR Notes

## What changed (latest)
- Added `debug_mode` with dashboard-only daytype diagnostics (pump/dump, FRD/FGD, rectangle sub-conditions, great flags, hidden reasons, final daytype state).
- Split display gates: `min_score_to_display` now affects entry/Blue/BOS/FVG/TP/SL/alerts drawing only; FRD/FGD scanner labels are controlled by `show_daytype_labels` and `show_only_great_daytype`.
- Kept FRD/FGD daytype computation on adjacent daily bars from `request.security("D")`; focus mode remains a display/time-window filter.
- Relaxed Great-rectangle defaults to reduce false-zero hits (`rect_lookback_bars=16`, `rect_max_atr_mult=1.3`, `rect_body_ratio_max=0.55`, `min_touches_each_side=1`) and surfaced sub-condition debug flags.
- Preserved spec behavior: `require_great_daytype` still gates trade-day/Blue flow, while normal FRD/FGD labels can be shown independently.
- Fixed TradingView memory pressure by enforcing conservative indicator object caps (labels/lines/boxes).
- Removed multi-symbol/portfolio `request.security` loop usage; runtime now evaluates current chart symbol only.
- Enforced recyclable drawing objects (`bosLine`, `fvgBox`, `entryLine/tpLine/slLine`, `tpLbl/slLbl`) before redraw.
- Signal labels restricted to Blue1/Blue2/Blue3 with per-bar dedupe + cooldown + bounded label queue (`max_signal_labels`).
- Blocked reasons are dashboard-only by default (no per-bar blocked labels).
- Dashboard refactored to persistent `var table` + `table.cell_set_text` updates; no per-bar table recreation.
- Focus mode dashboard output constrained to focus-day status row only.
- Training mode miss-cases remain `plotshape` only (no `label.new`).

## Validation checklist
- [x] Pine script object caps and recyclable object flow updated.
- [x] Multi-symbol security loop removed from active logic.
- [x] Label throttling and queue cap updated.
- [x] Dashboard update strategy switched to `cell_set_text`.
- [x] README Removed/Deprecated log updated (daytype visibility debug changes).
- [x] PR template includes Removed/Docs/驗收 sections.
