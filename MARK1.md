# MARK1.md - Advanced Features

This document defines **Mark1** features of Agents.NET - advanced capabilities built on top of Core.

**See also**: `CORE.md` for bare minimum agent system features.

## Overview

Mark1 extends the Core agent system with advanced management, coordination, and governance capabilities.

**Prerequisites**: All Core features must be implemented first.

## Mark1 Features

### 1. Task Management (Hierarchical)

**Advanced task orchestration** with relationships and context control.

**Relationships**:
- depends on
- is dependent of
- loop to
- loop from
- parallel with

**Context Assignment**:
- fresh context
- summarized context (field: summarization prompt - what and how to summarize)
- fork context

**Agent Assignment**:
- assigned agent

**Task Fields**:
- title
- description
- ACs (Acceptance Criteria)
- FR (Functional Requirements)
- NFR (Non-Functional Requirements)

**See**: `specs/mark1/task-management/SPEC.md`

### 2. Workflow Management (Hierarchical)

**Structured workflow execution** with governance and limits.

**CRUD**:
- Definition CRUD
- Execution CRUD

**Capabilities**:
- Workflow instance can start new workflow instance from running workflow
- Agent can start any workflow at any time

**Steps**:
- Sequential/parallel execution
- Step-to-task assignment (any level)

**Limits** (optional but recommended):
- Message limit
- Context size limit
- Time limit

**Overseeing** (optional):
- Agent to check for loops/deviation

**Step Behavior**:
- Automatic completion once agent is done
- Continued run (agent stops itself, even if nothing left to do - e.g., waiting on incoming message)

**See**: `specs/mark1/workflow-management/SPEC.md`

### 3. Specs Management (Hierarchical)

**Specification management** with hierarchy and cross-references.

**CRUD**:
- Definition CRUD
- Execution CRUD

**Types**:
- Technical specifications
- Business specifications

**Hierarchy**:
- Parent-child relationships
- Nested specifications
- Cross-references between specs

**See**: `specs/mark1/specs-management/SPEC.md`

### 4. Memory Management

**Hierarchical memory** with advanced search and summarization.

**Hierarchy**:
- All agent communication history (user/LLM) is hierarchical
- Fully accessible by every agent

**Operations**:
- Search
- Summarize (parameter: summarization prompt)

**Scopes**:
- parent
- child (including sub-agents)
- specific branches (e.g., "architecture branch")
- find branch by prompt (e.g., "where we talked about data initialization?")
- depth
- filters (e.g., "only user requests", "messages sent to user and user answers")

**Short-term Memory**:
- Current agent execution
- Starts fresh on new agent instance
- Still persisted
- Integrated into overall memory

**See**: `specs/mark1/memory-management/SPEC.md`

### 5. Tool/MCP Execution Governance

**Monitoring and governance** for tool and agent execution.

**Monitoring**:
- try/catch
- Overseeing
- Monitor for interactive input
- Monitor for "still executing" status
- Peek current output (tail with limit on last lines)

**Agent Invocation Governance**:
- Applies to agent invocation
- Example use case: "command too broad, takes too long, refactor needed" - use agent channels

**See**: `specs/mark1/tool-governance/SPEC.md`

### 6. Advanced Coordination

**Enhanced coordination** features beyond basic channels.

**Advanced Channel Features**:
- Multicast support (does not wait for user response)
- Message filtering and routing
- Priority queuing

**Advanced Agent Coordination**:
- Waiting for agent with deadline
- Agent result caching
- Batch agent spawning

**See**: `specs/mark1/coordination/SPEC.md`

### 7. Advanced Deployment

**Distributed and multi-project** deployment capabilities.

**Memory**:
- Local AND distributed
- Support multiple concurrent projects (may be unrelated)

**UI and Agents**:
- Same deployment model (local AND distributed)

**See**: `specs/mark1/deployment/SPEC.md`

## Extended Architecture

### Additional Projects (`src/`)

Mark1 adds the following projects to Core:

- `Agents.Core.Memory` - Memory infrastructure (Tasks/, Notes/, Decisions/, Graph/)
- `Agents.Core.TaskManagement` - Task orchestration
- `Agents.Core.WorkflowManagement` - Workflow execution
- `Agents.Core.SpecsManagement` - Specification management
- `Agents.Plugins` - Plugin infrastructure
- `Agents.Security` - Security infrastructure
- `Agents.Governance` - Tool/MCP governance

### Test Projects (`tests/`)

- Each source project has a corresponding `*.Tests` project using xUnit

## Specifications

Mark1 system specifications are maintained in `specs/mark1/`:

- `task-management/` - Task management specification
- `workflow-management/` - Workflow management specification
- `specs-management/` - Specification management specification
- `memory-management/` - Memory management specification
- `tool-governance/` - Tool/MCP governance specification
- `coordination/` - Advanced coordination specification
- `deployment/` - Deployment specification

**Inheritance**: Child specs inherit all parent spec properties and constraints.

See `specs/README.md` for complete specification hierarchy.

## Migration from Core

When implementing Mark1 features:

1. Ensure all Core features are stable and tested
2. Implement Mark1 features incrementally
3. Maintain backward compatibility with Core
4. Add Mark1 features as optional enhancements
5. Document migration path for users

## Development Principles

Same principles as Core:

- KISS (Keep It Simple, Stupid)
- SOLID principles
- DRY (Don't Repeat Yourself)
- Small steps
- HITL (Human In The Loop) with explicit approval required

**CRITICAL - MUST Follow During Development**:

When user prompt is unclear or there's a problem implementing as originally intended, you MUST:
1. Analyze why (briefly)
2. Ask user for clarification
3. Suggest possible solutions (briefly)

This ensures alignment and prevents wasted effort on misunderstood requirements.
