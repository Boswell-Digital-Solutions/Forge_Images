# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ForgeImages is a template-driven image asset pipeline for the Forge ecosystem. It provides deterministic, auditable image asset generation: AI agents may generate and select candidates, but all validation and compilation must pass through ForgeImages' template-defined rules — no bypasses. The repo has two parts: a Rust core engine/CLI (`forgeimages-core`) and a Python FastAPI bridge + agent skill (`forgeagents-forgeimages`).

## Common Commands

- `cd forgeimages-core && cargo build --release` — build the Rust core
- `cd forgeimages-core && cargo test` — run all Rust tests
- `cd forgeimages-core && cargo test --test invariants` — run the 6 contract invariant tests only
- `cd forgeagents-forgeimages && pip install -e . && uvicorn bridge.forgeimages_bridge:app --host 127.0.0.1 --port 8100` — start the bridge service
- `cd forgeagents-forgeimages && pytest tests/ -v` — run the Python (bridge/skill boundary) tests
- CLI: `forgeimages-cli templates|validate|compile --templates-dir ./templates ...`

## Architecture

- `forgeimages-core/` (Rust 2024 edition): `lib.rs` (Six Laws + public API), `templates.rs` (template contracts/registry), `validation.rs` (rules + failure-mode policy, kept separate), `hashing.rs` (SHA-256 canonical-JSON manifests), `print.rs` (PrintAuthority enum), `pipeline.rs` (compile always validates), `bin/forgeimages_cli.rs`.
- `forgeagents-forgeimages/` (Python 3.10+): `bridge/` — FastAPI HTTP gateway (5 endpoints) plus append-only JSONL `audit.py`; `skill/` — async `httpx` agent skill; `tests/` — boundary tests.
- Data flow / trust boundary: Agent → Skill (httpx) → Bridge (FastAPI) → CLI (subprocess) → Core (Rust), with every call audit-logged. No file paths ever cross the trust boundary — agents send/receive base64 only.

## Notes

- `compile_asset()` always calls `validate_asset()` first — there is no code path that bypasses this. HTTP 422 / CLI exit code 2 both mean validation failure, and agents cannot proceed past either.
- The audit log is append-only — never modify, truncate, or delete entries.
- Templates are contracts: old templates must keep working forever. Bump the template version for a behavior change; never modify a template in place.
- Agents can generate/select candidates, request compilation, and list templates. Agents cannot skip validation, override templates, write files directly, or change the failure mode (Block/Warn/Log).
