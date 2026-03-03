# PR Notes

## What changed (latest)
- Fixed focus-date off-by-one and focus/trade-day state freeze issues:
  - Renamed input to `focus_input = input.time(...)` and derive `focusY/focusM/focusD` with timezone parameter.
  - `focusAnchor = timestamp(timezone, y, m, d, 09, 30)` remains NY-aligned.
  - Focus headline/HUD date now uses explicit `Y-M-D` string assembly to avoid exchange-timezone date drift.
- Focus mode now controls **display only**, not **calculation**:
  - Removed focus-based calc window gating from Asia range build and state-machine progression.
  - Daily state reset runs regardless of focus mode to keep 15m trade-day flow healthy.
  - Added `isInFocusDay = time >= focusDayStart && time < focusDayEnd` and apply it only to HUD/plot/alerts visibility.
  - Ensures prior-night Asia session (20:00-00:00 NY) still participates in trade-day computation even when focused output is hidden.
- Added temporary HUD debug line:
  - `Focus anchor(Y-M-D)=...`
  - `tradeDayToday=...`
  - Used to verify selecting day 14 displays/aligns to day 14.
- Pinned HUD defaults/update behavior for stable center-left display and memory safety:
  - Defaults: `show_hud=true`, `hud_x_bars_offset=30`, `hud_y_anchor="LastPrice"`, `hud_bg_opacity=70`, `hud_text_size="small"`, `hud_align="left"`.
  - Enforced HUD refresh only on `barstate.islast` and reuses a single `var label hudLbl` via `label.set_xy` + `label.set_text`.
  - HUD keeps BOS/FVG/Retest/Blue1/2/3/ScoreA/ScoreA+ ✅/⛔ rows and preserves `入場規則=Blue3✅才手動進場`.
- Added **backtestable Focus Trade Day** flow on current branch (no new branch):
  - Day2 remains FRD/FGD.
  - Day3 is Trade Day (`tradeDayToday = FRD_yesterday OR FGD_yesterday`).
  - FRD/FGD and TRADE DAY remain `plotshape` markers (no `label.new` daytype objects).
- Refined focus mode to NY 09:30 anchor inputs:
  - `focus_mode` (default `false`)
  - `focus_input` (`input.time`, `0` means auto-use today NY 09:30)
  - `focusAnchor = timestamp(timezone, Y,M,D,09,30)`
  - Display filter day boundary: `isInFocusDay = [focusDayStart, focusDayEnd)`.
- Focus display behavior:
  - `focus_mode=true`: dashboard and chart draws are restricted to focused day output; key lines/boxes/blue labels outside focus day are hidden/cleaned.
  - `focus_mode=false`: normal realtime behavior unchanged.
  - Alerts stay off by default in focus (`allow_alerts_in_focus=false`).
- Dashboard updated to progress style with a focus headline:
  - `FOCUS: YYYY-MM-DD 09:30 NY | TradeDay=Y/N | Grade=A/A+ | Hidden<70=Y/N`
  - Progress columns: `Asia/Sweep/BOS/FVG/Retest/Blue3`.
- Docs/template housekeeping:
  - Added short Removed/Deprecated log entry in `README.md`.
  - Updated this `PR_NOTES.md`.
  - Confirmed `.github/pull_request_template.md` includes `Removed / Deprecated`, `Docs Updated`, and `驗收 (Acceptance)` sections.

## Validation checklist
- [x] Focus date shown in HUD/dashboard matches selected NY calendar day (no minus-one day drift).
- [x] Focus mode only filters outputs, not core bar-by-bar calculations.
- [x] Trade Day logic follows Day3 = yesterday FRD/FGD.
- [x] Daytype markers are `plotshape` only.
- [x] Focus anchor/session and Asia attribution aligned to NY 09:30 + previous-night Asia session.
- [x] Focus mode defaults to no alerts.
- [x] Dashboard headline + progress format implemented.
- [x] README/PR notes/template requirements satisfied.
