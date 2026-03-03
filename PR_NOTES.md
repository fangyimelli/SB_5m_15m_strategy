# PR Notes

## What changed (latest)
- Removed helper request-pack functions and inlined request tuples to satisfy Pine consistency warnings.
- Replaced rectangle touch counting implementation to avoid `ta.sum` reference errors.
- Added Great FRD/FGD gate inputs and rectangle proxy checks (compression/flatness/touches).
- Trade Day and blue-flow enablement now depend on Great-daytype when enabled.
- Added score visibility filter (`min_score_to_display`) to hide low-quality chart outputs and alerts.
- Added Day2 rectangle visualization option (`draw_rectangle_box`) and Great reason propagation to dashboard status.
- Updated docs and PR template governance files.

## Validation checklist
- [x] Pine script updated with Great gate logic.
- [x] Score visibility gate integrated with draw/alert paths.
- [x] README Removed/Deprecated log updated.
- [x] PR template exists and includes Removed/Docs/Acceptance sections.
