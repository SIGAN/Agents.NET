# Core Specification

## Purpose

Bare minimum AI agent system based on codeact pattern. This specification defines the Core features that must be implemented first.

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

**Internal Tooling**:
- Python snippet execution (only internal tool needed)

## Core Concepts

### MicroAgent
- Specialized AI with basic context
- System prompt defining behavior
- Description (when to use/invoke)
- MUST use one or more Skills (agent = collection of skills)
- Spawns concurrently

**Note**: Advanced context management (fresh/summarized/fork) and hierarchical memory are Mark1 features.

### Skill
- Executable configuration (NOT just knowledge provider)
- Model (which LLM to use)
- Tools (allowed operations)
- Rules (behavior constraints)
- MCPs (Model Context Protocol integrations)
- Snippets (reusable code/scripts)

### HITL (Human In The Loop)
- Approval requests for critical operations
- Human interventions to stop/redirect agent execution
- Configurable approval policies
- Real-time human oversight

**Note**: Advanced HITL features (audit trails, multi-approval workflows) are Mark1 features.

## Cross-Platform

- .NET 10 runtime (Windows, Linux, macOS)
- Container support (Docker, Kubernetes)
- Cross-platform file system abstraction

## Cross-Model LLM Support

- Unified interface for multiple LLM providers (similar to LiteLLM)
- OpenAI, Anthropic, Azure OpenAI, and other providers
- Provider-agnostic agent definitions
- See `specs/core/llm-gateway/SPEC.md`

## Deployment

### Local Deployment
- Local execution environment
- Single project support

**Note**: Distributed deployment and multi-project support are Mark1 features.
