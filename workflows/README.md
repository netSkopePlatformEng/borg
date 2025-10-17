# Automation Workflows

Real-world automation patterns using Claude Code, MCP servers, and integrated tools. These workflows demonstrate how to combine multiple capabilities for powerful automation.

## Available Workflows

### [Multi-Cluster Operations](multi-cluster-operations.md)
Perform operations across multiple Kubernetes clusters safely and efficiently.

**Key Patterns**:
- Using `clusters_exec_all` instead of bash loops
- Parallel execution with error handling
- Collecting and aggregating results

### [CM Ticket Automation](cm-ticket-automation.md)
End-to-end automation for creating and managing Change Management tickets.

**Key Patterns**:
- Checking maintenance windows
- Populating all required fields
- Linking to related tickets
- Following approval workflows

## Workflow Philosophy

Good workflows:
- **Break down complexity**: Step-by-step instructions
- **Handle errors**: Graceful failure and rollback
- **Provide feedback**: Progress updates and results
- **Are repeatable**: Work consistently across runs
- **Include verification**: Confirm success at each step

## Creating New Workflows

To document a new workflow:

1. **Describe the goal**: What are we trying to accomplish?
2. **List prerequisites**: What tools, permissions, setup required?
3. **Provide step-by-step instructions**: How to execute the workflow
4. **Include examples**: Show actual commands and outputs
5. **Add troubleshooting**: Common issues and solutions

## Common Workflow Patterns

### 1. Multi-Step Automation
```
Step 1: Verify current state
Step 2: Create backup/snapshot
Step 3: Perform change
Step 4: Validate change
Step 5: Rollback if needed
```

### 2. Cross-System Integration
```
Step 1: Query Kubernetes for issues
Step 2: Create Jira tickets for each issue
Step 3: Link tickets to parent epic
Step 4: Return ticket URLs
```

### 3. Batch Operations
```
Step 1: Get list of targets
Step 2: For each target (safely):
  - Perform operation
  - Verify success
  - Continue or abort
Step 3: Aggregate results
```

## Using Workflows with Claude Code

### Method 1: Reference the Workflow
```
Follow the multi-cluster operations workflow to check node status
across all our Kubernetes clusters
```

### Method 2: Request Workflow Creation
```
Create a workflow for rotating TLS certificates across all clusters.
Follow the patterns in workflows/multi-cluster-operations.md
```

### Method 3: Customize Existing Workflow
```
Adapt the CM ticket automation workflow for a Kubernetes version upgrade.
Include pre-checks for pod disruption budgets.
```

## Best Practices

### Plan Before Executing
```
Show me the plan for this workflow before executing any steps
```

### Verify at Each Step
```
After each step, verify success before continuing
```

### Use Idempotency
Design workflows that can be re-run safely:
- Check if operation already completed
- Skip completed steps
- Don't fail on "already exists" errors

### Log Everything
Record what happened for troubleshooting:
- Commands executed
- Outputs received
- Errors encountered
- Decisions made

## Integration with Tools

### Claude Code + MCP Servers
Most powerful combination - Claude orchestrates, MCP servers execute.

### Claude Code + Bash
For local operations and command orchestration.

### Claude Code + Git
For change tracking and deployment workflows.

### Claude Code + Jira
For work tracking and change management.

## Example: End-to-End Infrastructure Change

```
1. Check approved maintenance window for c1-sjc3
2. Create CM ticket with all required fields
3. Verify current cluster state using k8s MCP
4. Execute change using documented MOP
5. Validate change was successful
6. Update CM ticket with results
7. Close CM ticket if successful
```

---

**Workflows transform the Borg from individual drones to a coordinated collective.**
