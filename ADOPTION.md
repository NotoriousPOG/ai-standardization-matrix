# Adopting the matrix

## By project stage

### Spike / portfolio demo
- Aim: L1–L2 on C01, C04, C06, C08; honest labeling (C12)
- Skip claiming production authz (C03 L3) if the runtime is localhost-only
- Keep side effects off (C09)

### Internal automation (read-only)
- Gate: C01–C08 + C11 at L2
- Add C05/C07 if retrieval or investigation briefs are involved
- C12 citation + tool metrics before wide rollout

### Internal automation (writes)
- Everything above, plus C09 L2/L3 approval gates
- C03 L3 for anything beyond a single trusted operator
- Dual control for containment, payments, identity changes, or production deletes

### Customer-facing / multi-tenant
- All twelve controls at L2 minimum
- C02, C03, C10, C11 at L3
- C12 in CI with negative controls; judge metrics only after human-labeled calibration

## Rollout pattern (two weeks)

| Day | Focus |
| --- | --- |
| 1–2 | Scorecard current system; list L0/L1 gaps |
| 3–5 | Implement C01 + C04 + C06 validators |
| 6–7 | Add C08 budgets and cancel |
| 8–9 | Enforce C02 scope on tools; lock C11 |
| 10 | Human review path (C09) if missing |
| 11–12 | Retrieval cite-only (C05) and hygiene fields (C07) |
| 13–14 | Eval harness (C12); fix failures; freeze Standard bar |

## Anti-patterns this matrix rejects

1. **Prompt-only security** — “Don’t invent evidence” without a validator
2. **God-mode tools** — one HTTP/shell tool “for flexibility”
3. **Retrieval as permission** — runbook text authorizing containment
4. **Unbounded agents** — no turn/tool/time caps
5. **Silent memory** — model writes durable facts without a human save
6. **Demo laundering** — prepared playback marketed as evaluated live quality
7. **Average scores** — green dashboard while C03 or C06 remain L0

## Mapping to common stacks

| Stack piece | Primary controls |
| --- | --- |
| Structured outputs / Zod / JSON Schema | C01, C06, C07 |
| MCP servers / tool routers | C03, C04, C08 |
| Vector DB / BM25 / FTS corpora | C02, C05 |
| Orchestrators (LangGraph, custom loops) | C08, C09 |
| Observability / traces | C11, C12 |
| Human review UIs | C09, C10 |

## Relationship to NIST AI RMF

Use this matrix as an **engineering control set**. Map upward when you need governance language:

| RMF function | Matrix emphasis |
| --- | --- |
| Govern | Org policy that L2 is the release bar; ownership of scorecards |
| Map | C01–C07 (context, tools, evidence, reasoning) |
| Measure | C12 (evals, negatives, honesty about limits) |
| Manage | C08–C11 (bounds, humans, memory, secrets) |

For a full RMF playbook assessment in Cursor, see [nist-ai-rmf-skill](https://github.com/NotoriousPOG/nist-ai-rmf-skill).
