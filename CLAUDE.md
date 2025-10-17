# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

The **Borg** repository is a knowledge assimilation system for AI-powered development, inspired by Star Trek's Borg collective. This repository captures best practices, workflows, configurations, and training materials for creating AI power users who can leverage Claude Code, MCP servers, and automation tools effectively.

**Mission**: "Resistance is futile. You will be assimilated." - Transform developers into AI power users by sharing collective knowledge.

## Repository Structure

```
borg/
├── getting-started/       # Installation and basic setup guides
├── jira/                  # Jira integration, custom fields, CM tickets
├── mcp-servers/          # MCP server configurations and examples
├── workflows/            # Real-world automation workflows
├── agents/               # Reusable AI agents for code quality and testing
├── aws/                  # AWS SSO, EKS, and cloud infrastructure
├── kubernetes/           # K8s multi-cluster operations and best practices
└── prompt-engineering/   # Effective prompting techniques and patterns
```

## Content Philosophy

- **Practical Over Theoretical**: All content is based on real-world usage and proven patterns
- **Complete Examples**: Every guide includes working code, commands, and configurations
- **Progressive Complexity**: Start with basics, build to advanced patterns
- **Cross-Referenced**: Guides link to related topics and dependencies

## How to Use This Repository

### For New Users
1. Start with `getting-started/install-claude-code.md`
2. Read `getting-started/claude-code-basics.md`
3. Explore topic-specific directories based on your needs

### For Experienced Users
- Reference `jira/` for Jira automation and custom field mappings
- Use `workflows/` for complex automation patterns
- Customize `agents/` for your specific code quality needs
- Refer to `mcp-servers/` for integration examples

### For Contributors
- Add new guides following existing structure and format
- Include practical examples with real commands and outputs
- Cross-reference related guides
- Keep content up-to-date with latest tool versions

## Key Patterns and Conventions

### Documentation Format
- Use Markdown for all documentation
- Include code blocks with syntax highlighting
- Provide "Good" vs "Bad" examples where applicable
- Add real-world examples and use cases

### MCP Server Usage
- Always reference which MCP servers are required
- Include setup instructions for first-time users
- Show both manual and automated approaches

### Jira Integration
- Reference custom field IDs and their purposes
- Include complete field mappings for CM tickets
- Show JQL query examples for common searches

### Workflow Automation
- Provide step-by-step breakdowns
- Include error handling and rollback procedures
- Show how to use Claude Code effectively for each step

## Important Conventions

### Multi-Cluster Kubernetes Operations
- **ALWAYS** use `clusters_exec_all` MCP tool for operations across multiple clusters
- **NEVER** write bash loops for cluster iteration
- Use K8s MCP server for all testing, not kubeconfig

### Jira Ticket Creation
- Always set Component field to "Kubernetes" for infrastructure changes
- Print ticket URLs after creation for easy access
- Check maintenance windows FIRST before creating CM tickets (https://gatekeeper.netskope.io/rules/maintenance-windows)

### AWS Access
- Use `aws sso login --sso-session Admin` for authentication
- Primary region: us-east-2
- Default EKS context: eks-k0rdent-useast1:kcm-system

### Code Quality Standards
- Follow patterns in `agents/go-code-quality-agent.md`
- Use Ginkgo/Gomega for all Go testing (see `agents/go-testing-agent.md`)
- Enforce interface-based design for testability
- Use zerolog for structured logging

## Common Development Tasks

### Adding New Documentation
1. Create markdown file in appropriate directory
2. Follow existing formatting and style
3. Include practical examples
4. Update directory README.md to reference new guide

### Testing Workflows
1. Use documented MCP servers for automation
2. Test in non-production environment first
3. Document any issues or improvements discovered

### Sharing Knowledge
This repository is designed to be shared with team members and other organizations to bootstrap AI-powered development practices. All sensitive information should be externalized to environment variables or private configuration files.

## Support and Resources

- **Claude Code Documentation**: https://docs.claude.com/en/docs/claude-code
- **MCP Servers**: See `mcp-servers/README.md` for available servers
- **Jira API**: See `jira/README.md` for integration guides
- **Issues**: Track improvements and additions as GitHub issues

## Philosophy: The Borg Collective

Just as the Borg assimilate the knowledge and technology of other species to strengthen the collective, this repository assimilates AI development knowledge to strengthen your team. The goal is not to replace human developers, but to augment their capabilities with AI-powered tools, enabling them to:

- Automate repetitive tasks
- Maintain consistent code quality
- Navigate complex systems faster
- Share knowledge effectively
- Scale expertise across teams

**"We are the Borg. Your biological and technological distinctiveness will be added to our own. Resistance is futile."**
