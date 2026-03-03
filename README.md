# SB 5m/15m Strategy Indicator

TradingView Pine Script indicator implementing SB workflow with portfolio/chart modes, focus mode, and staged Blue signals.

## Removed / Deprecated Log
- 2026-03-03: Fix: focus_mode snapshot session matching now uses fixed 5m context (`t5`, `inNy5`, `barDayKey5`, `condFocusNY5`), HUD snapshot fields use `condFocusNY5`, TradeDay uses whole-day `condFocusDay5`, and debug shows `focusDayKey/foundSnap/lastNYBar` to prevent false `foundSnap=N` on non-5m chart timeframes.
- 2026-03-03: Fix: focus HUD uses valuewhen snapshot for selected day (`stSnap/tradeDaySnap/scoreASnap/scoreAPlusSnap/fvgMitSnap/bosDirSnap`), removes focus-day masking fallback, and shows `—` only when selected day snapshot is unavailable.
- 2026-03-03: Fix: HUD focus snapshot bool fields now use `focusSnapshotOk` (from `ta.valuewhen(condFocusDay, time, 0)`) for `—` fallback; removed invalid `na()` checks on `series bool` values to fix Pine compile error.
- 2026-03-03: Fix: focus HUD uses `ta.valuewhen(condFocusDay, ..., 0)` snapshot fields (state/TradeDay/NY/BOS/FVG/Retest/Blue1/2/3/ScoreA/ScoreA+) instead of last-bar masking; show `—` only when selected focus day snapshot is unavailable (`na`).
- 2026-03-03: Fix: focus day detection uses `dayKey` comparison (`barDayKey == focusDayKey`) to avoid exchange/UTC day-shift; NY session filter split to `isInFocusNYSession` for focus-mode draw/alert gating.
- 2026-03-03: Removed dashboard table; HUD-only status panel; reserved label capacity for HUD; removed FRD*/FGD* star labels.
- 2026-03-03: Focus anchor date extraction now uses timezone-aware `year/month/dayofmonth(..., "America/New_York")`; HUD focus date uses Y-M-D string composition (no exchange-timezone `tostring(time)` drift), and focus mode now gates display/output only while state-machine/Asia/Sweep/BOS/FVG/Blue calculations run on all historical bars.
- 2026-03-03: HUD defaults pinned to center-left (`x=bar_index-30`, `y=LastPrice`, small/left text, semi-transparent bg) and HUD refresh forced to `barstate.islast` with a single reusable label to prevent memory growth.
- 2026-03-03: Added backtestable Focus Trade Day mode anchored at NY 09:30 (Day2=FRD/FGD, Day3=Trade Day), bound focus Asia range to prior 20:00-00:00 NY session, switched focus dashboard to single-day progress view, and kept daytype markers as plotshape pulses.
- 2026-03-03: Removed Great FRD/FGD rectangle gate; restored baseline FRD/FGD + Trade Day (next-day) markers via daily pulses, and decoupled daytype visibility from score/NY/focus trading-output gates.
- 2026-03-03: Debugged FRD/FGD visibility conflicts: added dashboard-only `debug_mode`, split daytype scanner labels from score gate, added `show_daytype_labels` + `show_only_great_daytype`, ensured focus mode does not alter D-bar daytype source, and relaxed Great rectangle defaults with per-condition diagnostics.
- 2026-03-03: Memory-limit hardening: reduced indicator object caps (labels/lines/boxes), switched to single-symbol security flow, throttled signal labels with capped queue, blocked reasons moved to dashboard-only, and dashboard now reuses a persistent table with text-only updates.
- 2026-03-03: Deprecated TA helper request-pack functions (`f_req5/f_req15/f_reqD`) due compiler consistency warnings; inlined `request.security` tuples and replaced touch counting to avoid unavailable `ta.sum`.
- 2026-03-03: Deprecated basic DayType-only trade-day enablement as sole gate; integrated **Great FRD/FGD rectangle** requirement and score-based visibility gate (`min_score_to_display`) for cleaner output.
