# Changelog

All notable changes to `faf-agent` will be documented in this file. This About Repo tracks the public-facing release history; internal commits live in the private source repo.

## [0.1.0] - 2026-05-04

### Added
- Initial release of FAF Agent — *The Voice of FAF*
- Format-grade MCP server: cites the FAF spec on every answer or refuses out-of-scope
- IANA-registered media type support: `application/vnd.faf+yaml`, `application/vnd.fafm+yaml`
- Dogfoods FAF format at TROPHY 100% (faf_version 2.5.2)
- Spec citations (15 spec-section pointers — extends to 30 in v0.2.0)
- Apophatic refusal templates for out-of-scope queries
- WJTTC test suite (Aero tier — sealed-artifact byte-equality; Tyres tier — live GitHub anchor verification)

### Format
- The agent is exactly the size of FAF. No more, no less.
- Bi-sync with FAF: same size, same growth, same substrate.
- See [`SPEC.md`](SPEC.md) for the architectural contract (PLAN-X v1.4, LOCKED).
- See [`Soul.md`](Soul.md) for the voice contract (v0.2.0, LOCKED).

---

`.faf 🐘🎙️`
