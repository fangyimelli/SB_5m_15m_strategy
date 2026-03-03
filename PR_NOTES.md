# PR Notes

## What changed (latest)

- 2026-03-03 Focus HUD snapshot source fix (selected-day HUD, no last-bar mask):
  - Focus day key remains `focusDayKey = f_dayKey(focusAnchor, timezoneInput)` and `condFocusDay = barDayKey == focusDayKey`.
  - Added explicit snapshot series in HUD block: `stSnap`, `tradeDaySnap`, `scoreASnap`, `scoreAPlusSnap`, `fvgMitSnap`, `bosDirSnap` via `ta.valuewhen(condFocusDay, ..., 0)`.
  - HUD rendering now switches by mode (`stHud/tradeDayHud/scoreAHud/scoreAPlusHud/fvgMitHud`) and derives BOS/Retest/Blue1/2/3 from `stHud >= STATE_XX`.
  - Removed focus masking fallback in HUD (`focus_mode && !isInFocusDay` => forced `—/⛔`); now only `hasSnap = not na(stSnap)` drives `—` when selected date is absent in loaded history.
  - HUD date line keeps selected `focusDateText`; debug line is now `focusDayKey=... | foundSnap=Y/N` controlled by `hud_show_focus_debug` input.
- 2026-03-03 Follow-up compile fix (HUD bool snapshot NA check):
  - Resolved Pine type error `Cannot call "na" with argument "series bool"` by removing `na()` checks on bool snapshot fields (`inNyHud`, `tradeDayHud`, BOS/FVG/Retest/Blue* booleans).
  - Added `focusSnapshotTs = ta.valuewhen(condFocusDay, time, 0)` as snapshot existence probe and use `focusSnapshotOk` to drive `—` fallback for all bool HUD rows.

- 2026-03-03 Focus HUD snapshot fix (focus_mode):
  - Kept day-key focus judgement: `focusDayKey = f_dayKey(focusAnchor, timezoneInput)`, `barDayKey = f_dayKey(time, timezoneInput)`, `condFocusDay = barDayKey == focusDayKey`.
  - HUD fields in `focus_mode=true` now use `ta.valuewhen(condFocusDay, <series>, 0)` to read focus-day snapshot rather than relying on last-bar date context.
  - Applied snapshot retrieval to: `state`, `isTradeDay`, `inNY`, `BOS/FVG/Retest/Blue1/Blue2/Blue3`, `ScoreA/ScoreA+`.
  - Removed focus masked fallback (`focus_mode && !isInFocusDay` => `—/⛔`). Now HUD shows `—` only when the selected focus-day snapshot is unavailable (`valuewhen` returns `na`, e.g. selected day not loaded in history).
  - HUD debug keeps `barDayKey/focusDayKey` and adds `focusSnapshot=OK/NA`.
  - HUD title updated to `焦點日快照=YYYY-MM-DD 09:30 NY`.

- 2026-03-03 Focus-mode day/session gating fix:
  - Replaced focus day check from timestamp window (`focusDayStart/focusDayEnd`) to day-key comparison: `focusDayKey = f_dayKey(focusAnchor, timezoneInput)`, `barDayKey = f_dayKey(time, timezoneInput)`, `isInFocusDay = barDayKey == focusDayKey`.
  - Added `isInFocusNYSession = time >= focusNYStart and time < focusNYEnd` and switched focus filters for draw/Blue/alerts to NY-session scope in focus mode.
  - HUD masking now continues to use `focus_mode && !isInFocusDay` (dayKey-based), so TradeDay/BOS/FVG/Blue/Score render correctly on selected focus day.
  - Added HUD debug line: `barDayKey=... | focusDayKey=...` for quick verification.
- 2026-03-03 HUD-only/label-capacity cleanup on current branch:
  - Removed dashboard `table` rendering and related table text refresh logic; HUD is now the only status surface.
  - Reserved label capacity for HUD in lifecycle cleanup (`reservedLabels=2`, `capForSignals=min(maxSignalLabels, max(1, maxLabelsCap-reservedLabels))`) so signal labels cannot starve HUD label creation.
  - Changed HUD defaults/content: `hud_y_anchor` default to `High`, keep single `barstate.islast` HUD label, and switched to vertical ✅/⛔ rows (`日期/TradeDay/Grade/NY/BOS/FVG/Retest/Blue1/Blue2/Blue3/ScoreA/ScoreA+`).
  - Enforced Blue behavior: Blue1/Blue2 alert-only (no signal labels, no TP/SL), Blue3-only alert + Entry/TP(+50p)/SL drawing.
  - Restored scanner marker text from `FGD*`/`FRD*` to `FGD`/`FRD`.
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
