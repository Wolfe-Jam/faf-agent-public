<!-- faf: faf-agent | Python | mcp-server | The Voice of FAF — format-grade MCP -->
<!-- faf: claim=project.faf | score=100 | family=FAF | siblings=README.md,Soul.md,SPEC.md | iana=vnd.faf+yaml,vnd.fafm+yaml -->

# CLAUDE.md — faf-agent v0.1.0

## What This Is

MCP server for FAF — cites the spec on every answer or refuses out-of-scope. IANA-registered formats: `application/vnd.faf+yaml`, `application/vnd.fafm+yaml`. Dogfoods FAF format at TROPHY 100% (faf_version 2.5.2).

## Architecture

```
.approval-packets/v0.1.0-setup/  ← Keystone receipt (LOCKED 2026-05-04, 13 docs)
Soul.md                          ← Voice contract v0.2.0 (LOCKED)
SPEC.md                          ← PLAN-X v1.4 architectural contract (LOCKED)
project.faf                      ← FCL slot data (faf_version 2.5.2, TROPHY 100%)
README.md                        ← Apophatic register; byte-sealed from packet
LICENSE                          ← MIT
src/
└── citations.json               ← 15 spec-section pointers (Day 2-3 extends to 30)
tests/
├── test_repo_integrity.py       ← Aero tier — sealed-artifact byte-equality
└── test_citations_resolve.py    ← Tyres tier — live GitHub anchor verification
skill/                           ← Empty until Day 14 (FAF Skill manifest)
```

Layer order: `Soul.md (voice) → SPEC.md (architecture) → project.faf (slots) → src/ + tests/`. Test Shadows discipline: every artifact ships with its test in the same commit.

## Toolchain

- **Score:** `bunx faf score` → must report `🏆 TROPHY 100%`
- **Validate:** `bunx faf check`
- **Test (current):** `python3 tests/test_repo_integrity.py` · `python3 tests/test_citations_resolve.py`
- **Future (Day 6-7):** `uv init` + FastMCP server + `pytest -m wjttc`
- **Publish (Day 15):** `uv publish` via `/pubpypi` skill

## Key Patterns

- **Test Shadows** — `tests/test_<artifact>.py` ships with each artifact in same commit; no monolithic megafile.
- **WJTTC 4-tier** — `🛡️ Brake` (refusals) · `⚙️ Engine` (correctness) · `🌀 Aero` (polish) · `🛞 Tyres` (live probes). Pytest markers, not separate files. `pytest -m wjttc` aggregates.
- **Citation-or-silence** — every answer cites the spec or returns the "I don't know" refusal template (per Soul.md voice grammar).
- **Apophatic guard** (Day 9) — keyword + `rapidfuzz` pattern-match against `src/citations.json`; refusal template on no-match.
- **Rust IP via WASM bridge** — `fafb` (Mk4 scoring) + `faf-rust-sdk` (FAFb spec) consumed via `wasmtime`. Same bridge `faf-cli` uses.
- **Sealed contracts** — `Soul.md` + `SPEC.md` + `README.md` are byte-equal to `.approval-packets/v0.1.0-setup/` copies; `test_repo_integrity.py` asserts this on every commit.

## Commands (planned, Day 6-7)

6 FastMCP tools (no more — exactly the size of FAF):
- `validate_faf` / `score_faf` — deterministic, Rust IP via WASM
- `ask` / `cite` — RAG-backed via FAF-RAG; citation-mandatory
- `etch_memory` / `recall_memory` — forward to `mcpaas.live/mcp`

## State

- Branch: `main` · 5+ commits · `--private` repo (flips public Day 4-5 per Trojan Horse posture)
- Phase: setup foundation LOCKED; Day 1 done; Day 4-5 done (project.faf TROPHY 100%)
- Pattern source: `~/FAF/gemini-faf-mcp/` (Python FastMCP, PyPI v2.2.1)
- Blocked: faf-cli #61 (v3.0 schema scoring regression — workaround: faf_version 2.5.2)
- Target: v0.1.0 PyPI ship at Day 15
