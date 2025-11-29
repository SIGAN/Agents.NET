# Agents.NET

A general-purpose AI agent system built on .NET 10 using the codeact pattern with extensive metaprompting capabilities.

## Overview

Agents.NET provides a framework for building dynamic AI agents with full control over execution aspects. The system emphasizes reusability of existing tools (MCPs) and configuration-driven agent definitions rather than predefined agent types.

### Design Principles

- **Dynamic Control**: AI agents have full control over every aspect of execution
- **Reuse Over Reinvention**: Leverage existing MCPs (FileSystem, Fetch, shell-mcp-server, etc.)
- **Configuration-Driven**: Specialized agents (code agent, build agent) are configurations, not predefined types
- **CodeAct Pattern**: Required for all agents to handle real-world tasks requiring communication, execution, and calls
- **Extensive Metaprompting**: Dynamic prompt construction throughout the system
- **KISS, SOLID, DRY**: Simple, maintainable, aspect-based architecture

## Core Concepts

### Agent
- System prompt defining behavior
- Description (when to use/apply)
- One or more Skills

### Skill
- Rules governing behavior
- Tools for execution
- MCPs (Model Context Protocol) integrations
- Snippets (reusable code)

### Hierarchical Task Management
- Task relationships: depends on, is dependent of, loop to, loop from, parallel with
- Context modes: fresh, summarized, fork
- Fields: title, description, ACs, FR, NFR
- Agent assignment

### Hierarchical Workflow Management
- CRUD operations for definition and execution
- Workflows can spawn new workflow instances
- Agents can start workflows at any time
- Sequential/parallel steps
- Step-to-task assignment (any level)
- Optional limits: message, context size, time
- Optional overseeing for loops/deviation detection

### Agent Coordination
- CRUD channels for inter-agent communication
- Pre-created "user" channel for user interaction
- Multicast support
- Waiting for agents with deadlines

### Memory Management
- Hierarchical communication history (fully accessible by all agents)
- Search and summarize with scopes: parent, child, branches, depth, filters
- Short-term memory per agent instance
- Local AND distributed deployment support

### Tool/MCP Governance
- Try/catch error handling
- Monitoring: interactive input, execution status
- Output peeking (tail with line limits)
- Applies to all tool and agent invocations

## Architecture

Built using **aspect-based programming** with separate projects for each concern:

- **Agents.Core.Contracts** - Shared contracts and interfaces
- **Agents.Core.Memory** - Memory infrastructure with Tasks/, Notes/, Decisions/, Graph/ folders
- **Agents.Core.Coordination** - Channel and message coordination
- **Agents.Core.LlmGateway** - LLM abstraction (prompt caching, conversation management, API keys, compatibility)
- **Agents.Tools** - Standard tool implementations
- **Agents.Plugins** - Plugin infrastructure
- **Agents.MicroAgents** - Agent definitions
- **Agents.Security** - Security infrastructure

## Getting Started

### Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)

### Build

```bash
dotnet build
```

### Test

All projects use xUnit for testing. Each project has a corresponding `*.Tests` counterpart.

```bash
dotnet test
```

### Continuous Integration

GitHub Actions automatically builds and tests on every push to `main` and on pull requests.

## Project Structure

```
agents.net/
├── Agents.net.slnx          # Solution file
├── README.md                # This file
├── CLAUDE.md                # AI agent guidance
├── LICENSE
├── refsrc/                  # Reference sources (excluded from git)
├── specs/                   # Hierarchical specifications
│   └── system/              # Core system specs
├── src/                     # Source projects
│   ├── Agents.Core.Contracts/
│   ├── Agents.Core.Memory/
│   │   ├── Tasks/
│   │   ├── Notes/
│   │   ├── Decisions/
│   │   └── Graph/
│   ├── Agents.Core.Coordination/
│   ├── Agents.Core.LlmGateway/
│   ├── Agents.Tools/
│   ├── Agents.Plugins/
│   ├── Agents.MicroAgents/
│   └── Agents.Security/
└── tests/                   # Test projects (xUnit)
    ├── Agents.Core.Contracts.Tests/
    ├── Agents.Core.Memory.Tests/
    ├── Agents.Core.Coordination.Tests/
    ├── Agents.Core.LlmGateway.Tests/
    ├── Agents.Tools.Tests/
    ├── Agents.Plugins.Tests/
    ├── Agents.MicroAgents.Tests/
    └── Agents.Security.Tests/
```

## Deployment

The system supports both **local** and **distributed** deployment:
- Memory management works locally and distributed across multiple concurrent projects
- UI and agents follow the same deployment model

## Development

### Principles

- **KISS** (Keep It Simple, Stupid)
- **SOLID** principles
- **DRY** (Don't Repeat Yourself)
- Small steps with Human-In-The-Loop (HITL)
- Explicit approval required for major changes

### Internal Tooling

- Python snippet execution (only internal tool required)

## License

See [LICENSE](LICENSE) file for details.

## Contributing

This project follows a careful, step-by-step development approach with required HITL and explicit approval.
