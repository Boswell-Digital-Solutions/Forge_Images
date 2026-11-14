# §12 — Appendices

**Document version:** 2.0 (2026-11-14) — replaces the stale "unmapped legacy chapters" carry-forward
note. That list named headings from §3, §5-§11 as unplaceable; those chapters were never missing,
only left out of `_index.md`'s own chapter table by the bootstrap registry pass. They are now §3
through §11 above.

## Glossary

- **Designation:** `IMA` — the three-letter code this repo registers under in the Forge ecosystem
  registry, used for the compiled artifact name (`doc/IMASYSTEM.md`).
- **Six Laws:** the non-negotiable invariants enforced in `forgeimages-core/src/lib.rs` — see §1.
- **PrintAuthority:** the enum in `forgeimages-core/src/print.rs` governing which surface may
  authorize a physical print (see §2).
- **Trust boundary:** the Agent → Skill → Bridge → CLI → Core chain described in §2; no file path
  ever crosses it, only base64-encoded bytes.

## Cross-references

- Rust core engine and CLI: `forgeimages-core/`.
- Python bridge + agent skill: `forgeagents-forgeimages/`.
- Audit log: append-only JSONL, never modified, truncated, or deleted (`forgeagents-forgeimages/bridge/audit.py`).
- Repo-level guidance: `CLAUDE.md`.
