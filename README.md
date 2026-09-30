# Chinese Name Generator — AI Naming Agent

An AI-assisted Chinese naming system evolving from a legacy rule-based application into a structured, explainable Agent architecture.

Next.js · React · TypeScript · Python · FastAPI · Hybrid RAG · CrossEncoder · DeepSeek

## Overview

This project modernizes a legacy Chinese naming system into an AI-powered naming Agent while preserving its validated traditional naming rules.

The original Python/Streamlit prototype was first migrated to a maintainable Next.js + TypeScript application. The current phase extends the system with an independent Python Agent backend that combines deterministic naming rules, birth-chart analysis, retrieval-augmented generation (RAG), and LLM reasoning.

Instead of asking an LLM to generate names directly, the system separates **facts, constraints, retrieval, reasoning, and explanation** into different layers. Deterministic components handle rules that should remain stable, while the LLM is used where language understanding and flexible reasoning provide the most value.

The long-term goal is an interactive naming Agent that can understand traditional naming constraints, open-ended parental expectations, and multi-turn feedback while producing explainable name recommendations.

## System Evolution

    Legacy Python / Streamlit Prototype
                  ↓
        Next.js + TypeScript
                  ↓
         Golden Test Validation
                  ↓
       Python FastAPI Backend
                  ↓
        Birth Chart Analysis
                  ↓
       NamingConditionSet
                  ↓
          Hybrid RAG
                  ↓
        Agent Reasoning
                  ↓
     Explainable Recommendations

## Architecture

    User
     │
     ▼
    Next.js / React Frontend
     │
     ▼
    FastAPI Agent Backend
     │
     ├───────────────┐
     ▼               ▼
    Birth Chart    User Preferences
    Tools
     │               │
     └───────┬───────┘
             ▼
      NamingConditionSet
             │
       ┌─────┴─────┐
       ▼           ▼
    Naming       Hybrid RAG
    Engine       BM25 + Dense
                   │
                   ▼
                RRF Fusion
                   │
                   ▼
           CrossEncoder Rerank
       │           │
       └─────┬─────┘
             ▼
        Agent Reasoning
             │
          DeepSeek
             │
             ▼
      Candidate Ranking
             │
             ▼
    Explainable Recommendations

## What I Built

- Migrated the legacy Python/Streamlit naming application to Next.js + TypeScript.
- Preserved original naming behavior through **126 Golden Test cases** generated from the legacy implementation.
- Built an independent **Python FastAPI Agent backend** and connected it with the existing Next.js application.
- Added structured request/response schemas, configuration management, logging, and unified error handling.
- Designed a structured **BirthChart → NamingConditionSet** pipeline for converting birth information into downstream naming constraints.
- Separated deterministic traditional rules from LLM reasoning instead of allowing the model to rewrite validated business logic.
- Added multi-provider birth-chart provenance and validation structures so provider disagreements can be recorded rather than silently resolved by an LLM.
- Built a **Hybrid RAG pipeline** combining BM25 lexical retrieval and dense semantic retrieval.
- Added **Reciprocal Rank Fusion (RRF)** and **CrossEncoder reranking** for higher-quality evidence retrieval.
- Preserved retrieval provenance including BM25, dense, RRF, and reranking scores/ranks.
- Integrated **DeepSeek** as the LLM layer while keeping model access isolated from deterministic naming components.
- Designed the next Agent layer around multi-constraint reasoning, parental expectations, and iterative user feedback.

## Agent Design

The system follows one central principle:

> **Use deterministic code for facts and hard constraints; use the LLM for language understanding, preference interpretation, reasoning, and explanation.**

The LLM does not independently calculate or overwrite birth-chart facts, stroke counts, or validated naming rules.

Instead, information flows through structured layers:

    Birth Information
          ↓
      BirthChart
          ↓
    NamingConditionSet
          ↓
    Hard Constraints
      +
    Soft Preferences
          ↓
      RAG Evidence
          ↓
     LLM Reasoning
          ↓
    Candidate Ranking
          ↓
      Explanation

This architecture makes the system more controllable, testable, and explainable than direct LLM name generation.

## Hybrid RAG

The retrieval layer uses a two-stage retrieval and reranking pipeline:

    Agent Query
        │
        ├── BM25 Retrieval ─── Top 50
        │
        └── Dense Retrieval ── Top 50
                    │
                    ▼
                RRF Fusion
                  k = 60
                    │
                  Top 30
                    │
                    ▼
          CrossEncoder Reranker
                    │
                  Top 5
                    │
                    ▼
            Final RAG Evidence

Current reranker:

`BAAI/bge-reranker-v2-m3`

The retrieval trace preserves:

- source / chunk provenance
- BM25 score and rank
- dense similarity score and rank
- RRF score and rank
- CrossEncoder score and final rank

This provides an auditable path from the original query to the evidence ultimately supplied to the Agent.

## Naming Conditions

Instead of passing loosely structured text between components, the system uses a structured `NamingConditionSet`.

Conceptually:

    NamingConditionSet
    ├── Birth-chart-derived conditions
    ├── Five-element preferences / constraints
    ├── Stroke and naming-rule constraints
    ├── Family / generational character requirements
    ├── Character avoidance rules
    ├── Parent preferences
    ├── Desired meaning and expectations
    └── Provenance / validation metadata

Different conditions are not automatically treated as equally important.

For example, a required generational character may conflict with an ideal five-element combination. The Agent should preserve the hard family requirement while optimizing the remaining character and other naming dimensions around it.

## Multi-Turn Agent

The target Agent is designed for iterative naming rather than one-shot generation.

    Initial Conditions
          ↓
    Generate / Rank Candidates
          ↓
       User Feedback
          ↓
    Preference Extraction
          ↓
    Update Conditions / Weights
          ↓
    Re-rank / Generate Again
          ↓
         Repeat

Later turns therefore do not simply rerun the initial request with identical weights.

The LLM can interpret open-ended preferences such as:

- academic or intellectual expectations
- career and achievement aspirations
- prosperity or success-related meanings
- personality and temperament
- literary or classical style
- modern vs. traditional style
- preferred or disliked characters
- pronunciation and rhythm preferences
- family-specific requirements
- other free-form parental expectations

These preferences can then be converted into structured signals for downstream candidate generation and ranking.

## Explainability

The final system is designed to explain why each recommended name fits the user's conditions.

The explanation layer can combine:

    Birth-Chart Conditions
              +
    Traditional Naming Rules
              +
    Five-Element Contribution
              +
    Family Requirements
              +
    Semantic Meaning
              +
    Parent Expectations
              +
    Retrieved Evidence
              ↓
      Final Name Explanation

Intermediate traces can also record how individual candidates satisfy, conflict with, or compensate for different naming conditions.

## Engineering Principles

- **Deterministic core first** — validated naming rules remain outside the LLM.
- **LLM where language matters** — preference understanding, semantic reasoning, interaction, and explanation.
- **Structured contracts** — components communicate through explicit schemas instead of uncontrolled prompts.
- **Traceability** — retrieval and reasoning inputs preserve provenance.
- **Multi-turn adaptation** — later user feedback can change subsequent ranking behavior.
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
| Naming Logic | Legacy deterministic naming engine |
| Testing | Pytest · Vitest · Golden Tests |
| Data | Structured local datasets + RAG corpus |

## Current Status

Completed foundations include:

- Legacy Python → Next.js / TypeScript migration
- Golden-test protection of the original naming engine
- FastAPI Agent backend
- Next.js ↔ Python communication
- Birth-chart data contracts and tooling
- `NamingConditionSet` aggregation
- Provider provenance and validation handling
- Hybrid BM25 + Dense retrieval
- RRF fusion
- CrossEncoder reranking
- Retrieval benchmarking and traceability
- DeepSeek API configuration

The project is now moving into the **Agent reasoning and orchestration layer**, where structured traditional constraints, retrieved evidence, parental expectations, and multi-turn feedback will be combined to produce ranked and explainable name recommendations.

## Project Goal

The target architecture is not:

    Prompt → LLM → Random Chinese Names

Instead:

    Structured Facts
          +
    Deterministic Rules
          +
    Retrieved Knowledge
          +
    Human Preferences
          +
    LLM Language Reasoning
          ↓
    Explainable Chinese Naming Agent

The goal is to preserve the reliability of the original naming system while using modern LLM capabilities where they provide the most value: understanding users, interpreting nuanced preferences, reasoning across multiple constraints, and explaining why a name fits.
