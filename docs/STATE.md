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
| Everything | Not started | A GBFS poller was built and parked on 2026-09-15; its code is gitignored and it is not running |

## Next

G0, in the groups below. A prior-art and source-reading pass put most of this here, and
the reasoning for each sits with the decision that added it.

**No live data collection for now.** The Belgian comparison uses Max Halford's public
`bike-sharing-history` archive: Villo from August 2023 and Velo Antwerpen from April 2024,
both docked systems, snapshotted about every 16 minutes. A poller for the live feeds
exists locally but is gitignored and stopped; restarting it is only worth it to measure
how many short stockouts the archive's spacing hides.

**Read before writing a spec.** Two published papers already estimate rebalancing by
differencing station snapshots against trips, Médard de Chardon & Caruso (2015) and Luo,
Sun, Kou & Cai (2026), and both have been read. Neither splits the difference into
operator moves, dropped rides and faults, which is what S1 must add. The 2015 paper found
Divvy's early feed full of station-level faults, so S1 tests for level oscillations that
no trip or truck explains. Bi, Ye & Zhu on classifying a bicycle's operational states
comes before S1, because the filter that separates operator moves from maintenance moves
and dropped rides depends on it. Xu, Yan, Goh & Jaillet on locational demand comes before
S2, as the nearest published relative of the demand model and of its
substitution treatment.

**Compute from the archive.** Build the snapshot coverage report, records per
station-day against the expected cadence. The published gap list is known to be
incomplete, so the study window is chosen against the computed report rather than
against the gaps anyone announced. From the same snapshots, date every station capacity
change: about 200 Chicago docks were moved after a 2016 dock-allocation study, and each
enlarged station inside the window is both a known intervention and a held-out test of
the demand model at hours the archive hid.

**Specify two estimators, not one.** S2 carries both the availability offset and a
likelihood written on the stockout time, fitted on the same reconstruction, and reports
the gap between them as a measurement.

**Check against outside numbers.** Report the teleport recovery rate against the 77 per
cent reference, and the Chicago moves-per-hundred-trips ratio against the verified
median of 6.9 from another city over 2016 to 2018. Both are brackets from elsewhere, not
targets to fit.

Three open requests that nothing waits on. Confirm the 2013 agreement's Schedule 20
thresholds against a clean copy, the public scans having no text layer and being read by
OCR. File one records request for the Schedule 21 monthly operator reports, naming the
full and empty instance list, the normal/full/empty percentages, the bikes rebalanced per
day and the technician visit counts. Those are the quantities S1 reconstructs, so a
release would be an independent check on the reconstruction rather than an input to it.
And ask Luo, Sun, Kou & Cai, who offer their data on request, for their Divvy 2018
rebalancing estimates, a bracket for S1. No gate waits on any answer, and a refusal is
recorded as an answer.

## Known blockers

- None. The two open requests above are not blocking: each has a stated fallback.
