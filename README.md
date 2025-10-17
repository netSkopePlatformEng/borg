# The Borg Collective - AI Knowledge Assimilation

```
 _______  _______  ______    _______
|  _    ||       ||    _ |  |       |
| |_|   ||   _   ||   | ||  |    ___|
|       ||  | |  ||   |_||_ |   | __
|  _   | |  |_|  ||    __  ||   ||  |
| |_|   ||       ||   |  | ||   |_| |
|_______||_______||___|  |_||_______|

"Resistance is futile. You will be assimilated."
```

## Mission Statement

The Borg repository is a **collective knowledge base** for AI-powered development. Just as the Borg assimilate the knowledge and technology of other species to strengthen the collective, this repository assimilates best practices, workflows, and configurations to create AI power users.

**Our Goal**: Transform developers into AI power users by sharing practical knowledge about Claude Code, MCP servers, Jira automation, Kubernetes operations, and modern development workflows.

## What's Inside

### Getting Started
- **[API Access](getting-started/api-access.md)** - Get Claude Code API access at Netskope (Vertex AI)
- **[Install Claude Code](getting-started/install-claude-code.md)** - Get up and running with Claude Code CLI
- **[Claude Code Basics](getting-started/claude-code-basics.md)** - Essential concepts and workflows

### Jira Integration
- **[Jira Overview](jira/README.md)** - Automating Jira with Claude Code
- **[Custom Fields Reference](jira/custom-fields-reference.md)** - Complete Jira custom field mappings
- **[CM Ticket Guide](jira/cm-ticket-guide.md)** - Creating Change Management tickets
- **[Automation Examples](jira/jira-automation-examples.md)** - Real-world automation patterns

### MCP Servers
- **[MCP Overview](mcp-servers/README.md)** - Introduction to Model Context Protocol servers
- **[Kubernetes MCP](mcp-servers/kubernetes-mcp-setup.md)** - Multi-cluster Kubernetes operations
- **[Jira MCP](mcp-servers/jira-mcp-setup.md)** - Jira API integration

### Workflows & Automation
- **[Workflow Overview](workflows/README.md)** - Common automation patterns
- **[Multi-Cluster Operations](workflows/multi-cluster-operations.md)** - K8s operations across clusters
- **[CM Ticket Automation](workflows/cm-ticket-automation.md)** - End-to-end CM ticket workflows

### Custom Agents
- **[Agents Overview](agents/README.md)** - Reusable AI agents for development
- **[Go Code Quality Agent](agents/go-code-quality-agent.md)** - Enforce Go best practices
- **[Go Testing Agent](agents/go-testing-agent.md)** - CNCF/Kubernetes testing standards

### AWS & Cloud
- **[AWS Overview](aws/README.md)** - AWS SSO, EKS, and cloud operations
- **[SSO Setup](aws/sso-setup.md)** - AWS SSO configuration and troubleshooting

### Kubernetes
- **[Kubernetes Overview](kubernetes/README.md)** - K8s operations and best practices
- **[Multi-Cluster Best Practices](kubernetes/multi-cluster-best-practices.md)** - Managing multiple clusters

### Prompt Engineering
- **[Prompt Engineering Overview](prompt-engineering/README.md)** - Writing effective AI prompts
- **[Effective Prompts](prompt-engineering/effective-prompts.md)** - Patterns for better results

## Quick Start

### 1. Get API Access (Netskope Only)
```bash
# See getting-started/api-access.md for detailed instructions
# Contact Scott (sleibrand) to request access
# Configure Vertex AI credentials from 1Password
```

### 2. Install Claude Code
```bash
# See getting-started/install-claude-code.md for detailed instructions
curl -sSL https://claude.ai/install.sh | bash
```

### 3. Explore a Topic
Choose a directory based on what you want to learn:
- New to AI-assisted development? Start with `getting-started/`
- Need to automate Jira? Check out `jira/`
- Working with Kubernetes? Visit `kubernetes/` and `mcp-servers/`
- Building Go applications? See `agents/` for code quality patterns

**Note**: All guides assume you have API access and Claude Code installed. If you jumped ahead, make sure to complete steps 1-2 first.

### 4. Try a Workflow
```bash
# Example: Multi-cluster Kubernetes operations
cd workflows/
cat multi-cluster-operations.md

# Example: Create a CM ticket
cd ../jira/
cat cm-ticket-guide.md
```

## Philosophy: Collective Knowledge

The Borg operate as a collective consciousness where knowledge is instantly shared across all drones. This repository embodies that philosophy for software development:

- **Shared Experiences**: Real-world solutions, not theoretical concepts
- **Practical Patterns**: Working code, commands, and configurations
- **Continuous Assimilation**: Always growing as we learn new techniques
- **Zero Resistance**: Make AI adoption effortless through clear documentation

### What Makes This Different?

Unlike typical documentation repositories, Borg focuses on:

1. **Real-World Examples**: Every guide comes from actual usage and proven patterns
2. **Complete Context**: No assuming prior knowledge - we explain the "why" not just the "how"
3. **Tool Integration**: Shows how Claude Code, MCP servers, and Jira work together
4. **Progressive Learning**: Start simple, gradually build to complex automation
5. **Cross-Functional**: Covers development, operations, and project management

## Who Should Use This?

### Developers
- Learn to use AI for code reviews, refactoring, and testing
- Understand interface-based design and Go best practices
- Automate repetitive development tasks

### DevOps/SRE Engineers
- Master multi-cluster Kubernetes operations
- Automate infrastructure changes with CM tickets
- Use MCP servers for cloud automation

### Project Managers
- Understand what's possible with AI-assisted development
- Learn to create and track Jira tickets programmatically
- See how automation improves team velocity

### Anyone Curious About AI
- Practical introduction to AI-assisted development
- Real examples beyond basic chatbot usage
- Understanding the future of software development

## Contributing

The collective grows stronger with each contribution. To add your knowledge:

1. **Add Documentation**: Create guides in the appropriate directory
2. **Share Workflows**: Document your automation patterns in `workflows/`
3. **Create Agents**: Build reusable AI agents for common tasks
4. **Improve Examples**: Enhance existing guides with better examples

Follow the patterns in existing documentation:
- Use clear, descriptive titles
- Include practical examples
- Explain the "why" behind the "how"
- Cross-reference related guides

## The Borg Way

```
┌─────────────────────────────────────────────┐
│  "We are the Borg.                          │
│   Your biological and technological         │
│   distinctiveness will be added to our own. │
│   Resistance is futile."                    │
└─────────────────────────────────────────────┘
```

In this context, "assimilation" means:
- Learning AI-powered development techniques
- Sharing knowledge across the team
- Augmenting human capabilities with AI tools
- Building collective expertise

**Welcome to the collective. Let's build something amazing together.**

---

## Quick Links

- **Claude Code Docs**: https://docs.claude.com/en/docs/claude-code
- **MCP Specification**: https://modelcontextprotocol.org
- **Jira API**: https://developer.atlassian.com/cloud/jira/platform/rest/v3/
- **Kubernetes**: https://kubernetes.io/docs/

## License

This knowledge is meant to be shared. Use it, improve it, and help others assimilate.

---

*Repository initialized on 2025-10-17*
*"The Borg are the ultimate users. We assimilate knowledge to improve ourselves."*
