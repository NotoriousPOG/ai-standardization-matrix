# AI Automation Build Checklist

Use this in design reviews and PRs. Check only what you can prove.

Project: ________________ Date: ________ Reviewer: ________

## Must-pass for any live model → tool loop (L2)

- [ ] **C01** Strict schema validates ingress; unknown fields rejected
- [ ] **C02** Tenant / time / entity scope enforced in code (not only in the prompt)
- [ ] **C03** Tool calls use authenticated caller identity; model cannot supply authority
- [ ] **C04** Tools are allowlisted; args and results schema-validated; unknown tools fail
- [ ] **C06** Final output validates evidence / entity / citation IDs against context
- [ ] **C08** Hard caps: model turns, tool calls, wall time; cancel works; identical loops stop
- [ ] **C11** No secrets in repo, client storage, or exported traces

## Required when the feature exists

- [ ] **C05** RAG filters approved + tenant corpora; only retrieved IDs may be cited
- [ ] **C07** Schema separates findings, hypotheses, limitations; hypotheses need alternatives / needed evidence
- [ ] **C09** Default deliverable is a draft/brief; no silent high-impact side effects
- [ ] **C10** Memory is explicit, tenant-scoped, capped, and labeled untrusted
- [ ] **C12** Automated checks cover citations, tool traces, and at least one negative control

## Honesty / product labeling

- [ ] Demo or prepared playback is labeled as such (not “live AI”)
- [ ] Limitations of conclusions are visible to the reviewer
- [ ] Write-back / containment / spend paths are off unless explicitly enabled

## PR evidence links

| Control | Evidence (test, file, eval, screenshot) |
| --- | --- |
| C01 | |
| C02 | |
| C03 | |
| C04 | |
| C05 | |
| C06 | |
| C07 | |
| C08 | |
| C09 | |
| C10 | |
| C11 | |
| C12 | |

**Decision:** ☐ Approve ☐ Approve with follow-ups ☐ Block

Follow-ups: ________________________________________________
