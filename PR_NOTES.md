# PR Notes

## What changed (latest)
- Added **backtestable Focus Trade Day** flow on current branch (no new branch):
  - Day2 remains FRD/FGD.
  - Day3 is Trade Day (`tradeDayToday = FRD_yesterday OR FGD_yesterday`).
  - FRD/FGD and TRADE DAY remain `plotshape` markers (no `label.new` daytype objects).
- Refined focus mode to NY 09:30 anchor inputs:
  - `focus_mode` (default `false`)
  - `focus_trade_day_ny0930` (`input.time`, `0` means auto-use today NY 09:30)
  - `focusAnchor = timestamp(timezone, Y,M,D,09,30)`
  - `focusNYStart = focusAnchor`, `focusNYEnd = focusAnchor + NY session length`
  - Focus Asia source is previous-night session (`20:00-00:00 NY`) for that Trade Day.
- Focus display behavior:
  - `focus_mode=true`: dashboard and chart draws are restricted to the focused NY session window; key lines/boxes/blue labels outside window are hidden/cleaned.
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
- [x] Trade Day logic follows Day3 = yesterday FRD/FGD.
- [x] Daytype markers are `plotshape` only.
- [x] Focus anchor/session and Asia attribution aligned to NY 09:30 + previous-night Asia session.
- [x] Focus mode defaults to no alerts.
- [x] Dashboard headline + progress format implemented.
- [x] README/PR notes/template requirements satisfied.
