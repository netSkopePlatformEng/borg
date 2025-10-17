# Custom AI Agents

Reusable AI agents for specialized development tasks. These agents can be invoked in any project to enforce standards and automate quality checks.

## Available Agents

### [Go Code Quality Agent](go-code-quality-agent.md)
Enforces Go best practices, code quality standards, and consistency across all Go projects.

**Key Features**:
- Error handling analysis
- Structured logging enforcement (zerolog preferred)
- Function design validation
- Interface-based design promotion
- Documentation quality checks

**Usage**:
```
Invoke the Go code quality agent to review this codebase for best practices
```

### [Go Testing Agent](go-testing-agent.md)
Implements comprehensive testing strategies following CNCF and Kubernetes standards.

**Key Features**:
- Ginkgo/Gomega test framework (mandatory)
- Interface-based design for testability
- Mock generation with gomock
- Controller and reconciler testing patterns
- 85%+ test coverage enforcement

**Usage**:
```
Use the Go testing agent to create comprehensive tests for this Kubernetes controller
```

## Creating Custom Agents

To create a new agent:

1. **Define Purpose**: What specific task does this agent solve?
2. **Document Standards**: What patterns and conventions should it enforce?
3. **Provide Examples**: Include good vs bad code examples
4. **Create Templates**: Reusable templates for common outputs
5. **Save as Markdown**: Create `{agent-name}-agent.md` in this directory

## Agent Philosophy

Agents are:
- **Reusable**: Work across multiple projects
- **Opinionated**: Enforce specific standards
- **Educational**: Teach best practices through examples
- **Automatable**: Can be invoked via CI/CD

## Integration with Claude Code

Agents are invoked using the Task tool:

```
Use the {agent-name} agent to {perform specific task}
```

Claude will:
1. Load the agent's instructions
2. Apply agent standards to current context
3. Provide specific recommendations
4. Optionally make fixes

## Best Practices

- **Scope agents narrowly**: One agent, one purpose
- **Include real examples**: Show actual code patterns
- **Document exceptions**: When to deviate from standards
- **Version agents**: Update as standards evolve
- **Share across teams**: Agents promote consistency

---

**Agents are drones in the Borg collective - specialized units optimized for specific tasks.**
