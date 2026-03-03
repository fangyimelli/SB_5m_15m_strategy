# PR Notes

## What changed (latest)
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
- [x] README Removed/Deprecated log updated.
- [x] PR template includes Removed/Docs/驗收 sections.
