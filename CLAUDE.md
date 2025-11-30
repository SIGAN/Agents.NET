# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

General-purpose AI agent system based on codeact pattern.

**Key Requirements**:
- **Cross-platform**: Windows, Linux, macOS, etc.
- **Cross-model**: Multiple LLM providers (similar to LiteLLM)

## Feature Organization

The system is organized into two feature tiers:

- **CORE.md** - Bare minimum agent system (implement first)
  - LLM Gateway
  - MicroAgent (basic definition and execution)
  - Skill
  - Agent Channels (basic coordination)
  - HITL (Human In The Loop - approvals and interventions)
  - Spawn agent
  - WaitAndGetResult
  - Python execution (CodeAct pattern)

- **MARK1.md** - Advanced features (implement after Core is stable)
  - Task Management (Hierarchical)
  - Workflow Management (Hierarchical)
  - Specs Management (Hierarchical)
  - Memory Management (hierarchical, search, summarize)
  - Tool/MCP Execution Governance
  - Advanced Coordination
  - Advanced Deployment

**Development Priority**: Implement Core features first, then Mark1 features incrementally.

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
- See `specs/core/python-execution/SPEC.md`

## Feature Details

See detailed feature specifications in:
- **CORE.md** - Core features (implement first)
- **MARK1.md** - Advanced features (implement after Core)

## Architecture

### Aspect-Based Programming

**Core Projects** (`src/`) - Implement first:
- `Agents.Core.Contracts` - Shared contracts
- `Agents.Core.LlmGateway` - LLM abstraction layer (cross-model support, API keys, basic conversation mgmt)
- `Agents.Core.Coordination` - Channel/message coordination
- `Agents.Core.HITL` - Human In The Loop (approvals and interventions)
- `Agents.MicroAgents` - Agent definitions and execution
- `Agents.Tools` - Python execution tool

**Mark1 Projects** (`src/`) - Implement after Core:
- `Agents.Core.Memory` - Memory infrastructure (Tasks/, Notes/, Decisions/, Graph/)
- `Agents.Core.TaskManagement` - Task orchestration
- `Agents.Core.WorkflowManagement` - Workflow execution
- `Agents.Core.SpecsManagement` - Specification management
- `Agents.Plugins` - Plugin infrastructure
- `Agents.Security` - Security infrastructure
- `Agents.Governance` - Tool/MCP governance

**Test Projects** (`tests/`):
- Each source project has a corresponding `*.Tests` project using xUnit

**Other Folders:**
- `refsrc/` - Cloned projects/files for reference (contents excluded from git)
- `specs/` - Hierarchical specifications with inheritance
  - `specs/core/` - Core feature specifications
  - `specs/mark1/` - Mark1 feature specifications

## Specifications

All system specifications are maintained in the `specs/` folder with hierarchical structure.

**Organization**:
- `specs/core/` - Core feature specifications (implement first)
- `specs/mark1/` - Mark1 feature specifications (implement after Core)

**Inheritance**: Child specs inherit all parent spec properties and constraints.

See `specs/README.md` for complete specification hierarchy.

## Development Principles

- KISS (Keep It Simple, Stupid)
- SOLID principles
- DRY (Don't Repeat Yourself)
- Small steps
- HITL (Human In The Loop) with explicit approval required

## Coding Standards

**Async Methods** (MANDATORY):
- **All methods MUST be async** - No synchronous methods allowed
- **All async methods MUST include CancellationToken parameter** with default value
- Parameter format: `CancellationToken ct = default`
- Example:
  ```csharp
  public async Task<Result> DoSomethingAsync(string input, CancellationToken ct = default)
  {
      // implementation
  }
  ```

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
