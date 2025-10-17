# Jira Integration with Claude Code

Automate Jira ticket creation, updates, and queries using Claude Code and the Jira MCP server.

## Overview

The Jira MCP server enables Claude Code to:
- Create tickets (bugs, tasks, stories, CM tickets)
- Update ticket fields and status
- Query tickets using JQL
- Add comments and attachments
- Manage sprints and releases

## Contents

- **[Custom Fields Reference](custom-fields-reference.md)** - Complete mapping of Jira custom fields
- **[CM Ticket Guide](cm-ticket-guide.md)** - Creating Change Management tickets
- **[Automation Examples](jira-automation-examples.md)** - Real-world automation patterns

## Quick Start

### Prerequisites

- Jira MCP server installed and configured
- Jira API token with appropriate permissions
- Claude Code installed

### Basic Usage

Ask Claude to interact with Jira:

```
Create a SYS ticket for fixing the SR-IOV configuration issue on c1-sjc3 cluster
```

Claude will:
1. Use appropriate project (SYS)
2. Set correct custom fields (Environment Class: FedRAMP, Component: Kubernetes)
3. Create the ticket
4. Return the ticket URL

## Common Jira Tasks

### Creating Tickets

#### Bug Ticket
```
Create a bug ticket in SYS for a CVE-2024-1234 vulnerability in nginx-ingress.
Environment: FedRAMP, Severity: High
```

#### Task Ticket
```
Create a task in SYS to upgrade Kubernetes from 1.28 to 1.29 on all FedRAMP clusters
```

#### Story Ticket
```
Create a story in EP for implementing automated volume attachment detection
```

### Creating CM Tickets

```
Create a CM ticket for updating SR-IOV VF configuration on c1-sjc3.
Maintenance window: 2025-10-20 20:00-03:00 PDT
```

See [CM Ticket Guide](cm-ticket-guide.md) for complete details.

### Querying Tickets

```
Find all open security tickets in SYS project with High priority
```

```
List my active tickets across all projects
```

### Updating Tickets

```
Update SYS-12345 to In Progress status and add a comment about
starting the implementation
```

## Project Configurations

### Default Project
- **SYS** - Default project for infrastructure and Kubernetes work

### Project-Specific Settings

#### SYS Project
- **Component**: Kubernetes (default)
- **Environment Class**: FedRAMP (default)

#### ENG Project
- **Epic Link**: ENG-209453
- **Component**: Product Security

#### EP Project
- **Epic Link**: EP-17741
- **Component**: Developer Experience

See [Custom Fields Reference](custom-fields-reference.md) for complete project mappings.

## Custom Field Conventions

### Always Set
- **Component**: "Kubernetes" for infrastructure changes
- **Environment Class**: "FedRAMP" for production, "Non-Production" for dev/staging
- **Labels**: Descriptive labels for searching (e.g., "security", "infrastructure")

### When Creating CM Tickets
- **Datacenter**: Target datacenter(s) - e.g., "c1-sjc3"
- **Change Type**: "Infrastructure" > "Configuration Change"
- **Requested Begin/End Dates**: Must align with approved maintenance windows

### Print URLs
After creating or updating tickets, always print the ticket URL:
```
https://netskope.atlassian.net/browse/SYS-12345
```

## JQL Query Examples

### Security Tickets
```jql
project = SYS AND cf[18029] = "Security" AND status != Closed
```

### FedRAMP Issues
```jql
project = SYS AND cf[22697] = "FedRAMP" AND status IN (Open, "In Progress")
```

### My Active Work
```jql
assignee = currentUser() AND status IN (Open, "In Progress", "Code Review")
```

See [Custom Fields Reference](custom-fields-reference.md) for field IDs and more query examples.

## Workflow Integration

### 1. Planning Work
```
Find all open Kubernetes-related tickets in SYS that are unassigned
```

### 2. Creating Work Items
```
Create a ticket for the change we discussed, linked to SYS-27981
```

### 3. Tracking Progress
```
Update SYS-12345 to Code Review and link PR #456
```

### 4. Creating CM Tickets
Before creating a CM ticket, always check maintenance windows:
https://gatekeeper.netskope.io/rules/maintenance-windows

```
Check the approved maintenance window for c1-sjc3, then create a CM ticket
for the VF configuration update during that window
```

## Best Practices

### 1. Provide Complete Context
```
Create a security bug in SYS for CVE-2024-1234.
Component: nginx-ingress 1.8.0
Environment: FedRAMP (c1-sjc3, sfo1)
Severity: High
Remediation: Upgrade to 1.8.1
```

### 2. Use Templates
Follow the templates in [CM Ticket Guide](cm-ticket-guide.md) and [Custom Fields Reference](custom-fields-reference.md).

### 3. Link Related Work
```
Create a task for implementing the fix, linked to parent epic SYS-12000
```

### 4. Verify Before Creating CM Tickets
1. Check approved maintenance windows
2. Verify datacenter names match approved sites
3. Ensure all required fields are populated
4. Review MOP sections for completeness

## Automation Examples

### Bulk Ticket Creation
```
Create tickets for all CVEs in this security scan report (scan-results.json)
```

### Status Updates
```
Move all tickets in the "Testing" column to "Ready for Prod" if tests passed
```

### Sprint Planning
```
List all unestimated tickets in the current sprint and suggest story points
```

See [Automation Examples](jira-automation-examples.md) for detailed workflows.

## Troubleshooting

### Authentication Issues
Ensure your Jira API token is configured in the MCP server settings.

### Custom Field Errors
Check field IDs in [Custom Fields Reference](custom-fields-reference.md) - they vary by project.

### Permission Errors
Verify your Jira account has permissions for:
- Creating tickets in target project
- Updating ticket fields
- Viewing restricted projects

## Resources

- **Jira API Documentation**: https://developer.atlassian.com/cloud/jira/platform/rest/v3/
- **Custom Fields Reference**: [custom-fields-reference.md](custom-fields-reference.md)
- **CM Field Guide**: https://netskope.atlassian.net/wiki/spaces/OE/pages/3298853577/Change+Management+ticket+field+guide

---

**Jira automation is a cornerstone of the Borg collective. Master these patterns to multiply your productivity.**
