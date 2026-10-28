# Chinese Name Generation Agent

**AI-assisted legacy modernization with deterministic rules, local retrieval, and bounded multi-agent exploration.**

[English](README.md) · [简体中文](README.zh-CN.md)

## Project overview

An internal tool helps professional naming specialists turn family preferences into personalized Chinese-name recommendations. The legacy engine enforces naming constraints but cannot interpret open-ended wishes or assess whether a complete name sounds natural. V2 preserves that deterministic boundary while adding retrieval, specialist exploration routes, and independent LLM review; a human retains final judgment.

## Engineering highlights

- **Multi-strategy retrieval:** BM25 and local BGE character-vector composition search the complete eligible name space. Available pair hints and verified text evidence add signals; Reciprocal Rank Fusion (RRF) combines ranks before bounded LLM selection.
- **Bounded multi-agent execution:** Four routes share immutable family context and pool identity, with thread-based concurrency, locked budget reservations, stable merge order, and isolated failures.
- **Separate eligibility, quality, and evidence:** P2 rechecks candidate IDs; source-blind P4 assesses complete names. Code separates hard rejection, ranking penalties, and restrictions on explanatory claims.
- **Cost-aware selection:** Stage budgets, versioned caches, and at most one targeted supplement bound exploration. Final selection balances quality and diversity without fixed route quotas.
- **Recoverable delivery:** Persisted events drive SSE progress. Recommendations are checkpointed before explanations, allowing explanation failures to preserve usable results with an explicit partial status.

## V2 system architecture

![V2 system architecture](<assets/High-Level Architecture.png>)

The deterministic engine establishes eligibility; applicable exploration routes use local retrieval before independent whole-name review and portfolio selection. Recommendations are saved before explanation generation. Retrieval scores guide discovery, never substitute for whole-name quality.

| Route | Responsibility and evidence boundary |
| --- | --- |
| Poetry | Local corpus search; distinguish exact contiguous text evidence from broader imagery. A match alone does not establish a suitable naming origin. |
| Regional | Explore supplied birthplace, hometown, or family background without conflating them. Unverified associations remain labeled imagery. |
| Time | Explore natural time imagery from supplied date/time; specific events require evidence. |
| Creative | Explore coherent, modern complete names within the eligible pool; creative interpretations do not become historical facts. |

Independent professional-review modules exist, but **the current Preview skips paid per-candidate professional review and reserves its quality budget for unified P4**; that optional stage is omitted above. P4 excludes retrieval scores and route narratives. The portfolio allocator selects up to 30 names, returning fewer when quality or budget is insufficient. P5 supplies brief explanations and on-demand deeper readings, distinguishing validated factual evidence from creative imagery.

## Architecture evolution: V1 → V2

| Concern | V1 | V2 |
| --- | --- | --- |
| Discovery | One service coordinates semantic, cultural, browsing, and creative tools | Four independently executable routes with shared context and explicit eligibility |
| Retrieval | BM25 and composed semantic lists are interleaved | Full-pool multi-strategy ranking, RRF, bounded candidate windows |
| Selection | Independent whole-name review and diversity filtering | Unified source-blind review, separate claim restrictions, dynamic portfolio allocation |
| Recovery | Workflow budgets, caches, session persistence | Route isolation, stage checkpoints, persisted progress, recommendation preservation through P5 failures |
| Migration | Established default workflow | Separate V2 Preview and storage; V1 remains the default entry |

An earlier proposal to precompute every name's LLM interpretation gave way to reusable character-level assets and bounded whole-name evaluation. Composed character semantics remain retrieval clues, not verified whole-name definitions.

## Technology stack

| Layer | Private implementation |
| --- | --- |
| Interface | Next.js, React, TypeScript, Tailwind CSS |
| Backend | Python, FastAPI, Pydantic; standard-library thread pools and locks |
| Retrieval / LLM | NumPy, local BM25, Sentence Transformers/BGE, RRF; DeepSeek through a budgeted structured-output adapter |
| Storage / verification | Atomic versioned JSON, local caches; pytest, Vitest, Playwright, TypeScript checks; Docker Compose and GitHub Actions configuration |

## Evaluation & reliability

**Status — October 28, 2026:** V2 contracts, four routes, orchestration, unified P4/portfolio selection, and Preview/P5 are implemented. Historical D-1/D-2 offline reports record passing Python regressions, web tests, type checks, and builds. Those results describe earlier snapshots; subsequent retrieval, concurrency, and resilience changes still need consolidated final acceptance.

Golden-case regressions protect legacy rule behavior. Current test sources also cover admission, bounded concurrency, budget reservation, cache scoping, partial failures, and checkpoint/result recovery. Journals block automatic replay of paid requests with unknown outcomes; session recovery does not guarantee arbitrary interrupted tasks can safely resume.

**Real naming quality remains pending.** The same-budget comparison of V1, an enhanced single-agent baseline, and V2 has not completed final acceptance. No measured productivity gain, quality superiority, or production throughput is claimed.

Route parallelism operates within one naming job. Preview accepts one active job at a time; multi-user isolation, shared database/cache infrastructure, durable queue workers, and horizontal scaling remain planned.

## Repository scope

This is a **public engineering case study**, not a runnable distribution. Company source code, internal prompts, credentials, customer data, sensitive configuration, and private API contracts are excluded. Tests, services, deployment files, and CI described here belong to the private implementation.
