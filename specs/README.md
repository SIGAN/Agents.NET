# Specifications

Hierarchical specification system for Agents.NET.

## Structure

- **Child specs inherit parent specs** - all properties and constraints flow down
- Each spec is defined in a `SPEC.md` file within its folder
- Organized by feature tier (Core and Mark1)

## Feature Tiers

### Core Features (`core/`)

**Bare minimum agent system** - implement first:

- `llm-gateway/` - Cross-model LLM abstraction layer (foundation for agents)
- `agent-management/` - Agent lifecycle and execution, Skills
- `coordination/` - Inter-agent communication (basic channels)
- `python-execution/` - Python snippet execution (CodeAct pattern)

### Mark1 Features (`mark1/`)

**Advanced features** - implement after Core is stable:

- `task-management/` - Hierarchical task management
- `workflow-management/` - Hierarchical workflow management
- `specs-management/` - Hierarchical specifications management
- `memory-management/` - Hierarchical memory and context
- `tool-governance/` - Tool/MCP execution governance

## Development Priority

1. Implement all **Core** features first
2. Ensure Core is stable and tested
3. Implement **Mark1** features incrementally
4. Maintain backward compatibility with Core
