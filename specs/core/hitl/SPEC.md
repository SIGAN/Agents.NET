# HITL (Human In The Loop) Specification

**Inherits**: `specs/core/SPEC.md`

## Overview

Human oversight and control mechanism for agent operations, enabling approvals and interventions.

## Purpose

- Provide human control over critical agent operations
- Enable real-time human intervention in agent execution
- Support configurable approval policies
- Ensure safe and controlled agent behavior

## Core Capabilities

### 1. Approval Requests

Agents can request human approval before executing specific operations.

**Operations**:
- `RequestApproval(operation, context, timeout)` - Agent requests approval
  - `operation`: Description of what agent wants to do
  - `context`: Additional context for human decision
  - `timeout`: Optional timeout for approval (default: wait indefinitely)
  - Returns: `Approved | Denied | TimedOut`

- `GrantApproval(requestId, response)` - Human grants approval
  - `requestId`: ID of the approval request
  - `response`: Optional message back to agent

- `DenyApproval(requestId, reason)` - Human denies approval
  - `requestId`: ID of the approval request
  - `reason`: Optional explanation

**Use Cases**:
- File system modifications (delete, write)
- External API calls
- Executing shell commands
- Spawning new agents
- Resource-intensive operations

### 2. Human Interventions

Humans can interrupt and redirect agent execution at any time.

**Operations**:
- `Intervene(agentId, action, instruction)` - Human interrupts agent
  - `agentId`: ID of the agent to intervene on
  - `action`: `Stop | Pause | Redirect`
  - `instruction`: Optional new instruction or direction

- `Resume(agentId)` - Resume paused agent

**Actions**:
- **Stop**: Terminate agent execution immediately
- **Pause**: Pause agent, allow human to review state
- **Redirect**: Change agent's current task/goal

### 3. Approval Policies

Configurable policies defining what requires approval.

**Operations**:
- `SetApprovalPolicy(policy)` - Configure approval requirements
  - `policy`: Policy definition (JSON/configuration object)

- `GetApprovalPolicy()` - Retrieve current policy

**Policy Types**:
- **Always**: Always require approval for specific operations
- **Never**: Never require approval (auto-approve)
- **Conditional**: Require approval based on conditions
  - Operation type
  - Resource impact
  - Risk level
  - Agent identity

**Default Policy** (Core):
- File deletions: Require approval
- Shell command execution: Require approval
- External API calls: No approval required
- File reads: No approval required
- Agent spawning: No approval required

### 4. Approval Workflow

**Standard Flow**:
1. Agent prepares to execute operation
2. Agent checks approval policy
3. If approval required:
   - Agent sends approval request
   - Agent pauses execution
   - Human reviews request
   - Human approves or denies
4. Agent receives response
5. Agent proceeds (if approved) or handles denial

**Timeout Handling**:
- If timeout specified and exceeded: Agent receives `TimedOut`
- Agent must handle timeout (retry, abort, or proceed with fallback)

**Denial Handling**:
- Agent receives reason for denial
- Agent must handle denial gracefully
- Agent can request alternative approach or abort

## Integration Points

### Agent Execution

Agents must support:
- Pausing execution while waiting for approval
- Resuming execution after approval granted
- Handling approval denial
- Handling approval timeout

### Channels

HITL uses the pre-created "user" channel for:
- Sending approval requests to human
- Receiving approval responses from human
- Sending intervention commands to agents

### Message Format

**Approval Request**:
```json
{
  "type": "ApprovalRequest",
  "requestId": "uuid",
  "agentId": "uuid",
  "operation": "Delete file: /path/to/file",
  "context": "Cleaning up temporary files",
  "timestamp": "ISO-8601"
}
```

**Approval Response**:
```json
{
  "type": "ApprovalResponse",
  "requestId": "uuid",
  "approved": true,
  "response": "Approved - proceed with cleanup",
  "timestamp": "ISO-8601"
}
```

**Intervention**:
```json
{
  "type": "Intervention",
  "agentId": "uuid",
  "action": "Redirect",
  "instruction": "Focus on unit tests instead",
  "timestamp": "ISO-8601"
}
```

## Implementation Requirements

### Core.HITL Project

**Components**:
- `IApprovalService` - Approval request/response handling
- `IInterventionService` - Intervention handling
- `IApprovalPolicy` - Policy definition and evaluation
- `ApprovalRequest` - Request model
- `ApprovalResponse` - Response model
- `InterventionCommand` - Intervention model

**Behavior**:
- Non-blocking approval requests (async)
- Thread-safe approval tracking
- Support for multiple concurrent approval requests
- Intervention can interrupt any agent at any time

### Agent Integration

Agents must:
- Check approval policy before executing operations
- Send approval requests when required
- Wait for approval response
- Handle approval/denial/timeout
- Listen for intervention commands
- Support pause/resume/stop

## What's NOT in Core

Advanced HITL features deferred to Mark1:
- Audit trails (log all approvals/denials/interventions)
- Multi-level approvals (require multiple humans)
- Approval delegation (assign approvals to specific people)
- Conditional auto-approval (learn from past approvals)
- Approval analytics (track approval patterns)
- Role-based approval policies
