# SB 5m/15m Strategy Indicator

TradingView Pine Script indicator implementing SB workflow with portfolio/chart modes, focus mode, and staged Blue signals.

## Removed / Deprecated Log
- 2026-03-03: Debugged FRD/FGD visibility conflicts: added dashboard-only `debug_mode`, split daytype scanner labels from score gate, added `show_daytype_labels` + `show_only_great_daytype`, ensured focus mode does not alter D-bar daytype source, and relaxed Great rectangle defaults with per-condition diagnostics.
- 2026-03-03: Memory-limit hardening: reduced indicator object caps (labels/lines/boxes), switched to single-symbol security flow, throttled signal labels with capped queue, blocked reasons moved to dashboard-only, and dashboard now reuses a persistent table with text-only updates.
- 2026-03-03: Deprecated TA helper request-pack functions (`f_req5/f_req15/f_reqD`) due compiler consistency warnings; inlined `request.security` tuples and replaced touch counting to avoid unavailable `ta.sum`.
- 2026-03-03: Deprecated basic DayType-only trade-day enablement as sole gate; integrated **Great FRD/FGD rectangle** requirement and score-based visibility gate (`min_score_to_display`) for cleaner output.
