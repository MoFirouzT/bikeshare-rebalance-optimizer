# Phase specs and the ledger

Each phase of this project was built from a frozen spec in this directory:
scope, interfaces, and the test contract, reviewed before any code was written. This
file indexes them and records what each one concluded.

*Assumes: the capability map in [architecture.md](../architecture.md); the workflow in
[CLAUDE.md](../../CLAUDE.md) §3.*

---

## The phase and the label

The unit of work here is the **phase**. Delivery labels, if this project uses them,
record the order things were built and are not architecture; reader-facing documents
name capabilities instead.

Filenames name their subject, so a capability delivered over several phases has one
spec, and a capability whose later phases answered a different question has several.

| Capability | Phase specs | Labels | Packages |
| --- | --- | --- | --- |
| | | | |

## In flight

Approved-but-unbuilt and draft specs are listed separately because they carry no
ledger row: a row records what was found, and an unbuilt phase has found nothing.

| Spec | Label | Status | What it proposes |
| --- | --- | --- | --- |
| | | | |

---

## The ledger

One row per phase, oldest first. Dates are the implementing commit's date. The
**finding** column is the point: what it established, including when the answer was
"no". This ledger is the project's history, which is why the decision records do not
have to be.

| Phase | Date | Capability | What changed | Finding |
| --- | --- | --- | --- | --- |
| | | | | |

---

## Where the detail lives

This ledger is deliberately one line each. Every phase's design detail, its
resolved decision trail, and what it ruled out are in its own spec, under
**Decisions**, which is where a reader should go for the reasoning.

## Adding one

1. Copy [`_TEMPLATE.md`](_TEMPLATE.md); fill scope, interfaces, and the test contract.
   Add it to **In flight** above, which is where a spec lives until it is green.
2. Human review and approval, before any implementation.
3. Write the golden and property tests first, failing.
4. Implement to green.
5. Move the spec from **In flight** to a ledger row, and update
   [STATE.md](../STATE.md).

Resolve each open question in place, keeping the proposal, so the section becomes the
decision trail rather than a list of questions that were answered elsewhere.
