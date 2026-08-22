# AI Architect — Learning Roadmap
### .NET · Enterprise Banking · **Portable: Cloud + On-Premises**

---

## The constraint that defines everything

ASPEKT must ship the same product to cloud clients **and** to banks that run on-premises. That single requirement is more architecturally consequential than any framework choice in this document, and it produces three hard rules:

> **1. No managed service may sit on a critical path.**
> Every Azure-managed component you adopt is a component you must reimplement for the on-prem build. Adopt them only where you have a working open-source equivalent already identified.

> **2. Design for the weakest model you will ever run.**
> On-prem means open-weight. Open-weight models are meaningfully worse at multi-step tool calling, instruction adherence under long context, and knowing when to stop. An architecture that works on a frontier model can fail outright on a 70B model in a bank's datacenter — so the on-prem target sets the ceiling for the whole product.

> **3. An LLM deciding to call a tool does not authorize the action.**
> Authorization is deterministic, sits between model output and execution, and is identical in both deployments. This is the line between a demo and a system a bank will run.

Fourth rule, from the ChatGPT draft and worth keeping verbatim: **authoritative business data lives in enterprise systems, never in LLM memory.**

---

## Portability matrix — build this in week 1

This table *is* your architecture. Fill it before writing code; every row is a potential ADR.

| Capability | Cloud | On-prem | Abstraction |
|---|---|---|---|
| Inference | Azure OpenAI | vLLM / NVIDIA NIM / Ollama | OpenAI-compatible endpoint |
| Model | GPT-family | Llama / Qwen / Mistral / Phi | Config, never code |
| Embeddings | Azure OpenAI | BGE-M3, E5, Nomic (self-hosted) | Same interface |
| Vector store | Azure AI Search | pgvector / Qdrant / Weaviate | Repository interface |
| Orchestration | Agent Framework / SK | Same — runs anywhere | — |
| Durable workflow | Durable Functions | **Temporal self-hosted** | Prefer Temporal for both |
| Tracing | App Insights | Langfuse + OTel Collector | **OTel — always OTel** |
| Evaluation | Foundry eval | Ragas + own harness | Own harness for both |
| Guardrails | Content Safety | Llama Guard / NeMo Guardrails | Policy interface |
| Identity | Entra ID | AD / Keycloak | OIDC |

**The unifying trick:** vLLM and most on-prem runtimes expose an OpenAI-compatible API. Point [Microsoft.Extensions.AI](https://learn.microsoft.com/en-us/dotnet/ai/) at either endpoint and the model provider becomes configuration. Do this on day one — it costs almost nothing early and is expensive to retrofit.

Where a row has no shared abstraction (Azure AI Search is the obvious one), **choose the portable option even though it's worse in cloud.** One codebase beats two.

---

## Core vocabulary

| Concept | Purpose | Architect's question |
|---|---|---|
| **Agent** | Who is responsible | What is it accountable for, and where does it escalate? |
| **Skill** | What expertise it has | Reusable across agents, or coupled to one? |
| **Tool** | What operation it executes | What's the blast radius if it fires wrongly? |
| **Workflow** | How execution is controlled | What must be deterministic here? |
| **RAG** | What knowledge it retrieves | Who owns the corpus, how is staleness detected? |

```
Loan Agent
 ├── Credit Analysis Skill
 ├── Compliance Skill
 ├── Loan Policy Skill
 ├── Customer API Tool      ← authorization gate here
 └── RAG (policy corpus)
```

---

## Structure

A 16-step waterfall puts evaluation at step 11 and the architecture at step 16 — the two things you're hired for arrive last, after energy runs out. Instead, **three tracks in parallel from week 1**:

| Track | Weeks | Nature |
|---|---|---|
| **Build** — one system, progressively harder, **both targets** | 1–11 | Hands-on |
| **Architecture** — the document, written incrementally | 1–12 | Written |
| **Governance** — regulation, security, cost | 4–12 | Read + map |

The document is never written at the end. It's written weekly and finishes when the build does.

**The one build:** not 12 demos. One system with real ASPEKT users, real documents, real approval steps — running in both deployment modes.

---

## Weeks 1–2 · Foundations, tools, authorization, portability

**Learn:** tokens/context, structured output, function calling, grounding, tool contracts, error handling — and *when not to use an LLM*.

- [Microsoft — Generative AI for Beginners](https://github.com/microsoft/generative-ai-for-beginners)
- [Microsoft — AI Agents for Beginners](https://github.com/microsoft/ai-agents-for-beginners)
- [Microsoft.Extensions.AI](https://learn.microsoft.com/en-us/dotnet/ai/) — **the provider abstraction; start here, not with Foundry**
- [Microsoft Agent Framework — Get Started](https://learn.microsoft.com/en-us/agent-framework/get-started/)
- [Anthropic — Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)
- [vLLM](https://docs.vllm.ai) — get a local model serving an OpenAI-compatible endpoint in week 1
- [Ollama](https://ollama.com) — fastest path to a laptop dev loop

**Build:** .NET agent with three real tools — `GetCustomer()`, `GetLoan()`, `GetWorkflowStatus()`. Authorization gate present from the first commit; retrofitting it is how systems ship without it. **Run the same agent against Azure OpenAI and a local Llama or Qwen model.** Record the behavioural delta — that delta is your most important early finding.

Then **deliberately break it**: bad tool descriptions, ambiguous state, downstream timeout, tool called with plausible-but-wrong arguments. On the local model, break it harder — you'll find it fails earlier and stranger.

> ⚠️ **Verify Agent Framework GA status.** It's the Semantic Kernel + AutoGen consolidation — sound direction, but 12 weeks on a preview API is a risk. Keep agent logic behind your own interfaces. That isolation is itself ADR-004.

**Architecture doc:** scope, boundaries, deterministic-vs-LLM split, **portability matrix**.

---

## Weeks 3–4 · Retrieval, RAG, evaluation *from the start*

**Learn:** embeddings, chunking, hybrid search, metadata filtering, citations.

- [Microsoft — RAG solution design & evaluation guide](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/rag/rag-solution-design-and-evaluation-guide)
- [pgvector](https://github.com/pgvector/pgvector) — if the bank already runs Postgres, this is often the whole answer
- [Qdrant](https://qdrant.tech) — self-hostable, good .NET client
- [Ragas](https://docs.ragas.io) — **evaluate from week 3, not week 11**

```
Documents → Chunking → Embeddings → Index → Retriever → LLM → Grounded answer + sources
```

**On-prem note:** self-hosted embedding models (BGE-M3, E5) are competitive with cloud embeddings — this is the one area where the on-prem gap is small. Hybrid search (BM25 + vector) matters *more* on-prem, because it compensates for weaker generation.

Evaluation late is evaluation never. Build a 30-question golden set in week 3 and grow it weekly. **Run it against both model tiers** — cloud and local — every time. The two scores are different products and both must ship.

**Architect's question:** document lifecycle. Re-indexing cadence, permission inheritance, stale-content detection, and *who is accountable when the agent cites a superseded credit policy.*

---

## Week 5 · Agentic RAG + ADR-001

- [Microsoft — Agentic RAG architecture](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/rag/rag-agentic)

**Test this honestly on-prem.** Agentic RAG multiplies model calls, and iterative query planning is precisely where smaller models degrade. It may be a cloud-only feature, or may need a tighter deterministic query planner on-prem. Either conclusion is a legitimate architectural finding.

**ADR-001 — Traditional vs Agentic RAG**, with measured numbers from your golden set on both targets. An ADR without measurements is an opinion with formatting.

---

## Week 6 · On-prem inference, seriously

The comparative week, reframed. This is now the highest-value week in the plan.

- [vLLM](https://docs.vllm.ai) — production serving, continuous batching, tensor parallelism
- [NVIDIA NIM](https://developer.nvidia.com/nim) — packaged inference microservices; banks like the support contract
- [Llama Guard](https://www.llama.com/docs/model-cards-and-prompt-formats/llama-guard-3/) / [NeMo Guardrails](https://github.com/NVIDIA/NeMo-Guardrails)
- [Azure Local / Arc](https://learn.microsoft.com/en-us/azure/azure-local/) — if the bank wants Microsoft-supported on-prem

### GPU sizing — the thing nobody plans and finance always asks

Rough VRAM, weights only:

| Model | fp16 | 8-bit | 4-bit |
|---|---|---|---|
| 8B | ~16 GB | ~8 GB | ~5 GB |
| 32B | ~64 GB | ~32 GB | ~18 GB |
| 70B | ~140 GB | ~70 GB | ~40 GB |

Add KV cache on top — and **KV cache, not weights, is what concurrency actually costs you.** Long RAG contexts × concurrent users is the real sizing driver. A 70B at 4-bit fits one A100 80GB for a demo and falls over at 20 concurrent users with 32k contexts.

Produce a sizing model: tokens/sec required, concurrency, context length, quantization tradeoff, redundancy for HA. Then the CapEx number. This document alone will make you useful to ASPEKT immediately, independent of the rest of the roadmap.

**Quantization is a quality decision, not an infra decision.** 4-bit degrades tool-calling reliability. Measure it on your golden set; don't accept vendor benchmarks.

**ADR-002 — deployment topology and model strategy:** which models on which hardware, quantization, and the honest capability delta between tiers.

---

## Weeks 7–8 · Skills, memory, state, multi-agent

- [Agent Skills for .NET](https://devblogs.microsoft.com/agent-framework/agent-skills-for-net-is-now-released/)
- [Multi-agent orchestration](https://learn.microsoft.com/en-us/agent-framework/workflows/orchestrations/)
- Kleppmann, *Designing Data-Intensive Applications* — shared-data-model chapters

```
Agent
 ├── Context        (ephemeral)
 ├── Memory         (bounded, non-authoritative)
 └── Business State → Database / Enterprise API   ← only source of truth
```

**Most "multi-agent" failures are distributed-systems failures wearing a costume.** Idempotency, partial failure, compensating transactions — same problems, new vocabulary.

**On-prem multiplier:** every extra agent is another inference call against fixed GPU capacity. In cloud, multi-agent costs money and scales elastically. On-prem it costs *latency and throughput you cannot buy your way out of mid-quarter.* Multi-agent designs that are merely expensive in cloud are infeasible on-prem. This is the strongest argument in the document for keeping agent count low.

### Shared data models — in your JD, absent from most curricula
1. **Agent state schema** — what persists, what's ephemeral, what's PII
2. **Inter-agent contract** — *typed messages, not free-text handoffs.* Free-text handoffs are the most common cause of silent multi-agent failure, and smaller on-prem models make it far worse.
3. **Decision record** — inputs, model version, prompt version, retrieved sources, confidence, approver

---

## Week 9 · Workflows + human-in-the-loop

- [Agent Framework — Workflows](https://learn.microsoft.com/en-us/agent-framework/workflows/)
- [Temporal](https://temporal.io) + [.NET SDK](https://github.com/temporalio/sdk-dotnet) — **self-hostable, which is why it beats Durable Functions here**

```
AI Recommendation → Policy Check → Human Approval
                                    ├── Approve → Resume → Execute
                                    └── Reject  → Escalate → Record reason
```

Build a process that pauses and resumes **three days later, after a service restart.** Most portfolio projects cheat with in-memory state. Rejection and escalation paths matter more than approval paths — that's where auditors look.

**HITL is load-bearing on-prem.** Where a weaker model is less reliable, the correct response is more human gates and tighter deterministic routing — not a better prompt.

**ADR-003 — Autonomous Agent vs Deterministic Workflow + HITL.** Argue honestly. If deterministic wins, say so; that conclusion is a stronger hiring signal than the alternative.

---

## Week 10 · Observability, traceability, audit

- [OpenTelemetry .NET](https://opentelemetry.io/docs/languages/dotnet/)
- [OpenTelemetry — GenAI semantic conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/)
- [Langfuse](https://langfuse.com) — **self-hostable, so it works in both deployments**
- Grafana + Tempo + Prometheus — the on-prem observability stack banks already run

```
Trace ID
 ├── User + tenant
 ├── Agent + version
 ├── Model + version + quantization    ← on-prem addition
 ├── Prompt + version
 ├── Retrieved sources
 ├── Decisions + confidence
 ├── Tool calls + authorization outcome
 ├── Human approval + identity
 └── Final action
```

**Technical telemetry and business audit are different systems** — different retention, consumers, regulators. Ops wants p99 latency. The auditor wants to reconstruct why a customer was declined in March. Design both.

Also: **regression gates in CI** against the week-3 golden set, on both model tiers.

**Air-gap consideration:** some banks permit no telemetry egress at all. Your observability stack must run fully inside their perimeter, and you need a supportable story for how ASPEKT diagnoses a production issue it cannot see. Decide now whether you ship a diagnostic bundle export.

---

## Week 11 · Security, guardrails, regulation

### Security
- [OWASP GenAI Security Project](https://genai.owasp.org) — LLM Top 10
- [Llama Guard](https://www.llama.com/docs/model-cards-and-prompt-formats/llama-guard-3/) / [NeMo Guardrails](https://github.com/NVIDIA/NeMo-Guardrails) — the portable guardrail layer

Prompt injection, **indirect** prompt injection (a retrieved document containing instructions — the one that catches RAG systems), excessive agency, data leakage, tool abuse, RBAC, least privilege, tenant isolation.

```
LLM Decision → Guardrail/Policy → Authorization → Tool → Enterprise API
```

On-prem removes one class of risk (data egress) and adds another: **you now own patching, CVEs, and supply chain for the entire inference stack** — model weights, CUDA, vLLM, the vector database. Model provenance becomes a security question: which weights, from where, verified how.

### Regulation
- **[EU AI Act](https://artificialintelligenceact.eu)** — **creditworthiness assessment and credit scoring are explicitly high-risk under Annex III.** Risk management system, data governance, technical documentation, record-keeping, human oversight, accuracy and robustness declarations. Read Annex III and Chapter III properly, not a summary.
- **[DORA](https://www.digital-operational-resilience-act.com/)** — applies to EU financial entities since January 2025. As a vendor into banks, ASPEKT becomes a registered ICT third-party provider inside clients' scope. **On-prem changes the shape of this, not the existence of it** — you're still a third-party provider, just with a different resilience-testing story.
- **[NIST AI RMF](https://www.nist.gov/itl/ai-risk-management-framework)** — structure your risk documentation around it
- **ISO/IEC 42001** — needed eventually to clear bank procurement
- **GDPR** — Article 22 automated decision-making, lawful basis, right to explanation

On-prem is frequently *sold* as the compliance answer. It isn't. It solves data residency and egress. It does not touch Annex III obligations, Article 22, or DORA. Be the person in the room who knows that.

---

## Week 12 · Architecture, cost, lifecycle

- [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
- [Model Context Protocol](https://modelcontextprotocol.io) + [C# SDK](https://github.com/modelcontextprotocol/csharp-sdk)
- [AWS Well-Architected ML Lens](https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/) — stack-agnostic questions, read it anyway
- [TOGAF](https://www.opengroup.org/togaf) — bank clients often speak it

```
Channels
   ↓
API Gateway
   ↓
Agentic AI Platform          ← identical in both deployments
   ├── Orchestrator
   ├── Agents
   ├── RAG
   ├── Workflows / HITL
   └── Skills / Tools
   ↓
Model Abstraction Layer      ← the portability seam
   ├── Cloud:   Azure OpenAI
   └── On-prem: vLLM / NIM
   ↓
Integration Layer
   ├── Core Banking APIs
   ├── BPMN
   ├── CRM
   └── Data Services

Security │ Audit │ Observability │ Evaluation │ Cost
```

### Cost model — two completely different models

**Cloud is OpEx per token.** Cost per transaction × volume. Scales elastically, dies from success.

**On-prem is CapEx plus utilization.** GPUs, power, cooling, rack space, HA redundancy, and a 3-year refresh. Cost per transaction is a *function of utilization* — at 5% GPU utilization the per-transaction cost is absurd; at 60% it may beat cloud decisively. Your break-even chart is: transaction volume on x, cost on y, two curves, crossover point marked. **That single chart is the most persuasive artifact in your entire portfolio**, because it's the question every bank CFO asks and almost no vendor answers.

Include: idle cost (GPUs cost the same at 3am), HA redundancy multiplier, and the eval suite's own inference cost.

### Model lifecycle
Cloud models are deprecated on the vendor's schedule. On-prem models are frozen on **yours** — which sounds better and isn't: a bank on a two-year-old model with known weaknesses, and no one budgeted the upgrade. Document pinned versions, re-evaluation procedure, prompt portability, and who signs off that behaviour is unchanged. In a regulated environment, a model swap is a change-management event.

---

## The architect's decision flow

```
Business Requirement
        ↓
Cloud, on-prem, or both?         ← now the first question, not the last
        ↓
What MUST be deterministic?
        ↓
What genuinely benefits from LLM reasoning?
        ↓
Is an agent needed at all?        ← a real gate
        ↓
What knowledge requires RAG?
        ↓
What becomes a Skill? A Tool?
        ↓
Single agent or multi-agent?      ← default single; on-prem, strongly single
        ↓
Workflow / Orchestration
        ↓
Where is HITL required?           ← more gates on weaker models
        ↓
Authorization + Security
        ↓
Audit + Traceability
        ↓
Evaluation + Monitoring           ← on both model tiers
        ↓
Cost model                        ← OpEx and CapEx, with crossover
        ↓
Target Architecture → Technology Recommendation → Implementation Roadmap
```

Three defaults worth holding: **deterministic until proven otherwise**, **single agent until proven otherwise**, and **portable until proven otherwise.**

---

## Portfolio deliverables

1. **Enterprise Agentic AI Reference Architecture** — with a **dual-deployment topology**, portability matrix, GPU sizing model, cost crossover analysis, and regulatory mapping alongside the standard sections.

2. **Five ADRs** — each with rejected alternatives, measured evidence, consequences:
   - ADR-001 — Traditional vs Agentic RAG (measured on both tiers)
   - ADR-002 — Deployment topology and model strategy
   - ADR-003 — Autonomous Agent vs Deterministic Workflow + HITL
   - ADR-004 — Framework isolation boundary
   - **ADR-005 — Vector store selection under the portability constraint** (this is where the Azure AI Search decision gets made or refused)

3. **One near-production .NET implementation, running in both modes.** A demo repo is worth almost nothing. A system someone depends on, deployable into a bank's datacenter, is worth a great deal.

---

## On building with AI assistance

You build heavily with Claude. One real asset, one specific liability.

**Asset:** you can prototype three competing architectures in the time it took to argue about one. That's the architect's muscle — cheap, disposable spikes converting opinions into measurements. Use it for the on-prem comparisons especially: same agent, four models, one golden set.

**Liability:** an architect defends decisions under adversarial questioning. If the code was generated and you can't explain *why* the retry policy is exponential or *why* state checkpoints at that boundary, you fail the only test that matters. **Anything in the reference architecture, you must be able to whiteboard from memory. Anything below that line, generate freely.**

Note the irony worth being ready for: you build with frontier cloud models while shipping systems that run on weaker on-prem ones. Don't let your development experience set your expectations for what the delivered system can do.

---

## Certifications — after the work, not instead of it

| Certification | Verdict |
|---|---|
| [Azure AI Engineer (AI-102)](https://learn.microsoft.com/credentials/certifications/azure-ai-engineer/) | Reasonable, but **heavily cloud-managed-service oriented** — much of it doesn't transfer to your on-prem half |
| [Azure Solutions Architect Expert (AZ-305)](https://learn.microsoft.com/credentials/certifications/azure-solutions-architect/) | Stronger *architect* signal; hybrid/Arc content is directly relevant |
| [TOGAF](https://www.opengroup.org/togaf) | Bureaucratic; sometimes mandatory in tenders and public sector |
| Kubernetes (CKA) | **Underrated for you** — on-prem GPU serving lands on Kubernetes more often than not |

Skip anything branded "AI Architect Certified" by a vendor you've never heard of.

---

## Weekly cadence

| Block | Hours/week | Activity |
|---|---|---|
| Building | 6–8 | The one system — always running, both targets |
| Reading | 3–4 | Docs, one book chapter, one primary paper |
| Writing | 2 | This week's architecture section, plus ADRs |

Writing is not optional. An architect who cannot write a decision document is an engineer with a title.

**Apply sideways:** get the AI-heavy work inside ASPEKT before chasing the title elsewhere. Internal architects get made; external ones get poached.

---

## Verify every link

Click all of these — mine included. I can't browse, and my reliable knowledge runs to roughly May 2026. Microsoft docs URLs churn fast in this area, the Agent Framework surface is moving, and the open-weight model landscape changes monthly — treat any specific model recommendation here as a category, not a pick. Regulatory deadlines, EU AI Act phase-in especially, must be checked against a current primary source, never a language model.

**GPU memory figures above are order-of-magnitude planning numbers.** Validate against real benchmarks on your actual workload before anyone signs a hardware purchase order.
