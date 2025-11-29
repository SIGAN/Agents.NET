# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

General-purpose AI agent system based on codeact pattern.

## Design Intent

**Main Intent**: Give AI agents capabilities and ability to fully dynamically control every aspect of agent execution.

**Reuse, Don't Reinvent**:
- Reuse existing MCPs (FileSystem MCP for file I/O, Fetch for web content, shell-mcp-server for shell, etc.)
- All MCPs/tools defined by skill configuration
- Nothing predefined - GP agent system
- Specialized agents (code agent, build agent, etc.) are configurations, not predefined types

**CodeAct Pattern**:
- Required for all agents
- Most real tasks (even simple) require communication, execution, calls

**Metaprompting**:
- Use extensively throughout system
- Dynamic prompt construction for agents, skills, tasks

**CodeAct Pattern Implementation**:
- Python snippet execution (only internal tool required)
- See `specs/system/python-execution/SPEC.md`

## Core Concepts

### MicroAgent
- Specialized AI with configurable context (fresh/summarized/fork - assigned per task)
- System prompt defining behavior
- Description (when to use/invoke)
- MUST use one or more Skills (agent = collection of skills)
- Spawns concurrently, shares hierarchical memory with all agents

### Skill
- Executable configuration (NOT just knowledge provider)
- Model (which LLM to use)
- Tools (allowed operations)
- Rules (behavior constraints)
- MCPs (Model Context Protocol integrations)
- Snippets (reusable code/scripts)

## Task Management (Hierarchical)

### Relationships
- depends on
- is dependent of
- loop to
- loop from
- parallel with

### Context Assignment
- fresh context
- summarized context (field: summarization prompt - what and how to summarize)
- fork context

### Agent Assignment
- assigned agent

### Task Fields
- title
- description
- ACs (Acceptance Criteria)
- FR (Functional Requirements)
- NFR (Non-Functional Requirements)

## Workflow Management (Hierarchical)

### CRUD
- Definition CRUD
- Execution CRUD

### Capabilities
- Workflow instance can start new workflow instance from running workflow
- Agent can start any workflow at any time

### Steps
- Sequential/parallel execution
- Step-to-task assignment (any level)

### Limits (optional but recommended)
- Message limit
- Context size limit
- Time limit

### Overseeing (optional)
- Agent to check for loops/deviation

### Step Behavior
- Automatic completion once agent is done
- Continued run (agent stops itself, even if nothing left to do - e.g., waiting on incoming message)

## Specs Management (Hierarchical)

### CRUD
- Definition CRUD
- Execution CRUD

### Types
- Technical specifications
- Business specifications

### Hierarchy
- Parent-child relationships
- Nested specifications
- Cross-references between specs

## Agent Management

### CRUD
- Definition CRUD
- Execution CRUD

### Execution
- All agents spawn as concurrent

## Coordination

### Channels
- CRUD channels
- CRUD messages
- Pre-created well-known channel: "user"
- Multicast support (does not wait for user response)

### Agent Coordination
- Exchange messages between agents
- Waiting for agent with deadline

## Memory Management

### Hierarchy
- All agent communication history (user/LLM) is hierarchical
- Fully accessible by every agent

### Operations
- Search
- Summarize (parameter: summarization prompt)

### Scopes
- parent
- child (including sub-agents)
- specific branches (e.g., "architecture branch")
- find branch by prompt (e.g., "where we talked about data initialization?")
- depth
- filters (e.g., "only user requests", "messages sent to user and user answers")

### Short-term Memory
- Current agent execution
- Starts fresh on new agent instance
- Still persisted
- Integrated into overall memory

## Tool/MCP Execution Governance

### Monitoring
- try/catch
- Overseeing
- Monitor for interactive input
- Monitor for "still executing" status
- Peek current output (tail with limit on last lines)

### Agent Invocation Governance
- Applies to agent invocation
- Example use case: "command too broad, takes too long, refactor needed" - use agent channels

## Deployment

### Memory
- Local AND distributed
- Support multiple concurrent projects (may be unrelated)

### UI and Agents
- Same deployment model (local AND distributed)

## Architecture

### Aspect-Based Programming

**Source Projects** (`src/`):
- `Agents.Core.Contracts` - Shared contracts
- `Agents.Core.Memory` - Memory infrastructure (Tasks/, Notes/, Decisions/, Graph/)
- `Agents.Core.Coordination` - Channel/message coordination
- `Agents.Core.LlmGateway` - Abstracts prompt caching, conversation mgmt, API keys, compatibility
- `Agents.Tools` - Standard tools
- `Agents.Plugins` - Plugin infrastructure
- `Agents.MicroAgents` - Agent definitions
- `Agents.Security` - Security infrastructure

**Test Projects** (`tests/`):
- Each source project has a corresponding `*.Tests` project using xUnit

**Other Folders:**
- `refsrc/` - Cloned projects/files for reference (contents excluded from git)
- `specs/` - Hierarchical specifications with inheritance

## Specifications

All system specifications are maintained in the `specs/` folder with hierarchical structure.

**Inheritance**: Child specs inherit all parent spec properties and constraints.

See `specs/README.md` for complete specification hierarchy.

## Development Principles

- KISS (Keep It Simple, Stupid)
- SOLID principles
- DRY (Don't Repeat Yourself)
- Small steps
- HITL (Human In The Loop) with explicit approval required

## Testing

**Framework**: xUnit for .NET 10

**Structure**: Every project has a corresponding `*.Tests` project in the `tests/` folder.

## Build Commands

```bash
dotnet build
dotnet test
```

## Continuous Integration

GitHub Actions workflow runs on every push to `main` and on pull requests:
- Build solution (Release configuration)
- Run all tests

See `.github/workflows/ci.yml` for configuration.
