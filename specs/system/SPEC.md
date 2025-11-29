# System Specification

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

**Internal Tooling**:
- Python snippet execution (only internal tool needed)

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

## Deployment

### Memory
- Local AND distributed
- Support multiple concurrent projects (may be unrelated)

### UI and Agents
- Same deployment model (local AND distributed)
