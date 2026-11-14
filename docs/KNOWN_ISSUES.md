# Known Issues

This document tracks active limitations and historical fixes. Blocking impact is stated per item; an old resolved entry is not a substitute for current source verification.

## KI-IMA-20261114-001 — `doc/system/` carried duplicate and dead content across three documentation-protocol generations; no authored scope/governance/change-control chapter exists

**Status:** OPEN (one real gap remains; the rest is fixed in the same change as this entry).

**What was wrong.** `doc/system/` held content from three successive, never-reconciled documentation-protocol
passes:

1. The oldest (2026-03-06, "Forge Documentation Protocol v1", designation `fi`) left three byte-identical
   dead compiled artifacts on disk: root `SYSTEM.md`, `doc/SYSTEM.md`, and `doc/fiSYSTEM.md`. None was
   produced by the current `doc/system/BUILD.sh` (which only ever writes `doc/IMASYSTEM.md`), so all three
   were stale the moment the designation changed to `IMA`. `README.md`'s "Deep reference" line pointed
   readers at `doc/fiSYSTEM.md` — the dead one — instead of the live artifact.
2. A later "registry remediation" bootstrap pass (`doc/system/_index.md`, version "1.0 (bootstrap)") added
   a 7-file "class-aware" skeleton (`00-overview.md`, `01-architecture.md`, `10-scope.md`, `20-structure.md`,
   `30-governance.md`, `40-change-control.md`, `90-appendices.md`) without reconciling it against the
   pre-existing, real 11-chapter set (`01-overview-philosophy.md` through `11-handover.md`). Three of the
   new files (`00-overview.md`, `01-architecture.md`, `20-structure.md`) were byte-for-byte duplicates of
   `01-overview-philosophy.md`, `02-architecture.md`, and `04-project-structure.md` respectively. Three more
   (`10-scope.md`, `30-governance.md`, `40-change-control.md`) were never-filled placeholders, each
   verbatim: "This chapter is a registry-generated bootstrap scaffold ... Replace this placeholder with
   real authored content." `90-appendices.md` existed only to list the real chapters' section headings as
   "Unmapped legacy chapters ... could not be deterministically mapped ... Review and place them by hand" —
   never done. `_index.md`'s own chapter table listed only the 7 bootstrap files, so it did not describe
   what `BUILD.sh` actually assembled: `BUILD.sh` globs every `[0-9][0-9]-*.md` file regardless of whether
   `_index.md` names it, so the compiled `doc/IMASYSTEM.md` contained **every** real chapter plus its exact
   duplicate, each claiming the same `§1`/`§2` heading twice.
3. `BUILD.sh` also gated calling `validate_snapshots.sh` on `[ -x "$VALIDATOR" ]` (the executable bit)
   rather than on the file existing. `validate_snapshots.sh` was never `chmod +x`'d, so the validator was
   **silently skipped on every build** — the same latent bug found the same day in `eval-cal-node`'s
   doc tooling, from what looks like the same bootstrap generator.

**Cause.** Two unrelated documentation-tooling generations (the 2026-03-06 `fi`-designated pass and the
later registry-remediation bootstrap pass) each ran once, wrote their own output, and never cleaned up or
reconciled against what the previous generation had already written. Nothing re-verifies that `_index.md`'s
chapter table matches what `BUILD.sh` actually globs, so the mismatch was invisible until read by hand.

**Fix, same change as this entry.** Deleted the three dead compiled artifacts and fixed `README.md`'s
reference. Deleted the three exact-duplicate skeleton files and the three never-filled placeholder stubs.
Rewrote `90-appendices.md` to a real appendices chapter (glossary + cross-references) instead of the stale
"unmapped" list. Rewrote `_index.md`'s chapter table to name all 11 real chapters plus appendices (§1-§12),
and added the `**Designation:**`/`**Source:**`/`**Build command:**`/generated-artifact-warning/truth-classes
markers `scripts/audit_doc_system_shape.py` (in the `forge` workspace root) checks for. Made `BUILD.sh`
fail-closed (designation/output match check, `BUILD_OK` on success) and fixed the validator gate to check
`-f` instead of `-x`. `bash doc/system/BUILD.sh` now reports `BUILD_OK ... parts=12`, and the compiled
artifact has each of §1-§12 exactly once.

**Open.** No scope, governance, or change-control chapter exists for this repo today — the three
placeholder stubs that would have held them were deleted rather than backfilled, since authoring real
governance/authority-boundary claims is a different kind of change than a documentation-structure cleanup,
and the bootstrap stub's own text ("Registry will not invent repo truth that is not already present in the
repo") is the right instinct not to fabricate one. `CLAUDE.md`'s "Notes" section states some of the
relevant invariants already (the Six Laws, template immutability, the append-only audit log, what agents
cannot do) — a future change authoring a real governance chapter from those stated facts, reviewed by the
repo owner, would close this gap properly.

**Scope:** Open (the governance/scope/change-control chapter gap only; the duplication and dead-artifact
problem above is fixed).
