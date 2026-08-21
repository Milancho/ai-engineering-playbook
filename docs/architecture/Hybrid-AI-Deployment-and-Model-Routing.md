# Hybrid AI Deployment and Model Routing

**Status:** Architecture principle / target capability  
**Applies to:** Enterprise AI Intelligence Platform, agents, RAG, document intelligence, Core integration

## 1. Requirement

The platform must support **fully on-premises, hybrid, and cloud-enabled deployments**. Most financial-institution deployments may require significant on-premises processing, while approved workloads should retain the option to use cloud/open LLM providers.

Agents and business services must not depend directly on a specific model vendor. Model/provider selection is a platform responsibility.

## 2. Deployment Modes

```mermaid
flowchart TB
    P[Enterprise AI Intelligence Platform]
    P --> A[Fully On-Premises]
    P --> B[Hybrid]
    P --> C[Cloud-Enabled]

    A --> A1[Local LLM]
    A --> A2[Local Embeddings]
    A --> A3[Local Vector / Search Store]

    B --> B1[Sensitive Workload -> On-Prem LLM]
    B --> B2[Approved Workload -> Cloud LLM]

    C --> C1[Approved Cloud Provider]
```

### Fully On-Premises

Core, AI Platform, orchestration, RAG, document processing, embeddings, vector/search storage and LLM inference can operate inside the customer network.

### Hybrid

The platform can route workloads between local and cloud models according to tenant policy, data classification, capability, cost, latency and residency requirements.

### Cloud-Enabled

Approved clients/workloads can use external AI providers while still passing through the same AI Platform governance boundary.

## 3. AI Provider Gateway

```mermaid
flowchart LR
    A[Agent / AI Service] --> B[Platform Model Contract]
    B --> C[AI Provider Gateway]
    C --> D[Model Router]
    D --> E[On-Prem LLM]
    D --> F[OpenAI / Azure OpenAI]
    D --> G[Anthropic Claude]
    D --> H[Future Approved Provider]
```

Agents request **capabilities**, not model brands. A Financial Analysis Agent should describe requirements such as structured output, reasoning capability, context size and confidentiality; the router selects an allowed implementation.

## 4. Model Routing Policy

```mermaid
flowchart TD
    A[AI Request] --> B[Identify Tenant / Client]
    B --> C[Data Classification]
    C --> D[Residency / External AI Policy]
    D --> E[Task + Required Capabilities]
    E --> F[Cost / Latency / Availability Policy]
    F --> G[Model Router]
    G --> H[Approved Provider + Model]
```

Candidate routing inputs:

- tenant/client policy;
- task type;
- data classification;
- external-provider permission;
- residency requirements;
- required model capabilities;
- structured-output/tool-calling requirements;
- context size;
- cost ceiling;
- latency target;
- provider/model availability;
- approved fallback policy.

## 5. Claude and Other Cloud Providers

Claude is treated as an **optional provider behind the AI Provider Gateway**, not as an architectural dependency. The same rule applies to OpenAI, Azure OpenAI and future providers.

```mermaid
flowchart LR
    A[Platform] --> B[AI Provider Gateway]
    B --> C[Claude Adapter]
    B --> D[OpenAI Adapter]
    B --> E[Azure OpenAI Adapter]
    B --> F[Local LLM Adapter]
```

Provider-specific features may be used through adapters where valuable, but platform/domain contracts, audit models, security policy and business workflows remain provider-independent.

## 6. On-Prem LLM Boundary

The exact local inference technology/model remains a deployment decision. The architecture must allow an approved local serving stack/model to satisfy the same platform contract where capabilities permit.

```mermaid
flowchart LR
    A[AI Provider Gateway] --> B[Local Provider Adapter]
    B --> C[Local Inference Service]
    C --> D[Approved Open / Local Model]
```

No specific local model is mandated by this architecture document.

## 7. RAG Provider Independence

LLM routing alone is insufficient. Embeddings and retrieval infrastructure must also preserve deployment independence.

```mermaid
flowchart TD
    A[Approved Knowledge / Documents] --> B[Ingestion]
    B --> C[Embedding Provider Contract]
    C --> D[Local Embeddings]
    C --> E[Approved Cloud Embeddings]
    D --> F[Retrieval / Vector Store Contract]
    E --> F
    F --> G[On-Prem Store]
    F --> H[Approved Cloud Store]
    G --> I[Retriever]
    H --> I
    I --> J[Agent / AI Analysis]
```

LLM provider, embedding provider, and retrieval/vector-store implementation should be independently replaceable where practical.

## 8. Security Principle

Sensitive data must not be sent to an external model merely because a UI component or agent selected it. Routing is governed centrally.

```mermaid
flowchart LR
    A[Business Context] --> B[Authorization]
    B --> C[Data / Privacy Classification]
    C --> D[Provider Policy]
    D --> E[Context Minimization / Redaction if required]
    E --> F[Approved Model Route]
    F --> G[Audit]
```

## 9. Relationship to Tool Architecture

Model portability and tool portability are separate concerns.

```mermaid
flowchart TB
    A[AI Orchestrator] --> B[AI Provider Gateway]
    A --> C[Tool Registry / Policy]
    B --> D[Cloud / Local Models]
    C --> E[Core / DDC / BPMN Tools]
    C --> F[Syncfusion / Document Tools]
    C --> G[Financial / Validation Tools]
```

The model reasons and selects permitted tools; deterministic enterprise services execute authoritative operations.

## 10. Architecture Decision

**Deployment and Model Independence:** The Enterprise AI Intelligence Platform must support fully on-premises, hybrid, and cloud-enabled deployments. LLMs, embedding models, retrieval/vector stores, document providers, and external AI services must be accessed through platform-owned abstraction boundaries. Provider selection must be governed by tenant policy, data classification, capability, cost, latency, availability and residency requirements.
