# Changelog

All notable changes to the networking self-assessment tool (tools.werkitgirls.com)
are logged here, newest first.

---

## 2026-09-27

### Fixed
- **Action items accumulating across repeated results.** `showResults()` never
  cleared the `#actionItems` container before rendering a new set, so retaking
  the assessment more than once without reloading the page caused each style's
  action list to stack on top of whatever was already there — 10+ items after
  two attempts, 20+ after several. This is the likely explanation for an
  earlier report of a high-scoring result showing a low-tier, seemingly
  mismatched action item (a leftover from an earlier, lower-scoring attempt in
  the same session, not a scoring error).
  - Root cause: missing `actionWrap.innerHTML = '';` before the
    `actionTier.forEach(...)` loop.
  - Fix: added the missing reset line.
  - Also added a small hardening measure while in the area: option buttons now
    briefly lock (`pointer-events: none`) during the 420ms auto-advance window,
    so a stray double-click can't register two different answers for the same
    question. Not a fix for a confirmed bug — just removing a possibility.
  - Verified: scoring math (overall score, all four cluster totals) traced
    correctly against the underlying formulas on a clean, slow retake before
    this fix; confirmed the accumulation bug specifically by retaking twice in
    a row without reloading.
