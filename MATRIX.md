# AI Automation Standardization Matrix

Controls are rows. Maturity is columns. Score each control at the **highest level you can prove** with code, tests, or evals — not with intent.

Legend: **L0 Absent · L1 Basic · L2 Standard · L3 Hardened**

---

## C01 — Ingress & schema

Validate every untrusted boundary with a strict schema before model context is built.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Free-form JSON/text enters the agent | Schema exists for the happy path | Strict schema (reject unknown fields); applied on every ingress path | Schema + semantic checks (totals, references, enums); invalid input never reaches the model |
| **Verify** | Example payload parses | Unit tests for valid/invalid/extra fields | Fuzz or property tests; versioned schema changelog |

**Example:** Zod `IncidentSchema` / alert envelope before investigation starts.

---

## C02 — Scope & tenancy

Every run is bound to an explicit scope. Out-of-scope data is filtered or rejected.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Global corpus / all tenants visible | Soft filter in prompt text | Code filters by tenant, time window, and entity; mismatch fails closed | Durable dedup keys; transactional outbox; server-enforced scope on every tool |
| **Verify** | Prompt mentions tenant | Tests prove cross-tenant rows are dropped | Integration test: forged tenant in tool args is denied |

**Example:** `demo_finance` / `northwind` scope checks; windowed event selection; source-ID dedup.

---

## C03 — Auth & identity

The model proposes; the backend authorizes. Identity is never taken from model output.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Tools run as a shared god token | “Please respect permissions” in the system prompt | Tools inherit the authenticated caller; model cannot invent user IDs | Short-lived tokens, expiry/revocation, Host/Origin checks, least privilege per tool |
| **Verify** | Docs say “auth later” | Tool handlers require session/token | Negative test: forged caller identity rejected |

**Example:** Chat token bound to authenticated user; live runtime Origin/Host + ephemeral runtime token.

---

## C04 — Tool contracts

Tools are an allowlisted registry with validated inputs and outputs.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Free-form code execution or arbitrary HTTP | Named tools, weak/no arg checks | Zod (or equivalent) on args and results; unknown tools rejected | Read-only by default; no shell/containment/email unless separately gated; audit every call |
| **Verify** | Tool list documented | Unit tests for bad args / unknown tool | Trace shows validated call → result; dangerous tools absent or dual-controlled |

**Example:** `search_knowledge`, `lookup_asset`, `check_telemetry` + `finish_analysis` with schema; MCP allowlist pattern.

---

## C05 — Retrieval & RAG

Retrieval is scoped, approved, and citation-gated. Guidance is not permission.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Whole dump into context | Keyword/vector search, no filters | Tenant + approved-document filters; top-K cap; source IDs/versions returned | Cite-only-retrieved rule enforced in code; empty result is a valid outcome; guidance cannot authorize actions |
| **Verify** | Demo retrieves something | Retrieval probes with expected IDs | Eval: unknown knowledge ID rejected; empty-query control |

**Example:** Tenant/approved runbook search; knowledge IDs only from this run’s `search_knowledge` results.

---

## C06 — Evidence integrity

Outputs that claim evidence must reference IDs that exist in the validated context.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Free prose “findings” | Model asked to cite sources | Application validates evidence/entity/graph IDs on the structured report | Bounded repair on failure; no substitute prepared answer for a failed live run |
| **Verify** | Citations look plausible | Validator rejects invented E/K IDs | Negative eval control: invented evidence must fail |

**Example:** `validateAnalysis` against known evidence and retrieved citations.

---

## C07 — Reasoning hygiene

Separate observation from inference. Hypotheses stay labeled and falsifiable.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Narrative blends facts and guesses | Prompt asks for caution | Schema fields for findings vs hypotheses vs limitations; hypotheses require alternatives + needed evidence | Attribution rules (e.g. network outcome ≠ file outcome); graph proposals use existing node IDs only |
| **Verify** | Sample output looks careful | Schema rejects missing hypothesis fields | Regression cases for misattribution |

**Example:** Graph proposals H1/H2 with status `hypothesis`, alternatives, and needed evidence; analyst instructions on outcome attribution.

---

## C08 — Agent bounds

Every autonomous loop has hard limits.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Unbounded while-loop | Soft “don’t loop forever” prompt | Max model turns, max tool calls, per-turn tool cap, wall-clock timeout, cancel path | Identical-call detection; transient retry policy; repair attempt cap; budget visible in traces |
| **Verify** | Hope | Config constants exist | Tests hit each limit and abort cleanly |

**Example:** ≤7 turns, ≤10 tools, ≤4 tools/turn, ≤2 repairs, 4-minute timeout, repeat fingerprint stop.

---

## C09 — Human control

Default deliverable is a reviewable artifact. State-changing actions are opt-in and owned.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Agent executes containment/spend/send | Human “in the loop” only by convention | Product path ends in draft/brief/inbox; actions list owners, not automatic execution | Explicit approval gate for high-impact tools; audit who approved |
| **Verify** | Demo auto-acts | UI/API has review state | Production write-back disabled unless flagged; approval recorded |

**Example:** Analyst brief + Mark reviewed; “do not execute containment”; CRM draft + human review stage.

---

## C10 — Memory

Memory is explicit, scoped, limited, and never silently promoted to fact.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Model writes durable “facts” automatically | Notes exist, mixed with evidence | Explicit save/delete; tenant key; size/count caps; labeled untrusted in prompts | Session expiry; evidence change invalidates conversation; no auto-promotion of hypotheses |
| **Verify** | localStorage dump | Tenant isolation test | Deletion removes note from next context |

**Example:** Analyst notes (10-cap, tenant key); sessions expire; notes labeled untrusted.

---

## C11 — Secrets & data hygiene

Credentials and real sensitive data stay out of source, demos, and client storage.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Keys in repo or committed `.env` | `.gitignore` only | Runtime-only secrets; redaction in traces/exports; secret scan before push | Least-privilege providers; forget/clear controls; publication review checklist |
| **Verify** | Accidental key in history | gitleaks / equivalent clean | Trace export contains no provider key |

**Example:** OpenRouter key in process memory only; SECURITY-REVIEW publication checks; gitleaks.

---

## C12 — Evaluation & honesty

Prove the controls. Do not overclaim demo quality as production quality.

| L0 | L1 | L2 | L3 |
| --- | --- | --- | --- |
| Manual eyeballing only | A few golden transcripts | Automated citation, tool-correctness, and negative-control metrics | Optional faithfulness/judge with calibrated thresholds; stale-report detection; fixture vs production paths labeled |
| **Verify** | Screenshot | CI/eval exits nonzero on failure | Negative controls fail closed; fixture vs production paths labeled in docs/UI |

**Example:** DeepEval-style citation metrics, tool-correctness checks, invented-ID negative controls; fixture playback kept separate from production loops.

---

## Scoring rules

1. A control’s score is the **lowest** maturity you can prove across all production paths that use the agent.
2. Simulated/demo paths may score lower **if** they cannot perform real side effects and are clearly labeled.
3. **Production gate:** all of C01–C08 and C11 at **L2+**; C09 L2+ if any write path exists; C10 L2+ if memory exists; C12 L2 before calling the system “evaluated.”
4. Do not average into a single vanity score without listing failing controls.

## Priority when starting from zero

1. C01 Schema → C04 Tools → C06 Citations  
2. C08 Bounds → C02 Scope → C03 Auth  
3. C05 RAG → C07 Hygiene → C09 Human gate  
4. C11 Secrets → C10 Memory → C12 Evals  
