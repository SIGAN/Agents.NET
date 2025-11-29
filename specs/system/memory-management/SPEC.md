# Memory Management Specification

**Inherits**: `specs/system/SPEC.md`

## Overview

Hierarchical memory system with search, summarization, and scoped access.

## Hierarchy

- All agent communication history (user/LLM) is hierarchical
- Fully accessible by every agent

## Operations

- Search
- Summarize (parameter: summarization prompt)

## Scopes

- parent
- child (including sub-agents)
- specific branches (e.g., "architecture branch")
- find branch by prompt (e.g., "where we talked about data initialization?")
- depth
- filters (e.g., "only user requests", "messages sent to user and user answers")

## Short-term Memory

- Current agent execution
- Starts fresh on new agent instance
- Still persisted
- Integrated into overall memory
