# CLAUDE.md - operating contract for this repo

Built under spec-first discipline, one phase at a time. Read this file, then
`docs/STATE.md`, at the start of every session.

The method is the `project-discipline` skill: the rules, the document tiers, the
phase workflow, the decision-record bar, and the working preferences. It is not
repeated here. This file holds only what is specific to this project, and the skill's
rules apply in full except where `## Overrides` below says otherwise.

## 1. Math discipline (highest priority)

- The math is defined in `docs/formulation.md`, the single source of truth.
  NEVER change what it defines without (1) updating `docs/formulation.md` in the same
  change and (2) updating or adding a golden test.
- Before editing `src/bikeshare/`, restate the relevant clause from
  `docs/formulation.md` and the invariant it must satisfy.
- **The correctness trap:** fitting the demand model on observed rentals. Pickups are
  observed only while a bike is present, so a station that stood empty records zero
  pickups for that window and reads as quiet. The availability offset is what separates
  demand from rentals; drop it, mis-sign it, or compute it over the wrong window and the
  intensities stay plausible while being wrong worst at exactly the stations the
  operator is trying to fix. Every price, policy and result downstream inherits the bias.
- The gates are `tests/golden/` (golden) and `tests/property/` (property). A
  failing gate means the spec or the code is wrong: surface it, do not suppress it.

## 2. Where things live

- **Tier 0 - `planning/` (GITIGNORED, never commit):** the plan, unbuilt roadmaps, and
  private reasoning. Read it for context; never copy its framing into a committed file.
- **Tier 1:** `README.md`, `docs/architecture.md`.
- **Tier 2:** `docs/formulation.md`, `docs/conventions.md`, `docs/glossary.md`,
  `docs/decisions/`, `docs/studies/`.
- **Tier 3:** `docs/specs/<subject>.md`, one per phase; `docs/specs/README.md` is the
  ledger.

**Governing rule:** the release plan stays in `planning/`. Which releases exist, which
one is allowed to end the project, which is dropped first, and how the write-up is
framed are decisions about the project rather than engineering in it. A committed file
describes what this project does and what it found, never how far it intends to go.
The gated words are in `scripts/lint_docs.py`.

**Writing:** committed docs follow `docs/conventions.md`, which is this project's own
charter and says which of its rules are machine-checked. Read it before writing docs.

**Substantial edits are rewrites, not annotations.** When a change alters what a section
means, rewrite the whole section so it reads as though it had always said that, and put
the reasoning in `docs/decisions/` (or, in Tier 0, the plan's own decisions section).
Never leave the old reading in place next to the new one. This applies to
`planning/` as much as to committed docs, and `scripts/lint_docs.py` checks the phrasing
in both.

Two triggers, either one is enough:

- A section grows by more than about half while being edited. Stop and rewrite it whole
  before moving on; do not leave it for later, because later is a separate task nobody
  scheduled.
- An edit adds a sentence about what the document used to say. That sentence is the
  reasoning, and the reasoning goes to the decision record.

**Clarity outranks brevity.** Aim first at the message a reader who has seen none of the
surrounding work can take on one pass, then compress it. Never compress first and clarify
after: the author can still read their own meaning into broken text, so the break
survives. A short sentence that has to be asked about was not brief, it was incomplete.

## 3. Layering

Dependencies run one way, from data upward. A module never imports from a layer above
it.

| Layer | Package | May import |
| --- | --- | --- |
| 0 reconstruction | `bikeshare.data` | nothing in `bikeshare` |
| 1 censored demand | `bikeshare.demand` | layer 0 |
| 2 network price | `bikeshare.price` | layers 0 to 1 |
| 3 decision model | `bikeshare.decide` | layers 0 to 2 |
| 4 simulator and harness | `bikeshare.sim` | layers 0 to 1, and a policy interface |
| 5 agent and service | `bikeshare.agent` | layers 0 to 4 |

`bikeshare.sim` takes policies through an interface rather than importing
`bikeshare.decide`, so the model-free baselines run before the decision model exists.
No automated check enforces this yet; add one when the packages exist.

## 4. Commands

- env: `uv sync`
- run: `uv run python -m bikeshare.<module>`
- lint: `uv run ruff check .`
- types: `uv run mypy src`
- test: `uv run pytest`
- docs: `uv run python scripts/lint_docs.py` (writing charter)

## 5. Overrides

Rules from the `project-discipline` skill that this project deliberately does not
follow, each with the reason. This section is the only supported way to turn one off:
a rule that is silently ignored looks identical to a rule that was forgotten.

- None yet.

## 6. Guardrails (known failure modes)

- Do not code against a remote API schema from memory. Fetch and print a real sample
  first, then write code against the actual shape.
- Do not over-build. The spec's "out of scope" section is binding.
- After any refactor of `src/bikeshare/`, re-run the golden tests: they catch silent
  changes to meaning.
