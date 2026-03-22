# PageIndex-Style Vectorless RAG Goals (Approval Draft)

## Status

Pending stakeholder approval.

## Purpose

Provide a complete, execution-ready goal list for researching and implementing a **PageIndex-style vectorless RAG** system (hierarchical structural index + reasoning-guided traversal, without embeddings/vector DB).

## Scope Definition

### In scope
- Hierarchical document indexing from structured and semi-structured sources.
- Query-time reasoning/navigation over index trees/forests.
- Context assembly and answer generation with traceable citations.
- Enterprise SDLC deliverables (requirements, architecture, testing, operations, governance).

### Out of scope
- Operating a vector database for this workstream.
- Training custom embedding models for this workstream.
- Full legal review of external content licenses (handled as a separate legal track).

## Success Criteria (Program-Level)

This initiative is considered successful when all conditions below are met:
1. **Traceability:** every answer can point to source sections/pages used during retrieval.
2. **Reproducibility:** reference-study runs are replayable with pinned commit/model/prompt/config fingerprints.
3. **Operational viability:** documented latency/cost envelope for representative document sizes.
4. **Enterprise readiness:** security/privacy controls, runbooks, and governance artifacts are approved.

---

## Phase A — Research and Conceptual Mastery

### A1. Precise problem statement
- Define vectorless RAG vs embedding RAG, including I/O behavior, preferred use-cases, and known failure modes.
- Produce explicit non-goals for this initiative.

**Deliverable:** `A1_problem_statement.md`.

### A2. End-to-end PageIndex pipeline map
- Document ingestion → representation → hierarchical index build → query-time navigation → context assembly → answer generation (+ optional verification).
- Include one canonical architecture diagram.

**Deliverable:** `A2_reference_architecture.md` + diagram.

### A3. Controlled glossary
- Define terminology and map each term to PageIndex-style semantics: RAG, node/leaf, TOC, traversal, beam/best-first (if used), LLM-as-router, citation, hallucination, OCR, layout analysis, token budget, etc.

**Deliverable:** `A3_glossary.md`.

### A4. Algorithms and data structures
- Specify parsing strategies (PDF/MD/DOCX), heading detection, tree construction rules (merge/split), depth constraints, traversal/search strategy, scoring/ranking heuristics, stopping criteria, and fallback behavior when structure quality is poor.

**Deliverable:** `A4_algorithms_data_structures.md`.

### A5. Quantitative model
- Document token accounting, context budget policy, traversal complexity, and latency/cost trade-offs.
- Define node-size calibration method and expected impact on quality/latency.

**Deliverable:** `A5_math_and_complexity.md`.

### A6. Comparative analysis
- Compare PageIndex-style retrieval with dense, sparse (BM25), hybrid, graph RAG, and agentic multi-hop search across cost, latency, explainability, maintainability, and infra footprint.

**Deliverable:** `A6_comparative_analysis.md`.

### A7. Licensing/compliance study boundaries
- Inventory all external artifacts to be studied and classify: reusable, reference-only, or clean-room-only.

**Deliverable:** `A7_artifact_licensing_matrix.md`.

**Phase A exit gate:** all A1–A7 deliverables reviewed and signed off.

---

## Phase B — Reference Implementation Backtrack (Pinned Version)

### B1. Artifact inventory
- Pin exact repository commit/release, runtime versions, dependency lock state, and required services/APIs.

### B2. Repository map
- Produce a module-level map: entrypoints, CLI, config, parsers, index builder, retriever, prompt assets, schemas, tests, examples.

### B3. Minimal execution trace
- Run one minimal scenario (single document + one question).
- Capture call sequence, disk artifacts, prompt flow, model calls (count + purpose), and final answer/citations.

### B4. Data contracts
- Enumerate schemas: document model, node/edge metadata, artifact serialization, and schema versioning conventions.

### B5. Prompt/policy inventory
- Extract all prompts and parse/repair logic (JSON mode, regex, guardrails, retries, failure handling).

### B6. Configuration matrix
- Catalog every tunable parameter and expected behavior impact.

### B7. Test/benchmark gap analysis
- Record what reference tests cover and what enterprise controls are missing (PII, scanned docs, adversarial prompts, long docs).

**Phase B deliverables:**
- `B_reference_backtrack_report.md`
- `B_execution_trace_artifacts/`
- `B_config_matrix.csv`

**Phase B exit gate:** reproducible run artifact can be replayed by another engineer without undocumented steps.

---

## Phase C — Technical Documentation (“How it works”)

### C1. Architecture document
- Logical/physical views, component boundaries, and sequence diagrams (indexing path and query path).

### C2. End-to-end worked example
- One complete walkthrough with intermediate tree snapshots, retrieval decisions, token math per step, final context assembly, and citation mapping.

### C3. Appendix bundle
- Consolidated glossary + quantitative appendix for onboarding and design review.

### C4. Operational characteristics
- Latency decomposition, cost model (LLM calls × unit pricing), failure modes, observability hooks (logs/metrics/traces).

### C5. Lightweight threat model
- Prompt injection, sensitive-data leakage, unsafe ingestion vectors, and baseline mitigations.

**Phase C exit gate:** documentation package approved by engineering + security stakeholders.

---

## Phase D — In-House Enterprise Implementation Plan

### D1. Charter and measurable targets
- Define MVP/GA scope and measurable SLOs.
- Minimum metrics:
  - citation accuracy target
  - faithfulness/grounding proxy target
  - p95 latency by doc size band
  - cost/query target

### D2. Requirements baseline
- Functional: supported formats, sync/async APIs, tenancy boundaries, RBAC, audit trail.
- Non-functional: availability, scalability, retention, encryption, data residency, rate controls.

### D3. Architecture/design
- Service decomposition, API contracts, async indexing workflow, storage design for raw docs + index artifacts, secret handling.

### D4. Milestone plan
- Parser abstraction → index builder → retriever → orchestration layer → evaluation harness → hardening.

### D5. Quality engineering
- Unit, contract, golden-file E2E, and load tests.

### D6. LLMOps controls
- Model routing policy, prompt/index versioning, tracing, evaluation datasets, regression gates.

### D7. Security/privacy controls
- PII handling, tenant isolation, secure deletion, redaction options.

### D8. Runbooks
- Deploy, rollback, reindex, partial-failure recovery, incident response.

### D9. Parallel implementation documentation
- Author docs mirroring Phase C for the in-house system, including explicit deltas from reference behavior.

**Phase D exit gate:** MVP implementation plan approved with staffing, timeline, and risks.

---

## Phase E — Governance and Handoff

### E1. ADR decision log
- Record why vectorless PageIndex-style approach was chosen vs alternatives for target workloads.

### E2. Training assets
- Onboarding path, sandbox exercise, and troubleshooting guide for new engineers.

**Phase E exit gate:** handoff package accepted by owning team.

---

## Mandatory Amendments Included

1. **Baseline pinning by end of B1:** no deep analysis begins until one canonical reference commit is selected.
2. **Acceptance metrics locked by end of D1:** no implementation starts without measurable quality/latency/cost targets.
3. **Reproducibility controls in B3 + D6:** run profiles must capture model, tokenizer, prompt, config, and artifact version stamps.

## Open Decisions Required Before Execution

1. **Reference baseline:** VectifyAI/PageIndex only, or include additional public/commercial documentation variants?
2. **Deployment target:** Kubernetes/OpenShift/on-prem/cloud constraints and data residency requirements?
3. **LLM policy:** OpenAI, Azure OpenAI, local-only (vLLM/Ollama), or mixed strategy?
4. **Runtime standard:** Python-only acceptable, or polyglot requirement (e.g., Java/Go services)?
5. **Primary document domain:** legal, financial filings, mixed enterprise PDFs, or another corpus?

## Recommended Next Action

Approve/amend this draft and answer the five open decisions so execution planning can proceed with pinned assumptions.
