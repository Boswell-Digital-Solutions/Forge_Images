# ForgeImages — Compiled System Reference

**Designation:** IMA
**Document role:** Canonical compiled technical reference for the ForgeImages template-driven image asset pipeline
**Source:** `doc/system/`
**Build command:** `bash doc/system/BUILD.sh`
**Document version:** 2.0 (2026-11-14) — deduplication pass. Removed three bootstrap-generated files (`00-overview.md`, `01-architecture.md`, `20-structure.md`) that were byte-for-byte duplicates of `01-overview-philosophy.md`, `02-architecture.md`, and `04-project-structure.md`; removed three never-authored placeholder stubs (`10-scope.md`, `30-governance.md`, `40-change-control.md` — each still read "Replace this placeholder with real authored content"); rewrote `90-appendices.md`, which previously existed only to list the real chapters as "unmapped"; added the Designation/Source/Build-command markers and this generated-artifact warning; made `BUILD.sh` fail-closed. See `docs/KNOWN_ISSUES.md` for the open gap this leaves (no authored scope/governance/change-control chapter).
**Protocol:** BDS Documentation Protocol v2.0

> **Generated artifact warning:** `doc/IMASYSTEM.md` is assembled output. Edit
> the source modules under `doc/system/` and rebuild. Hand edits to the
> compiled artifact are overwritten by the next build.

This `doc/system/` tree is the canonical source of truth for ForgeImages. It
uses explicit **truth classes**: *canonical facts* define the Six Laws, the
trust boundary, and template-immutability (chapters §1-§2); *snapshot facts*
are dated, audit-derived counts and current implementation inventory
(chapters §3-§11) that may drift between audits.

Assembly contract:

- Command: `bash doc/system/BUILD.sh`
- Validation: `bash doc/system/validate_snapshots.sh` runs during assembly
- Primary output: `doc/IMASYSTEM.md`

| Part | File | Contents |
| --- | --- | --- |
| §1 | `01-overview-philosophy.md` | Service identity, the Six Laws, core principle, ecosystem role |
| §2 | `02-architecture.md` | Three-tier trust boundary, data flow, PrintAuthority model, template contracts |
| §3 | `03-tech-stack.md` | Rust core engine, Python bridge + skill, build tools, feature flags |
| §4 | `04-project-structure.md` | Directory tree, key module responsibilities |
| §5 | `05-config-env.md` | Configuration and environment variables |
| §6 | `06-api-layer.md` | Bridge HTTP endpoints |
| §7 | `07-backend-internals.md` | Core engine internals |
| §8 | `08-ecosystem-integration.md` | Forge ecosystem integration surface |
| §9 | `09-error-handling.md` | Validation failure modes, exit codes, HTTP statuses |
| §10 | `10-testing.md` | Rust and Python test commands and coverage |
| §11 | `11-handover.md` | Binary paths, health check, example calls |
| §12 | `90-appendices.md` | Glossary and cross-references |

## Quick Assembly

```bash
bash doc/system/BUILD.sh
```
