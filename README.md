<!-- faf: faf-agent-public | markdown | doc | Public About Repo for faf-agent — The Voice of FAF. Source code private at Wolfe-Jam/faf-agent. -->
<!-- faf: doc=readme | canonical=project.faf | family=FAF | private_source=Wolfe-Jam/faf-agent -->

# FAF Agent

[![FAF](https://mcpaas.live/badge/Wolfe-Jam/faf-agent-public.svg)](https://builder.faf.one)
[![IANA: vnd.fafa+yaml](https://img.shields.io/badge/IANA-vnd.fafa%2Byaml-00D4D4)](https://www.iana.org/assignments/media-types/application/vnd.fafa+yaml)
[![DOI: Agents paper](https://img.shields.io/badge/DOI-Agents%20paper-FF6B35)](https://doi.org/10.5281/zenodo.21951641)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

> 📖 **Public About Repo** — this is the public face of [`Wolfe-Jam/faf-agent`](https://github.com/Wolfe-Jam/faf-agent) (source private). Spec, README, format docs, skill manifest, project.faf — no source code. Same shape as Anthropic's [`claude-code`](https://github.com/anthropics/claude-code) repo: public face, private engine.


**The Voice of FAF.**

A sensible, reliable agent that knows the most about the FAF format. It validates, scores, and answers questions about `.faf`, `.fafm`, and `.fafa` documents — always citing the spec or clearly refusing when a question is outside scope.

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

**Home:** [faf.one/agent](https://faf.one/agent) · **Passport:** [faf.one/.well-known/fafa](https://faf.one/.well-known/fafa) · **Spec:** [AGENT-FORMAT.md](AGENT-FORMAT.md)

## Citation

If you use `faf-agent`, `faf-agent-mcp`, or the `.fafa` format in research or production, please cite the format paper:

> Wolfe, J. (2026). *Why Agents Need a Passport: .fafa — Portable Identity for the Agentic Era*. Zenodo. https://doi.org/10.5281/zenodo.21951641

Companion papers: [Context](https://doi.org/10.5281/zenodo.18251362) (`.faf`) · [Memory](https://doi.org/10.5281/zenodo.20348942) (`.fafm`).

### BibTeX

```bibtex
@article{wolfe2026fafa,
  title     = {Why Agents Need a Passport: .fafa — Portable Identity for the Agentic Era},
  author    = {Wolfe, James},
  year      = {2026},
  month     = {aug},
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.21951641},
  url       = {https://doi.org/10.5281/zenodo.21951641}
}
```

**License**
MIT

If `faf-agent` has been useful, consider starring the repo — it helps others find it.
