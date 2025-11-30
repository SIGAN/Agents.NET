# CORE.md - Bare Minimum Agent System

This document defines the **Core** features of Agents.NET - the bare minimum required for a functional agent system.

**See also**: `MARK1.md` for advanced features planned for Mark1 release.

## Purpose

Minimal viable AI agent system based on CodeAct pattern.

**Key Requirements**:
- **Cross-platform**: Windows, Linux, macOS, etc.
- **Cross-model**: Multiple LLM providers (similar to LiteLLM)

## Design Philosophy

**Keep It Minimal**:
- Only essential features for agent operation
- No hierarchical structures (tasks, workflows, specs)
- No advanced memory or governance
- Simple coordination via channels

**Reuse, Don't Reinvent**:
- Reuse existing MCPs (FileSystem MCP for file I/O, Fetch for web content, shell-mcp-server for shell, etc.)
- All MCPs/tools defined by skill configuration
- Nothing predefined - general-purpose agent system
- Specialized agents (code agent, build agent, etc.) are configurations, not predefined types

**CodeAct Pattern**:
- Required for all agents
- Most real tasks (even simple) require communication, execution, calls
- Python snippet execution (only internal tool required)

**Metaprompting**:
- Dynamic prompt construction for agents and skills

## Core Features

### 1. LLM Gateway

**Foundation** that all agents are built on.

- Unified interface for multiple LLM providers (OpenAI, Anthropic, Azure OpenAI, etc.)
- Provider-agnostic API
- Request/response translation
- API key management
- Basic conversation management

**See**: `specs/core/llm-gateway/SPEC.md`

### 2. MicroAgent

**Core Concept**: Specialized AI with basic capabilities.

**Properties**:
- System prompt defining behavior
- Description (when to use/invoke)
- MUST use one or more Skills (agent = collection of skills)
- Spawns concurrently

**Operations**:
- Create agent definition
- Spawn agent (concurrent execution)
- WaitAndGetResult (wait for agent completion and retrieve result)

**See**: `specs/core/agent-management/SPEC.md`

### 3. Skill

**Executable configuration** that defines agent capabilities.

**Properties**:
- Model (which LLM to use)
- Tools (allowed operations)
- Rules (behavior constraints)
- MCPs (Model Context Protocol integrations)
- Snippets (reusable code/scripts)

**See**: `specs/core/agent-management/SPEC.md`

### 4. Agent Channels

**Simple communication** between agents and with user.

**Features**:
- Create/delete channels
- Send messages
- Receive messages
- Pre-created well-known channel: "user"
- Basic message exchange between agents

**Operations**:
- CreateChannel
- DeleteChannel
- SendMessage
- ReceiveMessage
- WaitForMessage

**See**: `specs/core/coordination/SPEC.md`

### 5. Python Execution (CodeAct)

**Only internal tool** required for agents.

**Features**:
- Execute Python snippets
- Return results
- Basic error handling

**See**: `specs/core/python-execution/SPEC.md`

### 6. HITL (Human In The Loop)

**Human oversight and control** for agent operations.

**Features**:
- Approval requests (agent asks for permission before executing)
- Interventions (human stops or redirects agent execution)
- Approval policies (define what requires approval)
- Intervention triggers (define when human can intervene)

**Operations**:
- RequestApproval (agent requests human approval)
- GrantApproval / DenyApproval (human responds to approval request)
- Intervene (human interrupts agent execution)
- SetApprovalPolicy (configure what requires approval)

**See**: `specs/core/hitl/SPEC.md`

## What's NOT in Core

The following features are deferred to **Mark1**:

- Task Management (hierarchical)
- Workflow Management (hierarchical)
- Specs Management (hierarchical)
- Memory Management (hierarchical, search, summarize, scopes)
- Tool/MCP Execution Governance (monitoring, overseeing)
- Advanced coordination (multicast, deadlines)
- Deployment (distributed, multiple projects)
- Context assignment (fresh/summarized/fork)

## Architecture

### Core Projects (`src/`)

- `Agents.Core.Contracts` - Shared contracts
- `Agents.Core.LlmGateway` - LLM abstraction layer (cross-model support, API keys, basic conversation mgmt)
- `Agents.Core.Coordination` - Channel/message coordination
- `Agents.Core.HITL` - Human In The Loop (approvals and interventions)
- `Agents.MicroAgents` - Agent definitions and execution
- `Agents.Tools` - Python execution tool

### Test Projects (`tests/`)

- Each source project has a corresponding `*.Tests` project using xUnit

### Other Folders

- `refsrc/` - Cloned projects/files for reference (contents excluded from git)
- `specs/` - Hierarchical specifications with inheritance

## Specifications

Core system specifications are maintained in `specs/core/`:

- `llm-gateway/` - LLM Gateway specification
- `agent-management/` - Agent and Skill specifications
- `coordination/` - Channel communication specification
- `python-execution/` - Python execution specification
- `hitl/` - Human In The Loop specification

**Inheritance**: Child specs inherit all parent spec properties and constraints.

See `specs/README.md` for complete specification hierarchy.

## Development Principles

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
