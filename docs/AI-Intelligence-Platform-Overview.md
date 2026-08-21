# AI Intelligence Platform — What We Are Building

**START HERE**  
**Purpose:** Give management, product, architecture, and development one simple picture of what we are building, what the client will experience, what we build first, which technology we intend to use, and how the platform expands toward Vision 2030.

## 1. Vision

> Build an enterprise AI layer that understands documents and business data, uses existing enterprise capabilities as governed tools, assists customers and employees with analysis, guidance and recommendations, and keeps humans and deterministic systems in control.

```mermaid
flowchart TD
    V[Vision 2030<br/>Automation -> Intelligence] --> P[Aspekt AI Intelligence Platform]
    P --> A[Assistant<br/>Customer-facing]
    P --> C[Copilot<br/>Employee-facing]
    P --> D[Advisor<br/>Expert-facing]
    A --> U[Customers / Employees / Bankers]
    C --> U
    D --> U
```

This is not a standalone chatbot and not a replacement for Core. It is a shared intelligence foundation around deterministic enterprise systems.

## 2. Three Experiences, One Shared Platform

A key product principle is that **Assistant, Copilot and Advisor are not separate AI backends**. They are different experiences/personas running on one shared foundation.

```mermaid
flowchart TB
    F[Shared AI Foundation]
    F --> A[Assistant<br/>Customer]
    F --> C[Copilot<br/>Agent / Operations]
    F --> D[Advisor<br/>Banker / Relationship Manager]

    F --> K[Approved Knowledge]
    F --> O[Orchestration]
    F --> I[Enterprise Integration]
    F --> G[Governance / Audit]
    F --> M[Monitoring / Analytics]
    F --> X[Shared Context / Memory]
```

### Assistant

Customer-facing experience for bounded self-service, information/guidance, process initiation and approved safe actions. Human handoff must remain clear and context-rich.

### Copilot

Employee-facing experience for retrieval, summarization, real-time guidance, drafting, document/financial analysis and next-best-action recommendations. Human remains the decision maker for consequential work.

### Advisor

Expert-facing experience for preparation, client context, product/policy knowledge, meeting support, recommendations and follow-up. AI improves professional judgment; it does not replace it.

## 3. What the Client Will Experience First

Our first rich client experience should resemble an AI Copilot embedded in the real business process. The BKT AI demonstrations remain a useful credit-workflow UX reference, not a specification to clone.

```text
┌─────────────────────────────────────────────────────────────┐
│ LOAN APPLICATION                                AI COPILOT  │
├──────────────────────────────────┬──────────────────────────┤
│ Customer / Product / Amount      │ AI Assistant             │
│ Status / Officer / Case Data     │                          │
│                                  │ Missing documents        │
│ Documents                        │ Financial warnings       │
│ ✓ Financial Statement            │ Risk / trend findings    │
│ ✓ Registration                   │ Recommended next actions │
│ ✕ Required Document              │                          │
│                                  │ [Explain]                │
│ Financial Data                   │ [Check Documents]        │
│ Revenue / EBITDA / Debt / ...    │ [Analyze Financials]     │
│                                  │ [Draft Conclusion]       │
└──────────────────────────────────┴──────────────────────────┘
```

The AI experience may later appear as chat/Copilot, contextual actions, side panels, review/compare views, AI columns, smart forms, voice and background intelligence. All use the shared AI Platform rather than isolated direct model integrations.

## 4. Connected Intelligence and Shared Context

The platform should preserve context as work moves across self-service, employees, experts and workflows.

Candidate shared context includes identity, intent, journey/case history, case details, policy basis, evidence, next step, audit trail and follow-up status.

```mermaid
flowchart LR
    A[Assistant] --> H[Human Service / Operations]
    H --> C[Copilot]
    C --> D[Advisor]
    D --> R[Relationship / Outcome]

    M[Shared Context / Connected Memory]
    M -.context.-> A
    M -.context.-> H
    M -.context.-> C
    M -.context.-> D
```

The client should experience one connected institution, not a sequence of isolated bots and systems.

## 5. High-Level Business Capabilities

```mermaid
flowchart TB
    A[AI Intelligence]
    A --> B[Understand Documents / Intent]
    A --> C[Analyze Business / Financial Data]
    A --> D[Retrieve Approved Knowledge]
    A --> E[Explain Results / Warnings]
    A --> F[Detect Problems / Missing Information]
    A --> G[Recommend Next Actions]
    A --> H[Draft Business Content]
    A --> I[Summarize Cases / Meetings]
    A --> J[Orchestrate Governed Tools]
```

Examples include document extraction/classification, financial trends/anomalies, explanation of deterministic warnings, missing-document detection, policy retrieval, recommended next steps/conditions, draft summaries or credit conclusions, case summarization, and meeting preparation.

## 6. How It Works

```mermaid
flowchart TB
    CH[Channels / Enterprise UI<br/>APS · Web · Mobile · Voice · Agent Console · Advisor Desk] --> EXP[Assistant / Copilot / Advisor]
    EXP --> GP[Governance Perimeter]

    GP --> IN[Input Controls<br/>Identity · Auth · PII · Injection]
    IN --> O[Orchestration / Agents]
    O --> T[Governed Tool Registry]
    O --> K[RAG / Approved Knowledge - when required]
    O --> M[AI Provider Gateway]

    T --> DOC[Document Tools]
    T --> CORE[Core / DDC / BPMN Tools]
    T --> FIN[Financial / Validation Tools]
    T --> EXT[Approved External Tools]

    DOC --> SF[Syncfusion or Replaceable Provider]
    CORE --> ES[Existing Enterprise APIs / Services]
    FIN --> ES

    M --> L[On-Prem or Approved Cloud LLM]

    O --> OUT[Structured Findings / Drafts / Recommendations]
    OUT --> OG[Output Controls<br/>Policy · Evidence · Quality]
    OG --> H[Human Review / Handoff / Approval]
    H --> ES
    H --> AUDIT[Audit / Monitoring / Feedback]
```

### Responsibility Boundary

| AI Intelligence | Deterministic Core / Enterprise Systems |
|---|---|
| Understand | Calculate authoritative values |
| Extract / classify | Validate authoritative rules |
| Analyze | Apply product/business rules |
| Explain | Authorize actions |
| Recommend | Persist system-of-record state |
| Draft | Execute authoritative workflow transitions |
| Predict | Enforce permissions and controls |
| Retrieve / summarize | Authenticate and control account/action access |

AI may select permitted tools, but prompts are not a security boundary. Authorization, policy, approval, validation and audit are enforced by the platform and deterministic systems.

## 7. Architecture Baseline — Clean Architecture + Modular Monolith

The platform starts as an **ASP.NET Core Modular Monolith** with **Clean Architecture principles per module**. We deliberately avoid premature microservice decomposition while preserving module and provider boundaries that can evolve later.

```mermaid
flowchart TB
    UI[Angular UI<br/>DevExtreme + selected Syncfusion components] --> API[ASP.NET Core API]
    API --> MM[Modular Monolith]
    MM --> AG[AI / Agents]
    MM --> DOC[Documents]
    MM --> FIN[Financial Intelligence]
    MM --> KNOW[Knowledge / RAG]
    MM --> GOV[Governance]
    MM --> INT[Core Integration]
    MM --> CTX[Shared Context / Memory]

    AG --> MAF[Microsoft Agent Framework Adapter]
    DOC --> SF[Syncfusion Adapter]
    FIN --> DET[Deterministic .NET Validation / Calculations]
    GOV --> HITL[Human Review / Audit / Trace]
    INT --> CORE[APS / Core / DDC / BPMN]
```

### Architecture principles

- Modular Monolith first; extract services only when operational evidence justifies it.
- Clean Architecture principles inside module boundaries.
- Frameworks/vendors remain behind application-owned contracts/adapters.
- Assistant, Copilot and Advisor share one platform foundation rather than separate implementations.
- Deterministic Core remains authoritative for calculations, validation, rules, permissions, workflow authority and system-of-record writes.
- Human-in-the-loop for consequential outcomes.
- On-premises, hybrid and approved cloud deployment are first-class requirements.
- Structured outputs/contracts are preferred when AI results are consumed by business systems.
- Approved knowledge and shared context must be governed, traceable and authorization-aware.

Detailed decision: `docs/architecture/ADR-002-Clean-Architecture-Modular-Monolith.md`.

## 8. Technology Direction

| Layer | Initial technology / approach |
|---|---|
| Existing APS frontend | Angular + DevExtreme |
| New document-heavy AI UI | Angular + selected Syncfusion runtime components where valuable |
| Backend / API | .NET / ASP.NET Core |
| Application architecture | Clean Architecture + Modular Monolith |
| Agent/orchestration candidate | Microsoft Agent Framework behind platform abstraction |
| Document intelligence / processing | Syncfusion Document SDK / Document Processing capabilities as first provider candidate |
| Document AI tools | Syncfusion AI Agent Tools where validated for the use case |
| Financial calculations / validation | Deterministic .NET services/tools |
| Existing enterprise capability | APS/Core, DDC, BPMN through governed APIs/adapters/tools |
| AI provider access | Platform-owned AI Provider Gateway |
| Cloud LLM options | Claude, OpenAI, Azure OpenAI and future approved providers |
| On-prem AI | Approved local/open LLM through the same provider boundary |
| RAG / knowledge | Provider-independent ingestion, embeddings, retrieval, reranking and authorization filtering |
| Shared context | Platform-owned connected case/journey context with explicit security/audit boundaries |
| Governance | Platform-owned HITL, authentication/authorization context, policy, evidence, audit, traces and escalation |
| Monitoring / evaluation | Technical + model + journey/business outcome metrics |

### Syncfusion position

We already have a Syncfusion license, so Syncfusion is a strong first implementation candidate for document-heavy capabilities. It remains a **provider/enabler**, not the AI Platform.

```mermaid
flowchart TD
    SF[Syncfusion] --> R[Runtime Product Capabilities]
    SF --> E[Development-Time AI Engineering]
    R --> R1[Document SDK / Processing]
    R --> R2[AI Agent Tools]
    R --> R3[PDF / DOCX / Spreadsheet UI]
    E --> E1[Agent Skills]
    E --> E2[Agentic UI Builder]
    E --> E3[AI Development Assistance]
```

Existing APS screens remain DevExtreme-based. We evaluate selected Syncfusion UI components for new document-centric/evidence-heavy AI screens rather than replacing the existing frontend stack.

## 9. Deployment and Model Independence

The same platform must support client security and residency requirements.

```mermaid
flowchart TD
    P[Same AI Intelligence Platform] --> R[Model Router / Provider Gateway]
    R --> A[On-Prem<br/>Local LLM]
    R --> B[Hybrid<br/>Local + Approved Cloud]
    R --> C[Cloud-Enabled]
    C --> D[Claude]
    C --> E[OpenAI / Azure OpenAI]
    C --> F[Future Approved Provider]
```

Agents and business services depend on platform-owned contracts, not model brands. Claude, OpenAI/Azure OpenAI, local/open models, embedding providers, retrieval stores, and document providers remain replaceable behind governed boundaries.

## 10. Governance — Trust Is a Platform Capability

Governance should be designed from the beginning rather than retrofitted later.

```mermaid
flowchart TD
    RQ[Request / Context] --> AUTH[Authentication + Authorization]
    AUTH --> CLASS[Risk / Data Classification]
    CLASS --> POL[Policy + Allowed Knowledge / Tools / Models]
    POL --> EXEC[AI / Tool Execution]
    EXEC --> CHECK[Output / Evidence / Quality Checks]
    CHECK --> DEC{Risk / Approval Requirement}
    DEC -->|Low Risk| AUTO[Approved Automated / Deterministic Outcome]
    DEC -->|Medium Risk| REVIEW[Human Review]
    DEC -->|High Risk| HUMAN[Human Decision Required]
    AUTO --> AUD[Audit / Monitoring]
    REVIEW --> AUD
    HUMAN --> AUD
```

### Risk posture

- **Low risk:** bounded information and deterministic safe operations may be automated where policy allows.
- **Medium risk:** AI suggests/drafts; human review and confidence/escalation policy apply.
- **High risk:** lending decisions, investment/regulated judgment and other consequential actions remain human-led with full traceability.

Authentication boundaries, approved knowledge, prompt-injection/PII controls, tool permissions, human handoff, manual verification, auditability and model monitoring are platform-level concerns.

## 11. What We Build First — MVP v0.1

The first management-presentable vertical slice remains deliberately small and employee-facing, consistent with the principle of proving Copilot-style augmentation before broad customer automation.

```mermaid
flowchart TD
    A[Financial Document] --> B[Extract Structured Financial Data]
    B --> C[Human-Correctable Structured Schema]
    C --> D[Deterministic Validation]
    D --> E[3-5 Deterministic Financial Metrics]
    E --> F[AI Analysis]
    F --> G[3-5 Structured Risks / Trends / Warnings]
    G --> H[Evidence-First Human Review]
    H --> I[Accept / Correct / Reject]
    I --> J[Audit]
```

### MVP v0.1 demonstrates

- one supported financial-document scenario;
- structured extraction rather than prose-only AI output;
- source/evidence beside extracted values;
- deterministic validation and financial calculations outside the LLM;
- structured AI findings/explanations;
- human review and correction;
- basic traceability/audit;
- provider boundaries that allow later on-prem/cloud model choices.

### Explicitly deferred from MVP v0.1

Customer Assistant, full enterprise RAG, BPMN/DDC integration, production Core write-back, predictive ML, a complete model-routing engine, broad autonomous workflows and multi-agent collaboration are future phases unless discovery proves one is essential to validate the MVP.

## 12. Delivery Roadmap — What We Build in Each Step

```mermaid
flowchart LR
    P1[Phase 1<br/>AI Foundation + Document/Financial Intelligence] --> P2[Phase 2<br/>APS Loan Copilot]
    P2 --> P3[Phase 3<br/>Core / DDC / BPMN Intelligence]
    P3 --> P4[Phase 4<br/>Customer Assistant + Connected Journeys]
    P4 --> P5[Phase 5<br/>Advisor Intelligence]
    P5 --> P6[Phase 6<br/>Adaptive / Predictive Connected Intelligence]
```

### Phase 1 — AI Foundation + Document & Financial Intelligence

**What we build:** document ingestion, structured extraction, deterministic validation/calculation, AI analysis, evidence, human review, basic audit and provider boundaries.

**Primary experience:** early employee Copilot/review experience.

**Technology:** ASP.NET Core Modular Monolith, Clean Architecture, Microsoft Agent Framework candidate, Syncfusion Document SDK/AI tools, deterministic .NET financial tools, AI Provider Gateway, Angular with selected Syncfusion/DevExtreme UI.

### Phase 2 — APS Loan Application Copilot

**What we build:** embed intelligence into the real Loan Application workspace with governed commands such as Analyze Financials, Check Missing Documents, Explain Warnings, Summarize Application, Draft Credit Conclusion and Suggest Next Action.

**Primary experience:** Copilot.

**Technology:** Angular + DevExtreme, AI Platform API, Agent Framework adapter, Tool Registry, Document/Financial tools, governed Core APIs.

**BKT relation:** this is where the visible credit-workflow experience becomes strongly BKT-like.

### Phase 3 — Core / DDC / BPMN Intelligence

**What we build:** expose approved Core, DDC and BPMN capabilities as governed tools; add workflow/context intelligence, missing-information detection, blocker explanation and next-step recommendations.

**Primary experience:** Copilot + internal operational intelligence.

**Technology:** Core APIs, DDC, BPMN, Tool Registry/Policy, shared context, deterministic rules, audit and HITL.

### Phase 4 — Customer Assistant + Connected Journeys

**What we build:** bounded customer self-service, guidance, process initiation, safe actions where allowed, intelligent handoff with full case/journey context.

**Primary experience:** Assistant connected to human service/Copilot.

**Technology:** Web/Mobile/Chat/Voice channels as required, shared AI Platform, RAG/approved knowledge, authentication boundaries, tool policy, escalation and connected context.

### Phase 5 — Advisor Intelligence

**What we build:** client briefings, relationship context, product/policy retrieval, meeting preparation, notes, follow-up drafts and next-best-action support.

**Primary experience:** Advisor.

**Technology:** shared context/customer 360 sources, RAG, governed external/internal data tools, AI Provider Gateway, human-controlled recommendations and audit.

### Phase 6 — Adaptive / Predictive Connected Intelligence

**What we build:** broader pattern recognition, anomaly detection, prediction, recommendation, learning/feedback loops and justified multi-agent collaboration.

**Primary experience:** Assistant + Copilot + Advisor over one connected intelligence foundation.

**Technology:** evaluation/observability, historical/event data, prediction/ML where justified, governed agentic orchestration, model routing and multi-agent only where specialist separation adds measurable value.

## 13. Measurement — Measure the Journey, Not Only the Model

Evaluation should combine technical/model measures with business journey outcomes.

```mermaid
flowchart TB
    M[Platform Measurement]
    M --> C[Customer Outcomes<br/>Resolution · Effort · Handoff Quality]
    M --> E[Employee Effectiveness<br/>Retrieval · Handling · After-Work]
    M --> R[Risk / Compliance<br/>Policy · Hallucination · Overrides · Audit]
    M --> B[Business / Economics<br/>Repeat Contact · Cost · Adoption · Growth]
```

Containment or automation rate is not the only success metric. Resolution quality, continuity, trust, employee effectiveness and compliance matter at least as much in banking scenarios.

## 14. Documentation Hierarchy

This document sits above implementation decomposition. It is not an EPIC document.

```mermaid
flowchart TD
    A[Vision 2030] --> B[AI-Intelligence-Platform-Overview.md<br/>WHAT are we building?]
    B --> C[PRD<br/>WHAT must the product do?]
    C --> D[Architecture<br/>HOW will it work?]
    D --> E[MVP / Phase Scope<br/>WHAT do we build first?]
    E --> F[EPICs<br/>Large implementation areas]
    F --> G[Features]
    G --> H[Tasks]
    H --> I[Code]
```

Every future EPIC should be traceable to an approved product/phase capability. This prevents interesting AI experiments from expanding scope without contributing to the agreed business journey.

## 15. Repository Map

Use this file as the entry point. Detailed documentation provides drill-down, not a competing product definition.

```mermaid
flowchart TD
    A[START HERE<br/>AI Intelligence Platform Overview]
    A --> B[Product / PRD]
    A --> C[Architecture]
    A --> D[Planning / MVP]
    A --> E[Research]
    A --> F[AI Engineering]

    E --> E1[Vision 2030]
    E --> E2[BKT Reference]
    E --> E3[Syncfusion]
    E --> E4[Core Frontend / Backend]
    E --> E5[AI-Enhanced Banking / Connected Service]

    C --> C1[Document + Financial Architecture]
    C --> C2[Hybrid AI / Model Routing]
    C --> C3[ADR-001 Microsoft Agent Framework]
    C --> C4[ADR-002 Clean Architecture + Modular Monolith]

    D --> D1[MVP v0.1 Management Demo]
    D --> D2[Agentic AI Architect Learning Path]
```

## 16. One-Sentence Management Explanation

> We are building one Enterprise AI Intelligence Platform on top of our existing Core systems, exposed through connected Assistant, Copilot and Advisor experiences; we start with a small Document & Financial Intelligence Copilot MVP, keep deterministic banking logic and humans in control, and progressively connect Loan, DDC, BPMN, customer journeys and advisor workflows using the same governed, on-prem/hybrid-capable AI foundation.

## 17. The Picture to Remember

```mermaid
flowchart TD
    V[Vision 2030] --> P[One AI Intelligence Platform]
    P --> A[Assistant]
    P --> C[Copilot]
    P --> D[Advisor]
    P --> AG[Agents / Orchestration]
    AG --> T[Governed Tools]
    AG --> K[Approved Knowledge / RAG]
    AG --> M[AI Provider Gateway]
    T --> CORE[APS / Core / DDC / BPMN / Documents]
    A --> H[Human / Customer Outcome]
    C --> H
    D --> H
    H --> CORE
    CORE --> F[Data / Events / History / Feedback]
    F --> P
```

Everything else in the repository explains how this shared intelligence foundation will be implemented, validated, governed, measured and expanded.
