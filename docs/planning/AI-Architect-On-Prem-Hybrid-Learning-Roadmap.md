# AI Architect — On-Prem & Hybrid AI Learning Roadmap

## Purpose

This roadmap extends the Agentic AI Architect learning path with the ability to design solutions that can run:

- Fully on-premises
- In a private datacenter
- In restricted or air-gapped environments
- In hybrid on-prem + cloud environments
- With interchangeable LLM providers

The architectural goal is to keep agents, skills, workflows, RAG, and business integrations independent from the physical location or vendor of the LLM.

## Target Architecture

```text
                    ENTERPRISE / BANK DATACENTER
┌─────────────────────────────────────────────────────────┐
│                                                         │
│ Channels / Angular / Mobile                             │
│              │                                          │
│              ▼                                          │
│        .NET API Gateway                                 │
│              │                                          │
│              ▼                                          │
│       Agentic AI Platform                               │
│       ├── Orchestrator                                  │
│       ├── Agents                                        │
│       ├── Skills                                        │
│       ├── Workflows / HITL                              │
│       ├── Guardrails                                    │
│       └── Model Abstraction                             │
│              │                                          │
│       ┌──────┴──────────┐                               │
│       ▼                 ▼                               │
│   Local RAG        Local LLM Serving                    │
│       │            Ollama / vLLM                       │
│       ▼                 │                               │
│ Vector Store            │                               │
│       └────────┬────────┘                               │
│                ▼                                        │
│         Enterprise APIs                                 │
│   Core / BPMN / CRM / DDC / Documents                  │
│                                                         │
│ Security │ Audit │ OTel │ Evaluation │ Monitoring      │
└─────────────────────────────────────────────────────────┘
                       │
                       │ OPTIONAL
                       ▼
                Cloud AI Providers
```

## Learning Order

```text
1. Local LLM Fundamentals
        ↓
2. Ollama Development
        ↓
3. Production LLM Serving with vLLM
        ↓
4. Local Embeddings + RAG
        ↓
5. Local Vector Stores
        ↓
6. Model Gateway / Provider Abstraction
        ↓
7. Agent Framework with Local Models
        ↓
8. On-Prem Security
        ↓
9. Air-Gapped Architecture
        ↓
10. GPU / Capacity Planning
        ↓
11. Observability + Evaluation
        ↓
12. High Availability + Disaster Recovery
        ↓
13. Hybrid AI Architecture
        ↓
14. Final On-Prem Reference Architecture
```

## 1. Local LLM Fundamentals

### Learn

- Open-weight vs hosted models
- Model size and parameter count
- Quantization
- Context windows
- Tokens per second
- VRAM requirements
- CPU vs GPU inference
- Concurrent requests
- Model licensing
- Data residency

### Goal

Understand that an AI application must not assume the model is a cloud API.

```text
Agent Application
       │
       ▼
Model Interface
       │
 ┌─────┼─────────┐
 ▼     ▼         ▼
Local  Azure    Other
LLM    Model    Provider
```

## 2. Ollama — Local Development

### Resource

[Ollama Documentation](https://docs.ollama.com/)

### Learn

- Install and run local models
- Model selection
- Local REST/API usage
- Embedding models
- Model configuration
- Development/testing workflows

### Build

```text
.NET Application
      ↓
AI Abstraction
      ↓
Ollama
      ↓
Local Model
```

### Goal

Run your first agent/LLM application without sending prompts or data to an external AI provider.

## 3. Production LLM Serving — vLLM

### Resource

[vLLM Documentation](https://docs.vllm.ai/)

### Learn

- Production model serving
- OpenAI-compatible APIs
- Continuous batching
- GPU utilization
- Parallelism
- Throughput
- Model deployment
- Serving multiple users

### Architecture

```text
Agents
  │
  ▼
Model Gateway
  │
  ▼
Load Balancer
  │
 ┌┴──────────────┐
 ▼               ▼
vLLM Node 1   vLLM Node 2
 GPU              GPU
```

## 4. Local Embeddings + RAG

### Learn

- Local embedding models
- Document ingestion
- Chunking
- Metadata
- Local indexing
- Retrieval
- Reranking
- Grounding
- Citations
- Access-controlled retrieval

### Architecture

```text
Enterprise Documents
        ↓
Document Processing
        ↓
Local Embedding Model
        ↓
Vector Store
        ↓
Retriever
        ↓
Agent / LLM
        ↓
Grounded Response
```

### Important architecture rule

Sensitive documents should remain inside the approved enterprise security boundary unless policy explicitly allows external processing.

## 5. Local Vector Stores

Choose one initially. Do not try to master all products.

### Option A — PostgreSQL + pgvector

[pgvector](https://github.com/pgvector/pgvector)

Good when PostgreSQL already exists in the enterprise architecture and the scale/use case fits it.

### Option B — Qdrant

[Qdrant Documentation](https://qdrant.tech/documentation/)

Dedicated vector search platform suitable for self-hosting.

### Also understand

- Elasticsearch / OpenSearch vector capabilities
- Backup/restore
- Replication
- Metadata filtering
- Tenant isolation
- Access control

## 6. Model Gateway & Provider Independence

This is one of the most important architecture topics.

### Goal

Business agents should not depend directly on a specific model vendor.

```text
BPMN Agent ───────┐
DDC Agent ────────┼──► Model Abstraction / Gateway
Document Agent ───┘             │
                         ┌───────┼────────┐
                         ▼       ▼        ▼
                       vLLM    Ollama   Cloud
```

### Learn

- Provider abstraction
- Model routing
- Fallback
- Timeouts
- Retry policies
- Model capability metadata
- Cost/latency routing
- Configuration-driven provider selection
- Provider health checks

### Architect question

Can the organization move from a cloud model to an on-prem model without redesigning its business agents?

The target answer is **yes**.

## 7. Microsoft Agent Framework + Local Models

### Resources

[Microsoft Agent Framework](https://learn.microsoft.com/en-us/agent-framework/)

[Microsoft Agent Framework — Get Started](https://learn.microsoft.com/en-us/agent-framework/get-started/)

### Learn

Keep these layers independent:

```text
Agent
 ├── Instructions
 ├── Skills
 ├── Tools
 ├── RAG
 ├── Workflow
 └── Model Interface
          │
          ▼
    Local or Cloud Model
```

### Build

Run the same agent against at least two model configurations where practical:

```text
Configuration A → Local model
Configuration B → Cloud model
```

The business agent should require minimal or no redesign.

## 8. On-Prem AI Security

### Resource

[OWASP GenAI Security Project](https://genai.owasp.org/)

### Learn

- Network segmentation
- Authentication
- RBAC
- Least privilege
- Service identities
- Secrets
- TLS
- Data encryption
- Prompt injection
- Tool authorization
- Sensitive data handling
- Tenant isolation
- Model access controls
- Audit logging

### Architecture

```text
User
 ↓
Authentication
 ↓
Authorization
 ↓
Agent
 ↓
Guardrails
 ↓
Tool Policy
 ↓
Enterprise Authorization
 ↓
Business API
```

### Rule

Running the model on-prem does **not** automatically make the AI solution secure.

## 9. Air-Gapped / Restricted Environments

### Learn

Design systems that can operate without public Internet access.

Consider:

- Model distribution
- Container image distribution
- Package/dependency mirrors
- OS updates
- Vulnerability scanning
- Model updates
- Offline documentation
- Certificate management
- License dependencies
- Backup/restore
- Security patch process

### Architecture

```text
INTERNET
   X
   X  No runtime dependency
   X
┌─────────────────────────┐
│ Enterprise Datacenter   │
│                         │
│ Agents                  │
│ Local LLM               │
│ Local RAG               │
│ Local Vector DB         │
│ Local Monitoring        │
│ Local Package Registry  │
└─────────────────────────┘
```

## 10. GPU & Capacity Planning

### Learn

- VRAM
- Model size
- Quantization
- Context length
- Batch size
- Tokens/sec
- Requests/sec
- Concurrent users
- KV cache
- GPU memory utilization
- Latency targets
- Horizontal vs vertical scaling

### Capacity flow

```text
Business Load
      ↓
Concurrent Users
      ↓
Requests / second
      ↓
Average Input Tokens
      ↓
Average Output Tokens
      ↓
Latency SLA
      ↓
Model Selection
      ↓
GPU / Node Sizing
```

### Architect requirement

Do not recommend a model/GPU architecture based only on model parameter count. Size against measurable workload and SLA assumptions.

## 11. Local Observability + Evaluation

### Resources

[OpenTelemetry .NET](https://opentelemetry.io/docs/languages/dotnet/)

[OpenTelemetry GenAI Semantic Conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/)

### Monitor

- Request latency
- Model latency
- Tokens
- Throughput
- GPU utilization
- Queue depth
- Tool calls
- Retrieval latency
- Retrieval quality
- Errors
- Timeouts
- Agent decisions
- Human approvals
- Model/version
- Prompt/version

### Separate

```text
Technical Telemetry
        │
        ├── latency
        ├── errors
        └── infrastructure

AI Decision Trace
        │
        ├── model
        ├── retrieval
        ├── tools
        └── reasoning outcome

Business Audit Trail
        │
        ├── user
        ├── approval
        ├── action
        └── before / after
```

## 12. High Availability & Disaster Recovery

### Learn

- Multiple inference nodes
- Load balancing
- Health checks
- Failover
- Queue-based load leveling
- Vector-store replication
- Database HA
- Model artifact availability
- Backup/restore
- RPO/RTO
- Graceful degradation

### Architecture

```text
                 Load Balancer
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
       Inference Node 1    Inference Node 2
             │                   │
             └─────────┬─────────┘
                       ▼
                 Agent Platform
                       │
               ┌───────┴────────┐
               ▼                ▼
          Vector Store       Business APIs
```

### Failure question

If the LLM infrastructure is unavailable, what business processes can continue deterministically?

An architect should define this explicitly.

## 13. Hybrid AI Architecture

Not every deployment needs to be 100% local.

Learn how to support policies such as:

```text
Sensitive / regulated request
          ↓
       Local LLM

Non-sensitive approved request
          ↓
       Cloud LLM
```

### Architecture

```text
                  Agent Platform
                       │
                  Model Router
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
       ON-PREMISE              CLOUD
        vLLM                  AI Provider
          │
       Local Model

Policy decides routing.
```

### Learn

- Data classification
- Policy-based routing
- Provider allowlists
- Model capability routing
- Cloud fallback rules
- Local fallback rules
- Residency requirements
- Audit of provider/model selection

# 14. Final On-Prem Agentic AI Reference Architecture

Your final architecture should demonstrate:

- .NET application/API layer
- Microsoft Agent Framework
- Agent orchestration
- Agent Skills
- RAG
- Local embeddings
- Local vector store
- Local LLM serving
- Model/provider abstraction
- Enterprise APIs
- BPMN/workflow integration
- HITL
- Authentication/authorization
- Guardrails
- Auditability
- OpenTelemetry
- Evaluation
- GPU sizing assumptions
- HA/DR
- Air-gap considerations
- Hybrid deployment option

```text
                         USERS
                           │
                           ▼
                     API GATEWAY
                           │
                           ▼
                  AGENTIC AI PLATFORM
                           │
       ┌───────────────────┼───────────────────┐
       ▼                   ▼                   ▼
 ORCHESTRATOR             RAG              WORKFLOWS
       │                   │                 + HITL
   ┌───┼───┐               ▼                   │
   ▼   ▼   ▼          VECTOR STORE              │
 Agents / Skills            │                   │
   │                        │                   │
   └──────────────┬─────────┴───────────────────┘
                  ▼
            MODEL GATEWAY
                  │
       ┌──────────┴──────────┐
       ▼                     ▼
 ON-PREMISE MODEL        OPTIONAL CLOUD
  vLLM / Ollama            PROVIDER
       │
       ▼
 Enterprise Integration Layer
       │
 ┌─────┼──────┬──────┬──────┐
 ▼     ▼      ▼      ▼      ▼
Core  BPMN   CRM    DDC   Documents

────────────────────────────────────────────
Security │ Audit │ OTel │ Evaluation │ HA/DR
────────────────────────────────────────────
```

# Practical Project

Build an **On-Prem Enterprise Knowledge + Process Agent**.

Minimum capabilities:

1. Run an LLM locally.
2. Use local embeddings.
3. Store/retrieve knowledge locally.
4. Implement a .NET agent.
5. Call a real enterprise-style API/tool.
6. Add a human approval step.
7. Record an audit trail.
8. Add OpenTelemetry.
9. Test model/API/vector-store failures.
10. Demonstrate switching between local and cloud model providers without changing the business use case.

# Architect Completion Criteria

You should be able to answer:

- Why on-prem instead of cloud?
- Which data is allowed to leave the datacenter?
- Which model should be selected and why?
- How many GPUs/nodes are required?
- What happens when the model server fails?
- How does RAG remain tenant- and role-aware?
- How are tools authorized?
- How are AI decisions audited?
- How does HITL work after a restart?
- How is the system monitored?
- How are models updated in an air-gapped environment?
- How can the organization change model/provider later?
- Which parts remain deterministic if AI is unavailable?

When you can design and defend these decisions, you are thinking at **enterprise AI architect level**, not only at AI application developer level.
