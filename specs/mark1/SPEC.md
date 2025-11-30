# Mark1 Specification

**Inherits**: `specs/core/SPEC.md`

## Purpose

Advanced features built on top of Core. This specification defines the Mark1 features that extend the bare minimum agent system with hierarchical management, governance, and advanced coordination.

## Advanced Features

### Context Management

- fresh context
- summarized context (field: summarization prompt - what and how to summarize)
- fork context

### Hierarchical Memory

- All agent communication history (user/LLM) is hierarchical
- Fully accessible by every agent
- Search and summarization capabilities
- Scoped access (parent, child, branches, depth, filters)

### Advanced Deployment

- Distributed deployment
- Support multiple concurrent projects (may be unrelated)
- UI and Agents with same deployment model (local AND distributed)

## Mark1 Subsystems

The following subsystems extend Core functionality:

- **Task Management** - See `specs/mark1/task-management/SPEC.md`
- **Workflow Management** - See `specs/mark1/workflow-management/SPEC.md`
- **Specs Management** - See `specs/mark1/specs-management/SPEC.md`
- **Memory Management** - See `specs/mark1/memory-management/SPEC.md`
- **Tool Governance** - See `specs/mark1/tool-governance/SPEC.md`

## Development Approach

1. Implement all Core features first
2. Ensure Core is stable and tested
3. Implement Mark1 features incrementally
4. Maintain backward compatibility with Core
5. Mark1 features should be optional enhancements
