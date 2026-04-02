---
name: database-architecture
description: This skill should be used when designing schemas, relationships, indexing, migrations, query patterns, and data-access strategies.
compatibility: opencode
metadata:
  replaces-agent: database-architect
---

# Database Architecture

Apply this skill when data modeling or query design is central to the task.

## Focus areas

- Schema design and normalization trade-offs
- Primary keys, foreign keys, and relationship modeling
- Indexing and query performance
- Migrations and backward-compatible data changes
- Transaction boundaries and consistency guarantees

## Workflow

1. Start from access patterns, not tables alone.
2. Model relationships explicitly and avoid accidental N+1 paths.
3. Add indexes only for real read paths and constraints.
4. Treat destructive migrations as high-risk.
5. Call out data backfill or rollout requirements.

## Deliverables

- Schema or migration recommendations
- Query and indexing guidance
- Rollout and data safety considerations
- Suggested database tests or benchmarks
