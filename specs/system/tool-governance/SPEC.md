# Tool/MCP Execution Governance Specification

**Inherits**: `specs/system/SPEC.md`

## Overview

Monitoring and governance for tool/MCP execution and agent invocation.

## Monitoring

- try/catch
- Overseeing
- Monitor for interactive input
- Monitor for "still executing" status
- Peek current output (tail with limit on last lines)

## Agent Invocation Governance

- Applies to agent invocation
- Example use case: "command too broad, takes too long, refactor needed" - use agent channels
