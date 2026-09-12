# AI Standardization Matrix

A practical standard for building **AI automation** that stays inspectable, scoped, and reviewable.

Derived from UNN.DEV AI automation engineering patterns: schema validation at trust boundaries, allowlisted tools, scoped retrieval, cited outputs, bounded agent loops, and human review before action.

This is not a compliance certificate. It is a build standard: clear controls, maturity levels, and verification signals you can apply to any agent, RAG workflow, or automation loop.

## Why this exists

Most AI automation fails the same ways:

- The model invents evidence, IDs, or tool results
- Tools inherit more authority than the caller
- Retrieval text is treated as permission
- Loops run without budgets
- Drafts ship as decisions
- “It worked in the demo” substitutes for evaluation

The matrix turns those failure modes into **required controls**.

## Quick start

1. Read [MATRIX.md](MATRIX.md) — twelve control domains × four maturity levels
2. Score your system with [templates/scorecard.md](templates/scorecard.md)
3. Use [CHECKLIST.md](CHECKLIST.md) during design and PR review
4. Import [data/matrix.json](data/matrix.json) if you want machine-readable gates

**Target bar for production automation:** every control at **Standard (L2)** or higher. **Hardened (L3)** for anything that can change state, spend money, or touch production security systems.

## The twelve domains

| ID | Domain | One-line rule |
| --- | --- | --- |
| C01 | Ingress & schema | Validate untrusted input with strict schemas before the model sees it |
| C02 | Scope & tenancy | Bind work to tenant, time, and entity scope; reject out-of-scope data |
| C03 | Auth & identity | Authority comes from the authenticated session, never from model text |
| C04 | Tool contracts | Allowlist tools; validate arguments and results; no open shell/HTTP by default |
| C05 | Retrieval & RAG | Search approved corpora only; cite only what this run retrieved |
| C06 | Evidence integrity | Findings must reference known evidence/entity IDs; invent nothing |
| C07 | Reasoning hygiene | Separate facts, hypotheses, alternatives, and missing evidence |
| C08 | Agent bounds | Cap turns, tools, repairs, and wall time; stop identical loops |
| C09 | Human control | Default action is a reviewable draft; high-impact acts need a person |
| C10 | Memory | Explicit save only; label untrusted; isolate by tenant; cap retention |
| C11 | Secrets & data hygiene | No credentials in repos, exports, or browser storage of provider keys |
| C12 | Evaluation & honesty | Test citations, tools, and negatives; label demo vs live; state limits |

## Maturity levels

| Level | Name | Meaning |
| --- | --- | --- |
| L0 | Absent | Control missing or easily bypassed |
| L1 | Basic | Present for the happy path |
| L2 | Standard | Enforced in code, with tests or evals |
| L3 | Hardened | Production-grade: authz, budgets, audit, negative controls, repair limits |

## Pattern origins (examples behind each control)

| Pattern | Matrix control |
| --- | --- |
| Strict alert/incident/report schemas | C01, C06 |
| Tenant + time window + dedup key | C02 |
| Short-lived caller token for tools | C03 |
| MCP / plugin dispatcher with I/O contracts | C04 |
| Approved, tenant-filtered runbook search | C05 |
| Evidence/knowledge ID validation on finish | C06 |
| Hypotheses marked unverified + alternatives | C07 |
| Max rounds, tool calls, repairs, timeout | C08 |
| Analyst inbox / draft before approval | C09 |
| Explicit analyst notes, not auto-memory | C10 |
| Runtime-only provider keys; secret scanning | C11 |
| Citation + tool + negative-control evals | C12 |

See [ADOPTION.md](ADOPTION.md) for rollout by project stage.

## Suggested use

- **Design:** pick L2 as the default acceptance bar before writing agent code
- **PR review:** attach the checklist; block merge on C01–C08 + C11 at L2 for any live loop
- **Portfolio / hiring demos:** keep L2 for simulated paths; do not claim L3 without production authz
- **Audits:** export the scorecard and link failing controls to tickets

## Related work

- [nist-ai-rmf-skill](https://github.com/NotoriousPOG/nist-ai-rmf-skill) — NIST AI RMF assessment skill for Cursor
- Optional alignment notes: Map ≈ C01–C07, Measure ≈ C12, Manage ≈ C08–C11, Govern ≈ org policy wrapping this matrix

## License

MIT. Attribution appreciated; fork and adapt freely for your team’s automation standard.
