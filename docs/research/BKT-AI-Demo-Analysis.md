# BKT AI Demonstration — Consolidated Analysis

**Source:** One AI demonstration split into three parts for analysis  
**Status:** Reviewed research / reference implementation pattern  
**Purpose:** Capture useful product patterns demonstrated in the loan/credit workflow without treating the demonstration as a specification to clone.

## Demonstrated Pattern

The demonstration shows an AI Copilot embedded in a loan/credit-analysis workflow rather than an isolated chatbot. The observed pattern combines business commands, document processing, structured financial data, deterministic warnings/calculations, drafting, workflow awareness, and human review.

```mermaid
flowchart TD
    A[Loan / Credit Case] --> B[AI Copilot]
    B --> C[Domain Commands / Skills]
    C --> D[Document Extraction / OCR]
    D --> E[Structured Case Data]
    E --> F[Financial Spreading]
    F --> G[Deterministic Calculations / Rules]
    G --> H[Warnings / Findings]
    H --> I[AI Analysis / Explanation / Draft]
    I --> J[Human Review / Decision]
```

## Domain Commands / Skills

The Copilot exposes task-oriented actions rather than relying only on free-form chat. Observed examples include document extraction, ID/business-registration extraction, financial-statement OCR, document classification, missing-document checks, financial analysis, draft credit proposal/conclusion, executive summary, suggested conditions, and authority explanation.

### Product interpretation

A future Copilot should support governed **domain skills/commands** with explicit inputs, permissions, outputs, and audit behavior.

## Document-to-Case Pattern

A strong pattern is that document processing should not end with a prose response. Extracted information should become structured data that can be validated and, after appropriate review, applied to the business case.

```mermaid
flowchart LR
    A[Document] --> B[Extract]
    B --> C[Structured Schema]
    C --> D[Validate]
    D --> E[Review]
    E --> F[Apply to Case]
```

## Required Documents and Missing-Document Detection

The demonstration includes required-document/checklist behavior. Our interpretation is that authoritative document requirements should come from product/rule configuration, while AI can assist with classification, matching, extraction, explanation, and detection of missing evidence.

```mermaid
flowchart TD
    A[Product / Case Context] --> D[Required Document Rules]
    B[Uploaded Documents] --> E[Classification / Matching]
    D --> F[Checklist Comparison]
    E --> F
    F --> G[Missing / Present / Needs Review]
```

## Financial Spreading

The demonstration shows financial-statement processing feeding structured financial information and analysis. This is especially relevant to the proposed standalone MVP.

```mermaid
flowchart TD
    A[Financial Statement] --> B[OCR / Extraction]
    B --> C[Structured Financial Model]
    C --> D[Financial Spreading]
    D --> E[Deterministic Ratios]
    E --> F[Rules / Warnings]
    F --> G[AI Explanation]
    G --> H[Human Review]
```

## Deterministic Rules vs AI Reasoning

Warnings such as threshold breaches or ratio checks reinforce the design principle that calculations and deterministic policy checks should be executed outside the LLM. AI can explain, summarize, correlate evidence, and help the user understand the result.

## Workflow Awareness

The demonstration includes workflow gates/blockers. This suggests a useful future integration pattern: AI may inspect workflow state and assist users in resolving blockers, but the workflow engine remains authoritative for process state and execution.

```mermaid
flowchart LR
    A[Workflow Engine] --> B[Gate / Blocker State]
    B --> C[AI Copilot]
    C --> D[Explanation / Suggested Next Action]
    D --> E[Human / Authorized Action]
    E --> A
```

## Draft, Review, Decide

The use of draft proposals, conclusions, and summaries supports a human-in-the-loop pattern for consequential work.

```mermaid
flowchart LR
    A[Evidence + Rules] --> B[AI Draft / Recommendation]
    B --> C[Human Review]
    C --> D[Edit / Approve / Reject]
    D --> E[Authoritative Business Action]
```

## MVP Relevance

The strongest standalone vertical slice derived from the demonstration is **Document & Financial Intelligence**: ingest financial documents, extract structured data, validate it, calculate deterministic ratios, run rules, produce AI explanations/findings, and capture human review/audit.

This validates platform capabilities without requiring immediate coupling to a complete banking application.
