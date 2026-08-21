# AI Platform — Product Requirements Document

**Version:** 0.1 (Living Draft)  
**Status:** Discovery / Product Definition  
**Last updated:** 2026-08-21

## 1. Executive Summary

The AI Platform is a reusable connected-intelligence platform for financial and enterprise applications. It is intended to embed AI directly into business workflows rather than provide a standalone chatbot.

The platform will combine application context, approved knowledge, document intelligence, deterministic business rules and calculations, AI reasoning, secure tools/actions, human approval, and full auditability.

The initial research is focused on financial-services use cases, especially AI-assisted loan processing and internal employee copilots, while keeping the underlying platform reusable beyond a single organization or product.

## 2. Vision

Build a shared AI intelligence layer that can support multiple applications, modules, users, and workflows through common AI capabilities.

The platform should enable three interaction models:

- **AI Assistant** — customer-facing self-service and guided actions.
- **AI Copilot** — employee-facing contextual analysis, validation, recommendations, and explanations.
- **AI Advisor** — advanced assistance for relationship managers and other expert roles, including preparation, recommendations, and follow-up.

The long-term goal is connected intelligence: AI capabilities share governed context, knowledge, tools, rules, and audit infrastructure instead of each business module implementing an isolated AI solution.

## 3. Problem Statement

Traditional enterprise AI implementations are often isolated chatbots with limited business context and limited ability to perform reliable actions. Financial applications require stronger guarantees around data access, deterministic calculations, business rules, traceability, explainability, and human control.

The platform must bridge the gap between LLM reasoning and trusted enterprise systems.

## 4. Goals

- Embed AI into existing business workflows and applications.
- Provide a shared AI layer reusable by multiple modules.
- Allow AI agents to access authorized business context through controlled tools/APIs.
- Process business documents and convert relevant content into structured data.
- Separate deterministic rules/calculations from probabilistic LLM reasoning.
- Provide structured findings, warnings, recommendations, and explanations.
- Support human-in-the-loop approval for consequential actions and decisions.
- Provide comprehensive auditability of AI requests, context, tool calls, outputs, and actions.
- Support approved knowledge sources and retrieval mechanisms.
- Design the platform so that AI providers/models can evolve without redesigning the business platform.

## 5. Non-Goals — Initial Scope

The initial platform is not intended to:

- Allow an LLM to independently make final consequential financial decisions.
- Use an LLM as the authoritative calculator for deterministic financial calculations.
- Allow unrestricted direct access from an LLM to production databases or services.
- Build a separate AI stack for every business module.
- Depend completely on one AI model or one document-processing vendor.

These non-goals will be reviewed as the product definition evolves.

## 6. Users and Personas

### 6.1 Customer
Uses an AI Assistant for self-service, information, guided processes, and permitted secure actions.

### 6.2 Internal Employee
Uses an AI Copilot inside the business application to understand customer/application context, analyze information, find relevant knowledge, identify missing information, and receive recommendations.

### 6.3 Relationship Manager / Expert User
Uses an AI Advisor for customer preparation, contextual recommendations, opportunity identification, meeting support, summaries, and follow-up.

### 6.4 Administrator / Governance User
Configures access, knowledge sources, tools, policies, models, monitoring, and audit controls.

## 7. Core AI Capabilities

### 7.1 AI Assistant
Customer-facing conversational and action-oriented AI with controlled access to authorized customer data and services.

### 7.2 AI Copilot
Context-aware internal assistant embedded in enterprise applications. Initial research indicates this is a strong candidate for the first product/MVP focus because it allows human oversight while delivering immediate operational value.

### 7.3 AI Advisor
Advanced expert assistance that combines customer context, historical information, products, knowledge, and recommendations.

### 7.4 Document Intelligence
The platform should support documents such as PDF, Word, Excel, images, reports, and other relevant business formats. Document processing should produce structured information suitable for validation, rules, workflows, and AI reasoning.

### 7.5 Structured AI Output
Where appropriate, AI results should be machine-readable rather than only free-form text. Examples include findings, validation results, warnings, recommendations, confidence/provenance metadata, and explanations.

## 8. Initial Use Case — Loan Application Copilot

A user opens a loan application in the business application. The AI Copilot receives only the context the user is authorized to access and can combine:

- Customer information
- Loan application data
- Financial information
- Attached documents
- Existing obligations and relevant history
- Approved policies and product knowledge
- Configured business rules

A conceptual processing flow is:

1. Retrieve authorized loan and customer context through APIs/tools.
2. Retrieve relevant application documents.
3. Extract required document information into structured data.
4. Validate extracted data and required fields.
5. Execute deterministic business rules and financial calculations.
6. Use AI reasoning to interpret the combined evidence where appropriate.
7. Return structured findings, warnings, recommendations, and explanations.
8. Present results to the responsible user for review/decision.
9. Store an auditable record of the analysis and actions.

Example questions may include:

- What documents are missing?
- Are there inconsistencies between application data and submitted documents?
- Which configured rules failed?
- Why did a rule fail?
- Summarize this loan application.
- What should the credit officer review before making a decision?

## 9. Connected Intelligence

The platform should avoid isolated AI implementations for individual modules. Shared platform capabilities should include:

### Context
Authorized business context from systems such as customer, CRM, loans, accounts, DDC/DAC, workflows, and other application modules.

### Knowledge
Approved documents, policies, procedures, product information, and other governed knowledge sources.

### Tools
Controlled operations exposed to AI agents through application APIs and specialized tools.

### Rules
Deterministic validation, business rules, eligibility logic, and calculations implemented outside the LLM.

### Intelligence
LLM/AI capabilities used for language understanding, reasoning where appropriate, summarization, explanation, classification, and recommendations.

## 10. Agent and Orchestration Layer

The platform is expected to require an orchestration layer responsible for coordinating context, knowledge retrieval, document tools, business tools, rules, models, permissions, and audit.

The exact agent framework and orchestration architecture are intentionally **TBD** during discovery.

## 11. Document Processing

Document processing is a major platform capability under evaluation.

### Syncfusion Document SDK AI Tools

Syncfusion Document SDK AI Agent Tools are currently being evaluated as a possible **Document Tool Layer**. Potential responsibilities include document extraction, processing, conversion, form/document data extraction, redaction, and related document operations.

Syncfusion should not currently be treated as the entire AI platform. The working hypothesis is that it can provide specialized document tools callable by the orchestration/agent layer.

**Status:** Under Evaluation

## 12. Business Rules and Deterministic Calculations

A core design principle is separation between deterministic business logic and LLM reasoning.

Examples such as financial ratios, eligibility thresholds, date calculations, fees, repayment calculations, and policy rules should be executed by deterministic code/rule services where possible.

The LLM may explain results, combine evidence, summarize findings, or help a user understand why a deterministic rule produced a result, but it should not become the authoritative calculation engine.

## 13. Integrations — Candidates

Potential integration categories include:

- Core business APIs
- Customer and CRM
- Loan applications
- Document repositories
- DDC/DAC or dynamic-data services
- BPM/workflow engines
- API platforms/integration layers
- Communication/social channels
- Reporting and analytics
- Identity and access management

Specific integrations are **TBD** and should be selected based on the MVP.

## 14. Security, Governance and Trust

The platform must be designed for enterprise and financial-services controls. Requirements under investigation include:

- Authentication and authorization
- User-context-aware data access
- Tool/action authorization
- Data protection
- Approved knowledge sources
- Prompt/context protection
- Hallucination risk controls
- Human-in-the-loop approval
- Explainability
- Audit trail
- Model/provider governance
- Sensitive-data handling
- Environment separation
- Monitoring and observability

Detailed requirements are **TBD**.

## 15. Functional Requirements

Detailed functional requirements will be created after additional discovery. Initial capability areas are:

- Context retrieval
- Knowledge retrieval
- Document processing
- Structured extraction
- Validation
- Rule execution
- AI reasoning
- Structured recommendations/findings
- Explanations
- Tool/action execution
- Human approval
- Audit and traceability

## 16. Non-Functional Requirements

To be defined during architecture and MVP definition. Areas to cover include:

- Security
- Performance
- Scalability
- Availability
- Reliability
- Observability
- Auditability
- Extensibility
- Model portability
- Data privacy
- Cost controls

## 17. MVP

**Status: Not yet finalized.**

Current leading candidate:

> An internal AI Copilot embedded in a loan-application workflow that can obtain authorized application context, analyze attached documents, execute deterministic validations/rules, and present structured findings and explanations to a human user.

The MVP will be finalized after the remaining research and architecture analysis.

## 18. Future / Phase 2+

Candidate capabilities include:

- Customer AI Assistant
- AI Advisor
- Additional financial modules
- Cross-module Customer 360 intelligence
- Advanced workflow actions
- Multi-agent scenarios
- Additional document types and document workflows
- Broader knowledge/RAG capabilities
- Proactive intelligence and recommendations

These are hypotheses, not committed scope.

## 19. Open Questions

- What exact business scenario should be the MVP?
- Which AI/LLM provider(s) should be supported initially?
- Cloud, on-premises, or hybrid deployment?
- Which agent/orchestration framework should be used?
- What responsibilities should Syncfusion AI Agent Tools own?
- What document types and extraction scenarios are required for MVP?
- How should the Rules Engine be designed/integrated?
- What knowledge/RAG architecture is required?
- What forms of agent memory are required, if any?
- Which actions can AI execute automatically versus requiring approval?
- What level of explainability/provenance is required per output?
- What audit information must be persisted?
- Which core systems should be integrated first?
- What evaluation metrics will determine MVP success?

## 20. Decision Log

| ID | Decision / Hypothesis | Status | Rationale |
|---|---|---|---|
| D-001 | Build a shared AI platform rather than isolated module-specific AI solutions. | Proposed | Supports connected intelligence and reuse. |
| D-002 | Keep deterministic financial calculations and business rules outside the LLM. | Proposed | Reliability, testability, auditability, and repeatability. |
| D-003 | Evaluate Syncfusion Document SDK AI Tools as the Document Tool Layer. | Under Evaluation | Specialized document-processing capabilities may be exposed as agent tools. |
| D-004 | Use human-in-the-loop for consequential financial decisions/actions. | Proposed | Governance, risk control, and accountability. |
| D-005 | Treat AI models as replaceable platform dependencies rather than the product itself. | Proposed | Reduces model/vendor coupling and supports future evolution. |
| D-006 | Prioritize AI Copilot as the leading MVP interaction model. | Proposed | Internal usage offers high value with stronger human oversight. |

## 21. Research and References

### Research reviewed

1. **BKT AI demonstration — Video 1** — initial analysis of AI-assisted loan/application workflow and structured recommendations.
2. **BKT AI demonstration — Video 2** — pending detailed analysis.
3. **BKT AI demonstration — Video 3** — pending detailed analysis.
4. **Syncfusion — AI-Powered Development with Document SDK AI Tools** — evaluation in progress.
5. **Fintech News Switzerland — AI in Banking: From Chatbots to Connected Intelligence** — Assistant/Copilot/Advisor model and connected-intelligence strategy.

### Working research principle

Research references provide inspiration and evidence. They do not automatically become product requirements. Each material change should be evaluated before being incorporated into committed MVP scope.

---

## Change Log

### v0.1 — 2026-08-21

- Created initial living PRD.
- Added Connected Intelligence concept.
- Added Assistant / Copilot / Advisor interaction models.
- Added initial Loan Application Copilot use case.
- Added Document Intelligence and Syncfusion evaluation.
- Established deterministic rules/calculations versus LLM reasoning principle.
- Added security/governance areas, open questions, decision log, and initial MVP hypothesis.
