<!-- faf: faf-agent-public | markdown | doc | Public About Repo for faf-agent — The Voice of FAF. Source code private at Wolfe-Jam/faf-agent. -->
<!-- faf: doc=readme | canonical=project.faf | family=FAF | private_source=Wolfe-Jam/faf-agent -->

# FAF Agent

[![FAF](https://mcpaas.live/badge/Wolfe-Jam/faf-agent-public.svg)](https://builder.faf.one)
[![IANA Registered](https://img.shields.io/badge/IANA-application%2Fvnd.faf%2Byaml-blue)](https://www.iana.org/assignments/media-types/application/vnd.faf+yaml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

> 📖 **Public About Repo** — this is the public face of [`Wolfe-Jam/faf-agent`](https://github.com/Wolfe-Jam/faf-agent) (source private). Spec, README, format docs, skill manifest, project.faf — no source code. Same shape as Anthropic's [`claude-code`](https://github.com/anthropics/claude-code) repo: public face, private engine.


**The Voice of FAF.**

A sensible, reliable agent that knows the most about the FAF format. It validates, scores, and answers questions about `.faf` and `.fafm` documents — always citing the spec or clearly refusing when a question is outside scope.

Exactly the size of FAF. No more, no less.

`.faf 🐘🎙️`

---

**What it does**
- Validates and scores .faf documents (faf-cli kernel; native in v0.2.0)
- Answers specification questions with citations
- Refuses out-of-scope questions cleanly
- Maintains voice-native memory on a namepoint

**What it does not do**
- General-purpose reasoning or coding
- Answer questions unrelated to the FAF format

**Install**
```bash
uvx faf-agent-mcp
```

**Usage**
```bash
# Via Claude Code, Cursor, or any MCP host
```

**License**
MIT
