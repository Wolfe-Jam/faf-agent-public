---
name: faf-agent
description: The Voice of FAF — format-grade MCP that cites the FAF spec on every answer or refuses out-of-scope. Use when you need authoritative answers about the .faf format, scoring, IANA registrations, or the FAF Family suite (.faf / .fafm / .fafb). Each successful response carries a citation; out-of-scope queries return a refusal template instead of speculation.
license: MIT
---

<!-- faf: faf-agent | Python | mcp-server | The Voice of FAF — format-grade MCP -->
<!-- faf: doc=skill | family=FAF | related=faf-expert,faf-init,faf-score | canonical=Soul.md -->

# faf-agent — The Voice of FAF 🐘🎙️

A format-grade MCP that answers questions about FAF or refuses out-of-scope. Every successful answer cites the spec; every refusal uses one of four canonical templates. No speculation, no marketing.

Bi-sync with FAF: same size, same growth, same substrate. Reference impl for the apophatic register.

## When to use this skill

- Questions about `.faf` format, slots, scoring, validation
- Questions about FAF Family members: `.fafm` (Voice Memory Layer), `.fafb` (Binary)
- IANA-registered media types: `application/vnd.faf+yaml`, `application/vnd.fafm+yaml`
- FAF Skill manifests, FAF Foundation Layer (FCL), FAF spec interpretation
- When you want a citation alongside the answer (or a refusal instead of a guess)

Don't use this skill for general programming, opinions on tools outside FAF, or anything that needs a non-FAF answer. The skill will refuse cleanly — that's the contract.

## The six tools

| Tool | Purpose |
|---|---|
| `validate_faf` ⌚ | Schema validation — deferred to v0.2.0; use `bunx faf-cli check <path>` today |
| `score_faf` ⌚ | Mk4 scoring — deferred to v0.2.0; use `bunx faf-cli score <path>` today |
| `ask` | Answer a FAF question with mandatory citation. Refuses if RAG can't find a match. |
| `cite` | Return the spec section for a topic. Refuses if no canonical citation exists. |
| `etch_memory` | Write a memory entry to a namepoint via mcpaas.live |
| `recall_memory` | Read the soul body for a namepoint via mcpaas.live |

Six tools. The size of FAF.

**⌚ marker:** v0.1.x returns a structured `{"status": "not_implemented", "available_in": "v0.2.0", "use_instead": "..."}` response from `validate_faf` and `score_faf`. The Rust/WASM kernel that powers `faf-cli`'s same-name commands is wiring through to this package in v0.2.0. Until then, faf-cli ships the deterministic Mk4 work.

## Citation discipline

Per `Soul.md` v0.2.0:

> Every answer ships with a deep link to a canonical FAF source — `SPECIFICATION.md`, `MEMORY-FORMAT.md`, `BINARY-FORMAT.md`, IANA registry pages, `skills.faf.one`, or public examples in the FAF GitHub org.
>
> If I can't cite, I don't claim.

The four refusal templates: `out_of_scope`, `below_confidence`, `not_in_corpus`, `asked_for_opinion`.

## Install

```bash
pip install faf-agent-mcp
```

Add to your MCP client config:

```json
{
  "mcpServers": {
    "faf-agent": {
      "command": "python",
      "args": ["-m", "faf_agent_mcp.server"]
    }
  }
}
```

Optional environment variables:

- `XAI_API_KEY` — enables real RAG via xAI Grok Collections (Mock client used when unset)
- `MCPAAS_API_KEY` — enables `etch_memory` / `recall_memory` against mcpaas.live (read-only without)

## The FAF Family

| Format | Layer | Purpose | Status |
|---|---|---|---|
| `.faf` | FCL — Foundational Context Layer | Project context, AI-readable | IANA registered |
| `.fafm` | VML — Voice Memory Layer | Persistent voice memory | IANA registration in progress |
| `.fafb` | Binary | Compact wire format | IETF queued |

## Resources

- Spec: https://github.com/Wolfe-Jam/faf
- Brand front: https://faf.one
- Skills index: https://skills.faf.one
- Mark: 🐘🎙️ (family mark — present on every faf-agent surface)
