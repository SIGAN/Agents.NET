# Specifications

Hierarchical specification system for Agents.NET.

## Structure

- **Child specs inherit parent specs** - all properties and constraints flow down
- Each spec is defined in a `SPEC.md` file within its folder
- Organized by system domain

## Domains

- `system/` - Core system specifications
  - `task-management/` - Hierarchical task management
  - `workflow-management/` - Hierarchical workflow management
  - `specs-management/` - Hierarchical specifications management
  - `agent-management/` - Agent lifecycle and execution
  - `coordination/` - Inter-agent communication
  - `memory-management/` - Hierarchical memory and context
  - `tool-governance/` - Tool/MCP execution governance
  - `python-execution/` - Python snippet execution (CodeAct pattern)
  - `llm-gateway/` - Cross-model LLM abstraction layer
