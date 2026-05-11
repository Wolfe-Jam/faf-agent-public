# `.fafa` — FAF Agent Format Specification

**Version:** 0.1 (Draft)
**Status:** Draft — Spec in development
**MIME Type (proposed):** `application/vnd.fafa+yaml`
**File Extension:** `.fafa`
**IANA Registration:** Queued — to be submitted after `vnd.fafm+yaml` clears DE review
**Last Updated:** 2026-05-11
**Priority Date:** 2026-05-11 (this file's commit timestamp in Wolfe-Jam/faf-agent)

---

## 1. Purpose

`.fafa` is the **Agent Card** format of the FAF family. It defines a structured, vendor-neutral declaration of:

- **Identity** — who the agent is
- **Capabilities** — what the agent can do
- **Endpoints** — how the agent is reached
- **Attachment** — where the agent is located (optional)
- **Provenance** — what context the agent operates from (optional)

It is the agent-specific sibling of the IANA-registered `.faf` format (`application/vnd.faf+yaml`) and the in-review `.fafm` format (`application/vnd.fafm+yaml`).

**Position in the FAF family:**
- `.faf` carries **what the project IS** (Foundational Context Layer)
- `.fafm` carries **what the agent REMEMBERS** (Voice Memory Layer)
- **`.fafa` carries what the agent IS** (Agent identity + capability declaration)
- `.fafb` carries **the wire form** (Binary serialization)
- `.faf-grid` carries **diagnostic surface** (GRID)

---

## 2. Why this format exists

Multiple independent IETF and MCP-spec efforts in early 2026 converged on the same gap: every new agent-protocol draft defines an object model for agent identity + capability + endpoint, then defers the serialization format. No vendor media type exists for this domain.

Observed convergence (May 2026):
- `draft-dunbar-agent-attachment-01` (Futurewei + Oracle) — defines attachment properties; defers exposure format
- `draft-li-dmsc-macp-04` §5.1 (China Telecom et al.) — defines ACS (Agent Capability Specification); uses generic JSON
- `draft-sz-dmsc-iaip` (CAS) — defines capability profiles in TLV
- `draft-jimenez-agent-directory-00` (Ericsson) — uses generic `application/json` with JSON Schema
- MCP **SEP-1381** "Utilized Capability Declarations" — open question

`.fafa` provides a vendor-registered, YAML-native, FAF-family-coherent answer for this domain.

---

## 3. Core Concepts

| Term            | Definition |
|-----------------|----------|
| **Agent**       | An autonomous or semi-autonomous software entity that exposes capabilities and accepts invocation |
| **Capability**  | A named, addressable function the agent can perform — equivalent to MCP `tool`, A2A skill, or generic verb |
| **Endpoint**    | A reachable address + protocol binding for invoking the agent |
| **Attachment**  | (Optional) The infrastructure context within which the agent operates — gateway, domain, policy scope |
| **Provenance**  | (Optional) The context substrate the agent reads from — most commonly a `.faf` reference |

---

## 4. File Structure

A `.fafa` file is a single YAML document with the following top-level structure:

```yaml
version: "0.1"

agent:                       # REQUIRED — agent identity
  name: faf-agent
  id: "did:web:faf.one:agent"
  vendor: WolfeJAM
  version: "0.1.1"
  description: "The Voice of FAF — cites the spec or refuses out of scope without drama"

capabilities:                # REQUIRED — what the agent can do
  - name: validate
    type: tool
    description: "Validate .faf documents against the spec"
    input_schema: { ... }    # JSON Schema or equivalent
    output_schema: { ... }
    tags: [validation, faf, spec]

endpoints:                   # REQUIRED — how to reach the agent
  - protocol: mcp
    version: "2026-03"
    transport: stdio
    location: faf-agent-mcp
  - protocol: a2a
    version: "draft"
    transport: http
    location: "https://faf.one/agent/a2a"

attachment:                  # OPTIONAL — infrastructure context
  gateway: ...
  domain: ...
  policy: ...

provenance:                  # OPTIONAL — context substrate
  faf: "./project.faf"       # link to a .faf document
  spec: "application/vnd.faf+yaml"

signature:                   # OPTIONAL — provenance / integrity
  method: ...
  value: ...
```

**Required top-level fields:**
- `version`
- `agent` (with at least `name` and `id`)
- `capabilities` (array, may be empty if agent declares no public capabilities)
- `endpoints` (array, must be non-empty)

**Optional top-level fields:**
- `attachment`
- `provenance`
- `signature`
- `metadata` (free-form vendor extensions)

---

## 5. Agent Section

The `agent` object declares identity.

**Required subfields:**
- `name` — Unicode string, unique within the issuing vendor namespace
- `id` — Globally unique identifier (DID, URI, or vendor-scoped UUID)

**Optional subfields:**
- `vendor` — Issuing organization
- `version` — Semantic version of the agent (NOT of the .fafa spec)
- `description` — Short human-readable summary
- `homepage` — URL where the agent is documented
- `license` — SPDX identifier

---

## 6. Capabilities Section

Each capability is a structured object.

**Required subfields:**
- `name` — Unique within this `.fafa` document
- `type` — Capability category (`tool` | `resource` | `prompt` | `event` | other)

**Optional subfields:**
- `description` — Short human-readable summary
- `input_schema` — JSON Schema describing input parameters
- `output_schema` — JSON Schema describing return shape
- `tags` — Array of strings for filtering / discovery
- `apophatic` — Boolean: if true, the capability explicitly refuses out-of-scope invocation rather than attempting best-effort
- `cites_spec` — Reference to the format/spec the capability operates against (e.g., `application/vnd.faf+yaml`)

The `apophatic` and `cites_spec` fields are FAF-family conventions encoding the "Voice of FAF" discipline — capabilities that cite their spec or refuse cleanly.

---

## 7. Endpoints Section

Each endpoint declares a protocol binding.

**Required subfields:**
- `protocol` — Protocol identifier (`mcp`, `a2a`, `grpc`, `http`, or other)
- `transport` — Wire transport (`stdio`, `http`, `websocket`, `quic`)
- `location` — Reachable address — URI, package name, or transport-specific locator

**Optional subfields:**
- `version` — Protocol version
- `auth` — Authentication scheme reference
- `region` — Deployment region
- `health` — Health-check URI

Multiple endpoints MAY declare the same protocol with different transports.

---

## 8. Attachment Section (Optional)

The `attachment` object describes the infrastructure context within which the agent operates. It is aligned with the attachment-properties concept in `draft-dunbar-agent-attachment` but does not require any particular gateway protocol.

**Optional subfields:**
- `gateway` — Reference to the Agent Gateway (if any)
- `domain` — Administrative domain identifier
- `policy` — Policy attachment context
- `attachment_point` — Implementation-specific binding identifier

---

## 9. Provenance Section (Optional)

The `provenance` object cites the context substrate the agent operates from. This is the FAF-family integration point: a `.fafa` agent SHOULD declare which `.faf` (project context) it reads.

**Optional subfields:**
- `faf` — Reference to a `.faf` document (path, URL, or namepoint)
- `fafm` — Reference to a `.fafm` document (memory)
- `spec` — Media type of the substrate (`application/vnd.faf+yaml`, etc.)
- `version` — Version of the substrate referenced

---

## 10. Composition with Other Protocols

`.fafa` is **format-level**, not protocol-level. It composes with — does not replace — existing agent-interaction protocols:

| Protocol / Format | Relationship to `.fafa` |
|---|---|
| **MCP** (Model Context Protocol) | `.fafa` declares the agent; MCP is one possible endpoint binding |
| **A2A** (Agent-to-Agent) | `.fafa` is the format an Agent Card could be written in |
| **AAP** (`draft-dunbar-agent-attachment`) | `.fafa.attachment` carries AAP-style properties |
| **MACP** (`draft-li-dmsc-macp`) | `.fafa` is a candidate format for MACP's ACS (Agent Capability Specification) |
| **Agent Directory** (`draft-jimenez-agent-directory`) | `.fafa` is a candidate registration body for AD's capability declarations |
| **MCP Server Cards** (forthcoming) | `.fafa` is a candidate content format for MCP Server Cards |

`.fafa` follows the FAF doctrine: **"FAF defines. MD instructs. AI codes."** This format defines structure; instruction layers (CLAUDE.md, GEMINI.md, GROK.md) provide guidance; execution layers (MCP, A2A) carry out the work.

---

## 11. Security Considerations

`.fafa` carries identity claims and capability declarations. Implementations MUST:

- Validate `agent.id` against known identifier schemes
- Treat `endpoints` as untrusted until verified against `signature` if present
- Apply principle of least privilege when consuming declared capabilities — declaration is not authorization
- Treat input/output schemas as untrusted; validate at runtime
- Not auto-execute capabilities purely because they are declared

`.fafa` declarations MUST NOT contain:
- Secrets, API keys, or credentials
- User-personal data (use `.fafm` for that)
- Executable code

The format is **declarative**, not **executable**.

---

## 12. Interoperability Considerations

`.fafa` is YAML 1.2 / RFC 9512 compliant. Parsers MUST:
- Use YAML safe-load (no custom type construction)
- Limit alias expansion to prevent billion-laughs attacks
- Limit nesting depth to a reasonable bound (recommended ≤ 64)
- Treat unknown top-level fields as forward-compatible extensions
- Treat unknown subfields within `agent`, `capabilities`, `endpoints` as forward-compatible

`.fafa` files SHOULD use UTF-8 encoding and SHOULD be < 64KB to fit common registry storage budgets.

---

## 13. IANA Considerations (Forward-looking)

This specification anticipates a future IANA registration of:

- **Type name:** `application`
- **Subtype name:** `vnd.fafa+yaml`
- **File extension:** `.fafa`

Filing is **queued** behind the in-review `vnd.fafm+yaml` registration (RT #1451393). Submission is deliberately sequenced to:
1. Avoid overloading the IANA queue
2. Maintain a clean cadence with the Designated Expert
3. Preserve focus on the primary FAF family progression

This document establishes the priority date for the format design via its commit history.

---

## 14. Relationship Summary — the FAF family

| Format | Layer | Status | Use |
|---|---|---|---|
| `application/vnd.faf+yaml` | FCL — Foundational Context Layer | ✅ IANA-registered Oct 31, 2025 | What the project IS |
| `application/vnd.fafm+yaml` | VML — Voice Memory Layer | 🛰️ IANA DE review (RT #1451393) | What the agent REMEMBERS |
| **`application/vnd.fafa+yaml`** | **Agent Card — Agent identity + capabilities + endpoints** | **📋 Spec in development (this document)** | **What the agent IS** |
| `application/vnd.fafb` | Binary serialization | 🔒 Spec complete, held in reserve | Wire form |
| `application/vnd.faf-grid` | GRID — Diagnostic surface | 📋 Spec in development | Diagnostic / debugging surface |

Together: the FAF family defines a **complete substrate** for AI context — project, memory, agent, wire, diagnostic — all at the format level, none at the protocol level.

---

## 15. Versioning

`.fafa` follows semantic versioning:
- **0.x** — Draft. Format may change. NOT for production use yet.
- **1.0** — Stable. Backwards-compatible changes only in 1.x.
- **2.0** — Breaking changes only with strong cause.

The `version` field at document top-level declares which version of this spec the document conforms to.

---

## 16. Open Questions (v0.1)

These are deliberately deferred and will be resolved in successive drafts:

1. Should `id` allow multiple identifier schemes simultaneously?
2. Should `signature` be a single object or array (multiple signers)?
3. Should `capabilities` support hierarchical / grouped declaration?
4. Should there be a `lifecycle` field for create / start / stop / destroy semantics?
5. Should `provenance.faf` support content addressing (`fafb` hash) for tamper-evidence?
6. How does `.fafa` express degraded / partial capability (e.g., "available but rate-limited")?

These will be resolved before v1.0.

---

## 17. Reference Implementation

The reference implementation of `.fafa` consumption is `faf-agent-mcp` (PyPI: `faf-agent-mcp`, source: `Wolfe-Jam/faf-agent`). The agent reads its own `.fafa` declaration to advertise capabilities to MCP clients.

---

## 18. Provenance of this document

This specification is authored by James Wolfe (FAF Foundation). Initial draft committed to `Wolfe-Jam/faf-agent` (private repository) on the date in the header.

The convergence evidence cited in Section 2 was captured in the FAF intelligence canon at `/Users/wolfejam/PLANET-FAF/IETF/digest-a2a-aiproto-dmsc-2026-05-11.md`.

---

**Status: DRAFT — internal review only. Not for public distribution until v1.0.**
