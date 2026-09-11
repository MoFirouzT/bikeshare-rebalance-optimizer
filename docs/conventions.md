# Conventions

House rules for this repository: how shared quantities are named and expressed, and
how committed documentation is written.

*Assumes: the operating contract in [CLAUDE.md](../CLAUDE.md).*

---

## 1. Domain conventions

<!-- Project-specific. Sign conventions, units, time zones, naming, configuration,
     logging. Whatever a reader must hold in their head to read the canonical doc
     without being misled. Delete this comment once filled in. -->

## 2. Writing charter (documentation quality)

The rules that keep the docs clear and consistent. The ones marked **lint** are
enforced by `scripts/lint_docs.py` in CI; the rest are review judgment. A line that
must break a per-line check, for instance to quote a banned word, can end with a
`<!-- lint-ok -->` comment, used sparingly.

This charter is this project's, not a standard. Delete what does not apply here and add
what does; what is not negotiable is that the charter exists, that a subset of it is
machine-checked, and that it says which.

### Word choice

**Use the most common word that is exactly as precise.** A less common word must earn
its place by carrying meaning the common one lacks. If it only sounds more considered,
it is costing the reader and buying nothing. This repo is read by people who did not
write it, including readers whose first language is not English.

Substitutions this project has actually made, kept as a worked example. The table is a
record, not a checklist: the rule is the test.

| Instead of | Write |
| --- | --- |
| | |

**lint: no coined `-able` or `-ability` words.** The commonest failure of the rule, and
the check uses the system word list. If English has no such adjective, write the verb
phrase instead: not *unimplementable* but *cannot be implemented*. <!-- lint-ok --> A real word the list
genuinely lacks goes in `REAL_ADJECTIVES`.

**Borrow a term, do not coin one.** When this project needs a name for a concept, take
the existing term from the field and pin its local meaning in the [glossary](glossary.md).
Repurposed common words need pinning most: they look like plain English and are not.

**Exempt: terms of art.** A word naming a code identifier, a symbol from the canonical
document, or a rule ID is used verbatim every time and never softened into a synonym.
The [glossary](glossary.md) is the sanctioned list; anything outside it can be
simplified.

**Prose about code must be checkable, or not written.** A sentence asserting a fact
about the code is not executable, so nothing fails when the code moves underneath it
and the claim rots invisibly. `scripts/lint_docs.py` therefore binds the checkable
subset: a spec marked `Implemented` must have no unrecorded box and must carry its
`Decisions` section; `Depends on:` IDs must name real specs and form an acyclic graph;
`file.md#anchor` links must resolve to a real heading; specs must carry no instruction
that was already carried out; and math must render on GitHub. The rule generalizes:
**if a doc claim about code cannot be checked mechanically, either make it checkable
or leave it out.** Counts, module paths restated from memory, and prose duplicating a
table are the usual offenders.

**Math renders through GitHub's Markdown first.** GitHub treats a backslash before
ASCII punctuation as a Markdown escape and drops the backslash before MathJax runs, so
spacing control symbols render as literal punctuation inside formulas. Control words
(`\quad`, `\sum`) are unaffected. Spacing is cosmetic: delete it rather than reach for
a workaround. Inline math must not start or end with a space, which can stop GitHub
parsing the span as math at all.

### Structure

1. **One source per claim.** A document is either a **source**, which owns the claims
   it makes, or a **derived reading**, which owns nothing, says so in a stamp at the
   top, and links the source behind every load-bearing sentence. A source never
   restates another source. A derived reading may restate anything, because the stamp
   says which copy wins. Decisions go in `docs/decisions/` (why); work orders and
   module docs describe what.
2. **Every canonical section follows the same fixed skeleton**, ending in a worked
   example tied to a golden oracle. The worked example is mandatory: it is how a stuck
   reader self-debugs by recomputing.
3. **Justify, do not assert.** Each non-obvious step earns its why; theory claims are
   pinned to a verified chapter, never cited from memory.
4. **Lead with the correctness trap.** State the failure mode before the mechanics.
5. **lint: every canonical doc opens with a purpose line and an `*Assumes:*` line.**
   Name what the doc takes as given and link there. Point to prerequisites; do not gate
   on the reader's background. A closing footer naming where the document's reasoning
   lives is good practice on top of this, not instead of it.
6. **No cold jargon.** Define or link any term or symbol at first use.
7. **If it is spatial, draw it.** One small SVG beats a dense paragraph.

### Prose mechanics

8. **lint: no em dashes.** Banned outright in committed docs, enforced per line. Use a
   colon, semicolon, comma, period, or parentheses, and vary the device so the prose
   does not settle into one substitute. A project with an archive it never rewrites
   should decline this rather than carry a rule it breaks everywhere.
9. **lint: one load-bearing claim per sentence; files stay under the line cap.** Push
   qualifiers into the next sentence rather than nesting three asides. Split a file
   that outgrows the cap: agents read whole files, and a file that has to be split is
   usually two subjects. A file that grows by design goes in `LINE_CAP_EXEMPT`.
10. **No filler or tics.** Skip "it is worth noting" and "in essence", do not reach for
    a rule-of-three list every time, do not hedge. Plain words over flourish.
11. **lint: no career or self-positioning language in committed files.** The gated
    words are listed in `scripts/lint_docs.py`; strategy stays in the gitignored
    `planning/` log.
12. **Reader confusion is data.** When someone trips on a passage, capture the fix as a
    glossary gotcha or an FAQ entry, not a one-off reply.
13. **Anti-patterns are explicit:** say what not to do. Agents and readers both follow
    that well.
