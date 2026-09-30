```markdown
# Chinese Name Generator — AI Naming Agent

An AI-assisted Chinese naming system evolving from a legacy rule-based application into a structured, explainable Agent architecture.

Next.js · React · TypeScript · Python · FastAPI · Hybrid RAG · CrossEncoder · DeepSeek

## Overview

This project modernizes a legacy Chinese naming system into an AI-powered naming Agent while preserving its validated traditional naming rules.

The original Python/Streamlit prototype was first migrated to a maintainable Next.js + TypeScript application. The current phase extends the system with an independent Python Agent backend that combines deterministic naming rules, birth-chart analysis, retrieval-augmented generation (RAG), and LLM reasoning.

Instead of asking an LLM to generate names directly, the system separates **facts, constraints, retrieval, reasoning, and explanation** into different layers. Deterministic components handle rules that should remain stable, while the LLM is used where language understanding and flexible reasoning are valuable.

The long-term goal is an interactive naming Agent that can understand both traditional naming constraints and open-ended parental preferences, generate and rank candidates, explain its reasoning, and adapt across multiple rounds of conversation.

## Evolution

```text
Legacy Python / Streamlit Prototype
              ↓
Next.js + TypeScript Migration
              ↓
Golden Test Validation
              ↓
Python FastAPI Agent Backend
              ↓
Birth Information Analysis
              ↓
Structured Naming Conditions
              ↓
Hybrid RAG Retrieval
              ↓
Agent Reasoning + Candidate Ranking
              ↓
Explainable Name Recommendations
```

## Current Architecture

```text
                User
                  │
                  ▼
        Next.js / React Frontend
                  │
                  ▼
          FastAPI Agent Backend
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
 Birth Chart Tools     User Preferences
        │                   │
        └─────────┬─────────┘
                  ▼
        NamingConditionSet
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
 Legacy Naming Engine    Hybrid RAG
                        BM25 + Dense
                             │
                             ▼
                         RRF Fusion
                             │
                             ▼
                    CrossEncoder Rerank
        └────────────┬────────────┘
                     ▼
              Agent Reasoning
                     │
                  DeepSeek
                     │
                     ▼
        Candidate Names + Evidence
                     │
                     ▼
          Explainable Recommendation
```

## What I Built

- Migrated the original Python/Streamlit naming application to Next.js + TypeScript.
- Preserved legacy naming behavior using **126 Golden Test cases** generated from the original implementation.
- Built an independent **Python FastAPI Agent backend** and connected it with the existing Next.js application.
- Introduced structured request/response schemas, configuration management, logging, and unified error handling.
- Designed a structured **BirthChart → NamingConditionSet** pipeline for converting birth information into downstream naming constraints.
- Separated deterministic traditional rules from LLM reasoning instead of allowing the model to rewrite validated business logic.
- Added multi-provider birth-chart provenance and validation structures so disagreements can be recorded rather than silently resolved by an LLM.
- Built a **Hybrid RAG retrieval pipeline** combining BM25 lexical retrieval and dense semantic retrieval.
- Added **Reciprocal Rank Fusion (RRF)** followed by **CrossEncoder reranking** for higher-quality evidence retrieval.
- Preserved retrieval provenance including BM25, dense, RRF, and reranking scores/ranks for debugging and explanation.
- Integrated **DeepSeek** as the LLM layer while keeping model access isolated from deterministic naming components.
- Designed the next Agent layer to combine traditional constraints, retrieved knowledge, parental expectations, and multi-turn feedback.

## Agent Design Philosophy

The system follows one central principle:

> **Use deterministic code for facts and hard constraints; use the LLM for language understanding, preference interpretation, reasoning, and explanation.**

For example, the LLM should not independently calculate or overwrite birth-chart facts, stroke counts, or validated naming rules.

Instead, it receives structured information such as:

```text
Birth Information
      ↓
BirthChart
      ↓
NamingConditionSet
      ↓
Hard Constraints + Soft Preferences
      ↓
RAG Evidence
      ↓
LLM Reasoning
      ↓
Candidate Ranking / Explanation
```

This makes the system more controllable, testable, and explainable than direct LLM name generation.

## Hybrid RAG Pipeline

The retrieval layer uses a two-stage retrieval and reranking architecture:

```text
User / Agent Query
        │
        ├── BM25 Retrieval ───── Top 50
        │
        └── Dense Retrieval ──── Top 50
                    │
                    ▼
              RRF Fusion
                k = 60
                    │
                 Top 30
                    │
                    ▼
        BGE CrossEncoder Reranker
                    │
                  Top 5
                    │
                    ▼
            Final RAG Evidence
```

Current reranker:

```text
BAAI/bge-reranker-v2-m3
```

The retrieval trace retains:

- source / chunk provenance
- BM25 score and rank
- dense similarity score and rank
- RRF score and rank
- CrossEncoder score and final rank

This allows the Agent to explain not only what information was retrieved, but also how evidence reached the final reasoning context.

## Naming Condition Model

Instead of passing loosely structured text between components, the Agent uses a structured naming-condition layer.

Conceptually:

```text
NamingConditionSet
├── Birth-chart-derived conditions
├── Five-element preferences / constraints
├── Stroke and naming-rule constraints
├── Family / generational character requirements
├── Character avoidance rules
├── Parent preferences
├── Desired meaning and expectations
└── Provenance / confidence / validation metadata
```

This layer acts as the contract between deterministic tools, retrieval, and LLM reasoning.

A key design requirement is that different constraints are **not automatically treated as equally important**.

For example, a required generational character may conflict with an ideal five-element combination. The Agent should preserve the hard requirement while optimizing the remaining characters around it rather than simply rejecting the requirement.

## Multi-Turn Naming Agent

The target Agent is designed for iterative naming rather than one-shot generation.

```text
Initial Conditions
      ↓
Generate / Rank Candidate Set
      ↓
User Feedback
      ↓
Preference Extraction
      ↓
Update Soft Constraints / Weights
      ↓
Re-rank or Generate New Candidates
      ↓
Repeat
```

Later turns should therefore not simply rerun the first request with identical weights.

The Agent can progressively learn preferences such as:

- desired personality or temperament
- academic or intellectual expectations
- career and achievement aspirations
- prosperity or success-related meanings
- literary / classical style
- modern vs. traditional style
- preferred or disliked characters
- sound and rhythm preferences
- family-specific requirements
- other free-form requirements expressed in natural language

This is where the LLM provides the most value: translating open-ended human preferences into structured signals that the deterministic naming pipeline can use.

## Explainability

The final recommendation should expose the reasoning behind each candidate rather than returning only a name.

The planned explanation layer can combine:

```text
Traditional Naming Constraints
        +
Birth-Chart Conditions
        +
Character / Five-Element Contribution
        +
Family Requirements
        +
Semantic Meaning
        +
Parent Expectations
        +
Retrieved Cultural Evidence
        ↓
Final Name Explanation
```

Intermediate traces can also record how candidate names satisfy or compensate for individual conditions, providing a clearer path from the original birth information and user requirements to the final recommendation.

## Engineering Principles

- **Deterministic core first** — validated naming rules remain outside the LLM.
- **LLM where language matters** — preference understanding, semantic reasoning, interaction, and explanation.
- **Structured contracts** — components communicate through explicit schemas instead of uncontrolled prompts.
- **Traceability** — important retrieval and reasoning inputs preserve provenance.
- **Multi-turn adaptation** — user feedback changes subsequent ranking behavior.
- **Regression safety** — legacy behavior remains protected by Golden Tests.
- **Secret isolation** — external API credentials are loaded through environment variables and never committed.

## Tech Stack

| Layer | Stack |
| --- | --- |
| Frontend | Next.js · React · TypeScript · Tailwind CSS |
| Agent Backend | Python · FastAPI · Pydantic |
| LLM | DeepSeek |
| Retrieval | BM25 · Dense Retrieval |
| Fusion | Reciprocal Rank Fusion (RRF) |
| Reranking | BAAI/bge-reranker-v2-m3 |
| Traditional Logic | Legacy deterministic naming engine |
| Testing | Pytest · Vitest · Golden Tests |
| Data | Structured local datasets + RAG corpus |

## Current Status

Completed foundations include:

- Legacy Python → Next.js / TypeScript migration
- Golden-test protection of the original naming engine
- FastAPI Agent backend
- Next.js ↔ Python communication
- Birth-chart data contracts and tooling
- NamingConditionSet aggregation
- Provider provenance / validation handling
- Hybrid BM25 + Dense retrieval
- RRF fusion
- CrossEncoder reranking
- Retrieval benchmarking and traceability
- DeepSeek API configuration

The project is now moving into the **Agent reasoning and orchestration layer**, where structured traditional constraints, retrieved evidence, parental expectations, and multi-turn feedback will be combined to produce ranked and explainable name recommendations.

## Project Goal

The final system is not intended to be:

```text
Prompt → LLM → Random Chinese Names
```

Instead, it is designed as:

```text
Structured Facts
      +
Deterministic Naming Rules
      +
Retrieved Knowledge
      +
Human Preferences
      +
LLM Language Reasoning
      ↓
Explainable Chinese Naming Agent
```

The objective is to preserve the reliability of the original naming system while using modern LLM capabilities where they provide the most value: understanding people, interpreting nuanced preferences, reasoning across multiple constraints, and communicating why a name is a good fit.
```
