# STATE: session continuity

Read this first (after `CLAUDE.md`), update it at the end of every working session.
Holds: current phase, capability status, what is next, known blockers.

**History is not here.** What each phase concluded is one row per phase in the
[phase ledger](specs/README.md); the reasoning behind each is in that phase's spec,
under Decisions. This file stays short on purpose: it is rewritten every session, so
an append-only record inside it grows without bound.

---

## Current phase

**No active phase.** Nothing is built. G0 is the next step and S1 is written first and
alone.

## Capability status

| Capability | Status | Where |
| --- | --- | --- |
| All | Not started | `src/bikeshare/` is empty |

## Next

G0, in four groups. A prior-art and source-reading pass put most of this here, and the
reasoning for each sits with the decision that added it.

**Read before writing a spec.** Bi, Ye & Zhu on classifying a bicycle's operational
states comes before S1, because the filter that separates operator moves from
maintenance moves and dropped rides depends on it. Xu, Yan, Goh & Jaillet on locational
demand comes before S2, as the nearest published relative of the demand model and of its
substitution treatment.

**Compute from the archive.** Build the snapshot coverage report, records per
station-day against the expected cadence. The published gap list is known to be
incomplete, so the study window is chosen against the computed report rather than
against the gaps anyone announced.

**Specify two estimators, not one.** S2 carries both the availability offset and a
likelihood written on the stockout time, fitted on the same reconstruction, and reports
the gap between them as a measurement.

**Check against outside numbers.** Report the teleport recovery rate against the 77 per
cent reference, and the Chicago moves-per-hundred-trips ratio against the verified
median of 6.9 from another city over 2016 to 2018. Both are brackets from elsewhere, not
targets to fit.

Two open requests that nothing waits on. Confirm the 2013 agreement's Schedule 20
thresholds against a clean copy, the public scans having no text layer and being read by
OCR. And ask the city once, through records, for any rebalancing count covering the
window.

## Known blockers

- None. The two open requests above are not blocking: each has a stated fallback.
