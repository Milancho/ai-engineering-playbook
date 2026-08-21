# AI Engineering Playbook

A documentation-first workspace for designing, planning, building, reviewing, and evolving AI-enabled software products.

The repository has two complementary purposes:

1. **Product Engineering** — define what to build, why it matters, how it is architected, and how scope is decomposed into executable work.
2. **AI Engineering** — define how AI-assisted engineering agents and tools plan, implement, review, test, document, and maintain the software consistently.

The first product being explored with this playbook is a reusable **AI Platform / Connected Intelligence Platform**, initially researched through financial-services and document-intelligence use cases. The playbook itself is intended to remain reusable for other products as well.

## Current Status

**Discovery / Product Definition**

Current work focuses on research and the living Product Requirements Document (PRD). Architecture, MVP scope, EPICs, Features, Issues, and implementation will follow after the product direction is sufficiently stable.

Start here: [`docs/product/AI-Platform-PRD.md`](docs/product/AI-Platform-PRD.md)

## Source of Truth

Markdown documentation in this repository is the source of truth for the full product and engineering picture.

GitHub Issues are the execution layer. They will be used for Features, implementation Tasks, technical Spikes, and Bugs once the corresponding scope has been documented and approved.

Planning lifecycle:

```text
Research
  -> PRD
  -> Architecture
  -> MVP
  -> EPIC
  -> Feature
  -> GitHub Issue
  -> Code / Tests / PR
```

### Documentation-first planning rule

Major product scope, architecture, EPIC, and Feature decisions must first be reflected in the relevant Markdown documentation. Issues should link back to the relevant product or engineering context.

### No orphan tasks

Implementation tasks must belong to a defined Feature or clearly documented technical capability. Features must belong to an EPIC or explicitly defined MVP capability.

## Repository Language

**English is mandatory for all repository artifacts and engineering communication stored in the repository.**

This includes:

- Documentation and research notes
- PRDs, architecture, MVP, EPICs, and Features
- GitHub Issues and acceptance criteria
- Pull requests and reviews
- Commit messages
- Source code and identifiers
- Code comments
- Tests and developer-facing logs
- AI prompts and agent instructions
- Skills, hooks, commands, workflows, and guardrails
- Planner, Worker, and Reviewer outputs

Conversation outside the repository may use other languages, but anything committed to or recorded in the repository must be normalized to professional English.

## Product Engineering

Planned documentation areas include:

```text
docs/
  product/       Product requirements and product decisions
  research/      Research evidence, experiments, and references
  architecture/  Technical architecture and architectural decisions
  planning/      MVP, EPICs, Features, and planning views
```

The PRD describes **what and why**. Architecture will describe **how**. MVP documentation will define **what is built first**. EPICs and Features will provide the readable product breakdown, while GitHub Issues will manage execution.

## AI Engineering

The repository will progressively define a reusable AI Engineering system covering areas such as:

- Planner / Worker / Reviewer roles
- Agent skills
- Agent harness
- Hooks
- Guardrails
- Commands
- Context management
- Tool contracts and permissions
- Coding and documentation standards
- Testing and evaluation
- Security constraints
- Failure, retry, and escalation behavior
- Human approval points
- GitHub and pull-request workflows
- Multi-agent collaboration
- Provider-specific integration for tools such as ChatGPT, Claude, and GitHub Copilot

These artifacts will be created incrementally as real product development exercises them. The repository will not create a large speculative agent framework before it is needed.

### Shared truth across AI tools

ChatGPT, Claude, Copilot, and future AI engineering tools should consume the same product documentation, architecture, engineering rules, and acceptance criteria. Provider-specific instructions may adapt how a tool operates, but must not create a separate product or architecture truth.

A target engineering flow is:

```text
Feature
  -> Planner
  -> Implementation Plan
  -> Worker
  -> Code + Tests
  -> Reviewer
  -> Pass / Changes Required
  -> Pull Request
```

## Initial MVP Direction

The current preferred direction, still subject to discovery and review, is to start with a **standalone .NET AI/document-intelligence MVP using Syncfusion components**, rather than immediately coupling the experiment to a complete banking solution.

The prototype should be designed as reusable platform capability, not throwaway demo code. A later phase can integrate the proven capability with an existing SPA/.NET Core banking application and its authenticated business APIs.

This direction is documented as a hypothesis in the PRD and is not yet final committed scope.

## Working Principle

The model is not the product. The durable product is the governed platform around models: context, knowledge, tools, deterministic rules, security, audit, evaluation, and human control.
