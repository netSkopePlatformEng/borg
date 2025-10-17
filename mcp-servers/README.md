# MCP Servers - Model Context Protocol

MCP (Model Context Protocol) servers extend Claude Code's capabilities by providing integrations with external systems like Kubernetes, Jira, AWS, and more.

## What are MCP Servers?

MCP servers are plugins that allow Claude Code to:
- **Read and write data** from external systems
- **Execute operations** on remote services
- **Provide specialized tools** for specific domains

Think of MCP servers as "sense organs" for Claude Code - they let it interact with the world beyond your local filesystem.

## Available MCP Servers

### Kubernetes MCP Server
Enables multi-cluster Kubernetes operations.

**Capabilities**:
- List, get, create, update, delete Kubernetes resources
- Switch between clusters
- Execute operations across all clusters simultaneously
- Query resources with label selectors

**Setup**: See [kubernetes-mcp-setup.md](kubernetes-mcp-setup.md)

### Atlassian MCP Server
Automates Jira ticket management using the official Atlassian MCP server.

**Capabilities**:
- Create tickets (bugs, tasks, stories, CM tickets)
- Update ticket fields and status
- Query tickets using JQL
- Add comments and attachments
- Search issues and manage sprints

**Setup**:
```bash
claude mcp add --scope user --transport sse atlassian https://mcp.atlassian.com/v1/sse
```

Authenticate with your Atlassian account when prompted. See [Jira Integration Guide](../jira/README.md) for details.

## Installation

Most MCP servers can be added using the `claude mcp add` command:

### Atlassian MCP (Recommended)
```bash
claude mcp add --scope user --transport sse atlassian https://mcp.atlassian.com/v1/sse
```

### Other MCP Servers
Some MCP servers may require manual configuration in Claude Code's settings with specific commands, arguments, or environment variables. Refer to the specific server's documentation for setup instructions.

## Using MCP Servers with Claude Code

### Explicit Invocation

Tell Claude to use a specific MCP server:

```
Use the k8s MCP server to list all pods across all clusters
```

### Implicit Invocation

Claude automatically uses appropriate MCP servers based on context:

```
Create a Jira ticket for this infrastructure change
```

Claude will use the Jira MCP server automatically.

## Common Patterns

### Multi-Cluster Kubernetes Operations

**IMPORTANT**: Always use `clusters_exec_all` for operations across multiple clusters. Never write bash loops.

```
Use the k8s MCP server to check node status across all clusters
```

### Jira Automation

```
Create a CM ticket for the change we discussed. Check approved
maintenance windows first.
```

### Combined Workflows

```
1. Use k8s MCP to verify the change is ready
2. Create a Jira CM ticket with the details
3. Execute the change during the maintenance window
```

## Best Practices

### 1. Be Explicit About Which Server
```
Use the k8s MCP server to get VolumeAttachments
```

### 2. Specify the Operation Clearly
```
Use clusters_exec_all to list nodes - don't use bash loops
```

### 3. Provide Context
```
Use the Jira MCP to create a ticket in SYS project with
Component: Kubernetes and Environment: FedRAMP
```

### 4. Verify Outputs
Review MCP server responses before accepting results.

## Troubleshooting

### MCP Server Not Found
- Check MCP server is installed and configured
- Verify the server binary is in your PATH
- Review Claude Code's MCP configuration

### Authentication Errors
- Check API tokens and credentials
- Verify environment variables are set
- Test credentials manually (e.g., `curl` for API endpoints)

### Permission Errors
- Ensure your credentials have appropriate permissions
- Check RBAC for Kubernetes, project access for Jira
- Verify network connectivity (VPN requirements, etc.)

## Developing Custom MCP Servers

You can create custom MCP servers for your own integrations. The MCP specification is available at:
https://modelcontextprotocol.org

Example use cases for custom servers:
- Internal ticketing systems
- Custom deployment platforms
- Monitoring and observability tools
- Cloud provider APIs

## Resources

- **MCP Specification**: https://modelcontextprotocol.org
- **Kubernetes MCP Setup**: [kubernetes-mcp-setup.md](kubernetes-mcp-setup.md)
- **Atlassian MCP Setup**: See [Jira Integration Guide](../jira/README.md#setup) for one-line setup
- **Claude Code Docs**: https://docs.claude.com/en/docs/claude-code

---

**MCP servers are the Borg's connection to the universe - extending reach across all systems.**
