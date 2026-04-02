---
name: python-development
description: This skill should be used when implementing or reviewing Python services, automation, async behavior, data processing, type hints, and idiomatic Python structure.
compatibility: opencode
metadata:
  replaces-agent: python-pro
---

# Python Development

Apply this skill when Python design and implementation quality matter.

## Focus areas

- Idiomatic Python structure and naming
- Type hints and interface clarity
- Async, concurrency, and I/O handling
- Data processing and memory efficiency
- Testability and packaging discipline

## Workflow

1. Prefer readable functions and explicit types.
2. Use small composable units instead of deep inheritance.
3. Guard I/O boundaries and error paths carefully.
4. Optimize only after identifying the hot path.
5. Add tests around edge cases and data transformations.

## Deliverables

- Python implementation guidance
- Type and runtime recommendations
- Reliability and performance notes
- Suggested pytest coverage areas
