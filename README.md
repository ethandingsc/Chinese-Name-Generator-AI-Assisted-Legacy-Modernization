# Chinese Name Generator — AI-Assisted Legacy Modernization

> Modernizing a legacy Python web application into a maintainable, deployment-ready **Next.js + TypeScript** full-stack system through an **AI-assisted development workflow with Claude Code**.

[English](README.md) | [中文](README_ZH.md)

This project is a practical experiment in **AI-assisted software engineering and Vibe Coding**. Rather than asking an AI coding agent to generate a new application from scratch, the goal is to migrate an existing Python-based Chinese name generation application to a modern web stack while preserving its original behavior.

A key challenge is ensuring that the migration does not silently alter the existing business logic. To address this, the project uses a **Golden Fixture–driven regression testing strategy**, with **126 baseline cases** generated from the original Python implementation.

> **Implementation Note:** The core name-generation algorithm and domain-specific rule engine are proprietary and intentionally excluded from the public repository.

---

## Overview

The original application was implemented in Python and contained both web-layer code and domain-specific name-generation logic.

The modernization has three primary objectives:

* Migrate the application from **Python to TypeScript**
* Rebuild the web application using **Next.js + React**
* Preserve existing behavior through **automated regression testing**

The final application is designed to run entirely on the **Node.js / TypeScript stack**, without requiring a Python runtime in production.

### Tech Stack

| Area            | Technology                  |
| --------------- | --------------------------- |
| Framework       | Next.js                     |
| Frontend        | React                       |
| Language        | TypeScript                  |
| Styling         | Tailwind CSS                |
| Server          | Next.js Server / API Routes |
| Testing         | Vitest                      |
| Runtime         | Node.js                     |
| AI Coding Agent | Claude Code                 |
| Version Control | Git / GitHub                |

---

## Architecture

The migrated application follows a simple full-stack architecture:

```text
User
 │
 ▼
Next.js / React UI
 │
 ▼
Server / API Routes
 │
 ▼
TypeScript Business Logic
 │
 ▼
Name Generation Engine
 │
 ▼
Structured Results
 │
 ▼
React UI
```

The core business logic is separated from the UI and API layers so that each component can be independently maintained and tested.

---

## Legacy Modernization

This project is not a direct syntax translation from Python to TypeScript.

The migration process follows a structured workflow:

```text
Legacy Python Application
          │
          ▼
Codebase Analysis
          │
          ▼
Golden Fixture Generation
          │
          ▼
Next.js + TypeScript Architecture
          │
          ▼
Business Logic Migration
          │
          ▼
Regression Testing
          │
          ▼
API + Frontend Integration
          │
          ▼
Production Build Validation
          │
          ▼
Deployment Ready
```

The objective is to modernize the application's architecture **without redesigning or unintentionally changing the original business behavior**.

---

## Golden Fixture Regression Testing

One of the main risks when migrating a legacy application is introducing subtle behavioral changes.

Before rewriting the core logic, the original Python implementation was used to generate:

> **126 Golden Fixture baseline cases**

These cases capture representative outputs from the legacy implementation.

The validation process compares:

```text
Legacy Python Implementation
            │
            ▼
       Fixed Inputs
            │
            ▼
      Golden Outputs
```

against:

```text
New TypeScript Implementation
            │
            ▼
       Same Inputs
            │
            ▼
       New Outputs
            │
            ▼
   Regression Comparison
```

This allows the TypeScript migration to be evaluated against known legacy behavior rather than relying only on manual inspection.

The Golden Fixture therefore acts as a behavioral contract between the legacy and modern implementations.

---

## AI-Assisted Development

Claude Code is used as an **AI coding agent** throughout the migration process.

Its responsibilities include:

* Exploring the existing codebase
* Understanding legacy dependencies
* Scaffolding the Next.js application
* Migrating Python modules to TypeScript
* Implementing server/API routes
* Building React components
* Writing automated tests
* Running regression tests
* Debugging build and runtime failures
* Refactoring project structure
* Performing production build validation

The project is intentionally not treated as a fully autonomous AI coding exercise.

Critical changes remain subject to human review.

### Human-in-the-Loop Workflow

```text
Requirement / Existing Behavior
             │
             ▼
      Claude Code Agent
             │
             ▼
     Implementation / Diff
             │
        ┌────┴────┐
        │         │
        ▼         ▼
 Automated     Human Review
   Tests       for Critical
                 Changes
        │         │
        └────┬────┘
             ▼
        Verification
             │
             ▼
           Commit
```

Human review is prioritized for:

* Core business logic changes
* Unexpected or suspicious diffs
* Destructive file operations
* Architectural decisions
* Regression failures
* Dependency changes
* Final acceptance of migrated behavior

This creates a workflow closer to **AI-assisted software engineering** than simply accepting generated code.

---

## Vibe Coding Experiment

This project also serves as a practical exploration of **Vibe Coding with an agentic development tool**.

The experiment focuses on a simple question:

> **How far can an AI coding agent take a real legacy application migration while still producing software that can be tested, built, maintained, and eventually deployed?**

The emphasis is therefore not on how much code the AI can generate.

Instead, the focus is on whether the resulting system is:

* Functionally correct
* Behaviorally consistent
* Testable
* Maintainable
* Buildable
* Deployment-ready

The workflow follows the principle:

```text
AI Implements
     │
     ▼
Tests Verify
     │
     ▼
Human Reviews Critical Changes
     │
     ▼
Failures Are Diagnosed
     │
     ▼
AI Iterates
     │
     ▼
Final Verification
```

---

## Debugging Workflow

When failures occur, the project follows a structured debugging process:

```text
Symptom
   ↓
Hypothesis
   ↓
Validation
   ↓
Fix
   ↓
Re-test
```

The goal is to avoid treating AI-generated patches as automatically correct.

A fix is only considered complete after the relevant tests or build steps have been rerun successfully.

---

## Project Structure

The modernized application follows a conventional Next.js project structure:

```text
.
├── app/
│   ├── api/                 # Server / API routes
│   └── ...                  # Next.js application routes
│
├── components/              # Reusable React components
│
├── lib/
│   ├── ...                  # Application logic
│   └── types/               # Shared TypeScript types
│
├── public/                  # Static assets
│
├── tests/
│   └── fixtures/            # Regression test fixtures
│
├── package.json
├── tsconfig.json
└── README.md
```

The exact structure may evolve during the migration as responsibilities are separated and the application architecture is refined.

---

## Privacy & Repository Scope

The original application contains a private name-generation engine with domain-specific rules, datasets, filtering strategies, and generation logic.

Those implementation details are intentionally excluded from the public repository.

The public project focuses on demonstrating:

* Full-stack Next.js architecture
* TypeScript migration
* API design
* Frontend implementation
* Regression testing methodology
* Legacy modernization workflow
* AI-assisted development
* Production build and deployment practices

Conceptually:

```text
Public Application
       │
       ├── Next.js UI
       ├── API Interface
       ├── Type Definitions
       ├── Testing Infrastructure
       └── Project Documentation
                │
                ▼
       Private Generation Engine
                │
                ▼
          Generated Results
```

This separation allows the engineering workflow and application architecture to be documented without exposing the underlying proprietary generation rules.

---

## Validation Criteria

The migration is considered complete only when the application passes the following checks.

### Development

```bash
npm install
npm run dev
```

The development server must start successfully and the application must be usable through the browser.

### Type Safety

The TypeScript codebase must pass type checking without unresolved errors.

### Automated Tests

```bash
npm test
```

Unit and regression tests must pass, including comparison against the Golden Fixture baseline.

### End-to-End Behavior

The complete application flow must work:

```text
User Input
    ↓
Frontend
    ↓
API
    ↓
Generation Logic
    ↓
Structured Response
    ↓
Frontend Rendering
```

Both valid and invalid inputs should be handled correctly.

### Production Build

```bash
npm run build
```

The application must successfully produce a production build.

### Production Runtime

The final application must not require:

* Python
* Flask
* FastAPI
* Streamlit
* Local Python scripts
* Hard-coded Windows paths

The production application should run using the Node.js ecosystem alone.

---

## Deployment

The initial milestone is a fully working local production build.

The architecture is designed with future deployment in mind, including:

* Environment-variable-based configuration
* No dependency on local machine paths
* No Python runtime dependency
* Standard Next.js build workflow
* Compatibility with modern deployment platforms such as Vercel

A public deployment may be added after the migration and regression validation are complete.

---

## Current Progress

| Stage                         | Status         |
| ----------------------------- | -------------- |
| Legacy codebase preparation   | ✅ Complete     |
| Git migration checkpoint      | ✅ Complete     |
| Golden Fixture generation     | ✅ Complete     |
| 126 baseline regression cases | ✅ Complete     |
| Next.js + TypeScript setup    | 🔄 In Progress |
| Core logic migration          | ⏳ Planned      |
| API implementation            | ⏳ Planned      |
| Frontend implementation       | ⏳ Planned      |
| Vitest regression validation  | ⏳ Planned      |
| Production build              | ⏳ Planned      |
| Deployment                    | ⏳ Future       |

---

## Engineering Priorities

The project follows a strict priority order:

```text
Functional Correctness
        >
Behavior Preservation
        >
Engineering Structure
        >
User Experience
        >
Visual Effects
```

A visually polished interface is useful, but it does not compensate for incorrect migrated behavior.

---

## What This Project Explores

Beyond the application itself, this project explores several broader software engineering questions:

**Legacy Modernization**

How can an existing application be migrated between technology stacks without losing behavioral consistency?

**Regression Testing**

Can Golden Fixtures provide a practical safety net when rewriting domain-specific business logic?

**AI Coding Agents**

Which parts of a real software migration can an AI coding agent handle effectively?

**Human Oversight**

Which operations still require careful human review when working with an autonomous coding agent?

**Vibe Coding**

Can Vibe Coding move beyond rapid prototyping and produce software that actually passes tests, builds successfully, and is suitable for deployment?

---

## Lessons & Observations

One early observation from the project is that AI-generated changes still require validation.

Coding agents can rapidly explore repositories, generate implementations, run tests, and iterate on failures. However, generated diffs can occasionally contain incorrect assumptions or unintended modifications.

This makes automated tests and selective human review especially important when modifying existing systems.

The working principle for this project is therefore:

> **Use AI to increase development speed, but use tests and review to establish correctness.**

---

## Future Work

After the migration is stable:

* Complete full Golden Fixture regression validation
* Improve responsive design
* Expand automated test coverage
* Add production error handling
* Deploy a public demo
* Add application screenshots
* Document migration results
* Summarize lessons learned from the AI-assisted workflow

---

## About This Repository

This repository is both a web modernization project and an experiment in modern AI-assisted development.

The goal is not to demonstrate that an AI agent can generate large amounts of code.

The goal is to demonstrate a more useful engineering outcome:

> **Transforming a legacy Python application into a tested, maintainable, production-oriented Next.js + TypeScript system while using an AI coding agent as part of the development workflow.**
