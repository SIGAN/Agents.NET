# Workflow Management Specification

**Inherits**: `specs/system/SPEC.md`

## Overview

Hierarchical workflow management with CRUD operations for definition and execution.

## CRUD

- Definition CRUD
- Execution CRUD

## Capabilities

- Workflow instance can start new workflow instance from running workflow
- Agent can start any workflow at any time

## Steps

- Sequential/parallel execution
- Step-to-task assignment (any level)

## Limits (optional but recommended)

- Message limit
- Context size limit
- Time limit

## Overseeing (optional)

- Agent to check for loops/deviation

## Step Behavior

- Automatic completion once agent is done
- Continued run (agent stops itself, even if nothing left to do - e.g., waiting on incoming message)
