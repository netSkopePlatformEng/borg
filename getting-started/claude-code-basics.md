# Claude Code Basics

Learn the essential concepts and workflows for using Claude Code effectively in your daily development work.

## Prerequisites

Before using Claude Code, ensure you have:

1. **API Access** - For Netskope users, see [API Access Guide](api-access.md) to get Vertex AI credentials
2. **Claude Code Installed** - See [Installation Guide](install-claude-code.md)
3. **Environment Variables Set** - If using Netskope Vertex AI, ensure your shell has the required env vars configured

**Note**: All examples in this guide assume you've completed the setup above. When you see `claude`, it means you have your environment properly configured.

## What is Claude Code?

Claude Code is an AI-powered CLI that helps with:
- Code review and refactoring
- Writing tests and documentation
- Debugging and troubleshooting
- Automation and scripting
- Multi-file codebase understanding

Unlike chatbots, Claude Code:
- **Reads your entire codebase** for context
- **Makes direct file changes** with your approval
- **Executes commands** and tools in your environment
- **Integrates with external services** via MCP servers

## Core Concepts

### 1. Context Awareness

Claude Code understands your project by:
- Reading files in your current directory
- Following code references across files
- Understanding project structure and patterns
- Maintaining conversation history

### 2. Tool Usage

Claude Code can:
- **Read files**: View any file in your project
- **Edit files**: Make precise changes with your approval
- **Execute commands**: Run bash, git, npm, etc.
- **Search code**: Find functions, classes, patterns
- **Call MCP servers**: Integrate with Kubernetes, Jira, AWS, etc.

### 3. Planning Mode

Claude Code offers two modes:
- **Plan Mode**: Claude proposes changes, you approve before execution
- **Execute Mode**: Claude makes changes immediately (can be reverted)

Use plan mode for:
- Complex refactoring
- Risky operations
- Learning what Claude will do

## Basic Workflows

### 1. Code Review

```bash
cd /path/to/your/project
claude
```

Ask Claude:
```
Review this codebase for Go best practices. Focus on error handling,
testing gaps, and opportunities for interface-based design.
```

Claude will:
1. Scan relevant files
2. Identify issues
3. Suggest specific improvements
4. Optionally make changes

### 2. Writing Tests

```bash
claude
```

Ask:
```
Add comprehensive Ginkgo/Gomega tests for the VolumeDetector interface.
Include unit tests for all methods and integration tests using fake Kubernetes clients.
```

Claude will:
1. Analyze the interface
2. Create test files with proper structure
3. Write test cases covering happy path and error scenarios
4. Set up mocks and fixtures

### 3. Debugging Issues

```bash
claude
```

Ask:
```
I'm getting "connection refused" errors when connecting to Kubernetes API.
Help me debug this issue.
```

Claude will:
1. Ask clarifying questions
2. Check relevant configurations
3. Suggest diagnostic commands
4. Help fix the root cause

### 4. Refactoring Code

```bash
claude
```

Ask:
```
Refactor this code to use interfaces instead of concrete types,
making it easier to test with mocks.
```

Claude will:
1. Identify concrete dependencies
2. Extract interfaces
3. Update constructors and callers
4. Preserve existing functionality

## Effective Prompting

### Good Prompts

✅ **Specific and actionable**
```
Add error handling to the processVolume function. Wrap errors with
context about which volume failed and why.
```

✅ **Includes context**
```
This is a Kubernetes controller. Add Ginkgo tests following CNCF patterns,
using fake clients and proper reconciliation testing.
```

✅ **States expectations**
```
Refactor this to use dependency injection. All external dependencies
should be interfaces passed to constructors.
```

### Avoid

❌ **Too vague**
```
Make this better
```

❌ **Missing context**
```
Add tests
```

❌ **Unclear scope**
```
Fix everything wrong with this project
```

## Working with Files

### Reading Files

Claude automatically reads files when needed, but you can be explicit:

```
Read the detector.go file and explain how the detection logic works
```

### Editing Files

Claude makes surgical edits:
```
In detector.go, add logging before each API call using zerolog
```

Review changes before accepting.

### Creating Files

```
Create a new file cmd/root.go with a Cobra CLI setup for this tool
```

## Using MCP Servers

MCP servers extend Claude's capabilities. Common servers:

### Kubernetes MCP
```
Use the k8s MCP server to list all pods across all clusters
```

### Jira MCP
```
Create a Jira ticket in the SYS project for this infrastructure change
```

### AWS MCP
```
Check the current AWS SSO session and EKS cluster access
```

See [`../mcp-servers/README.md`](../mcp-servers/README.md) for setup.

## Managing Context

### Starting Fresh

Start a new conversation:
```bash
claude
# Or use Ctrl+C to exit and restart
```

### Providing Context

Help Claude understand your intent:
```
I'm working on a Kubernetes operator that watches VolumeAttachments.
The codebase uses client-go and should follow controller-runtime patterns.
```

### Referencing Files

Point Claude to specific files:
```
Look at pkg/detector/volumeattachments.go - there's a bug in the
filtering logic on line 42
```

## Git Integration

### Code Review Before Commit

```bash
# Make changes
git add .
claude
```

Ask:
```
Review my staged changes before I commit. Check for quality issues,
missing tests, and documentation gaps.
```

### Creating Commits

```bash
claude
```

Ask:
```
Create a git commit for these changes with an appropriate commit message
```

Claude will:
1. Review changes
2. Write a descriptive commit message
3. Execute `git commit` with your approval

### Creating Pull Requests

```bash
claude
```

Ask:
```
Create a pull request for this branch with a summary of changes
```

## Best Practices

### 1. Be Specific About Standards

Tell Claude what standards to follow:
```
Use Ginkgo/Gomega for testing - no other frameworks
Follow the patterns in agents/go-testing-agent.md
```

### 2. Request Explanations

Understand what Claude does:
```
Explain why you chose this approach before making changes
```

### 3. Iterate Gradually

Build up complexity:
```
First, add the interface. Then we'll update the tests.
Finally, we'll refactor the callers.
```

### 4. Verify Changes

Always review:
- File edits before accepting
- Command outputs
- Test results
- Git diffs

### 5. Save Successful Patterns

Document workflows that work well in this repository!

## Common Tasks

### Starting a New Project

```bash
mkdir my-project && cd my-project
git init
claude
```

Ask:
```
Set up a new Go project for Kubernetes volume management.
Include proper directory structure, Makefile, and README.
```

### Adding Features

```
Add a new detection method for orphaned PVCs. Follow the existing
VolumeAttachment detector pattern.
```

### Fixing Bugs

```
There's a nil pointer panic in detector.go line 87 when volume
attachments list is empty. Fix this and add a test to prevent regression.
```

### Writing Documentation

```
Generate a README.md for this project explaining what it does,
how to install it, and how to use it.
```

## Keyboard Shortcuts

- **Ctrl+C**: Cancel current operation / Exit Claude
- **Ctrl+D**: Exit Claude (same as `/exit`)
- **Up/Down Arrow**: Navigate command history
- **Tab**: Auto-complete (if available)

## Slash Commands

- `/help`: Show available commands
- `/clear`: Clear conversation history
- `/exit`: Exit Claude Code

## Next Steps

Now that you understand the basics:

1. **Try a real task**: Use Claude on one of your projects
2. **Set up MCP servers**: See [`../mcp-servers/`](../mcp-servers/README.md)
3. **Learn workflows**: Check [`../workflows/`](../workflows/README.md)
4. **Customize agents**: Explore [`../agents/`](../agents/README.md)

## Tips for Success

1. **Start small**: Begin with simple tasks like code review
2. **Build trust**: Verify Claude's suggestions before accepting
3. **Learn patterns**: Notice what works well and reuse those patterns
4. **Share knowledge**: Document your discoveries in this repository
5. **Combine tools**: Use Claude + MCP servers + custom agents for powerful workflows

---

**You've been assimilated into the basics. Now go forth and augment your development capabilities.**

*"The Borg do not deviate from their purpose. You will learn to leverage AI with the same precision."*
