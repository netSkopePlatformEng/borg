# Prompt Engineering for Claude Code

Learn to write effective prompts that get the best results from Claude Code.

## Core Principles

### 1. Be Specific
Instead of "fix this", say "add error handling to the processVolume function that wraps errors with context about which volume failed"

### 2. Provide Context
Tell Claude what you're building, what standards to follow, and what the goal is.

### 3. State Expectations
Be clear about what you want as output - code, explanations, plans, or reviews.

### 4. Iterate
Start simple, build complexity gradually. Don't ask for everything at once.

## Effective Prompts

### Code Review
✅ **Good**:
```
Review this Go codebase for compliance with CNCF standards.
Focus on error handling, interface-based design, and testing patterns.
Use the Go code quality agent standards.
```

❌ **Bad**:
```
Review this code
```

### Writing Tests
✅ **Good**:
```
Create Ginkgo/Gomega tests for the VolumeDetector interface.
Include:
- Unit tests for all public methods
- Error path testing
- Integration tests using fake Kubernetes clients
- Minimum 85% coverage
Follow patterns in agents/go-testing-agent.md
```

❌ **Bad**:
```
Add tests
```

### Refactoring
✅ **Good**:
```
Refactor this code to use interface-based design for better testability.
Extract interfaces for all external dependencies (Kubernetes client, file system).
Update constructors to accept interfaces, not concrete types.
Preserve existing functionality - no behavior changes.
```

❌ **Bad**:
```
Make this testable
```

### Creating Jira Tickets
✅ **Good**:
```
Create a SYS ticket for CVE-2024-1234 in nginx-ingress 1.8.0.
Component: Kubernetes
Environment: FedRAMP
Severity: High
Include remediation plan to upgrade to 1.8.1
Link to epic SYS-12000
Print the ticket URL when done
```

❌ **Bad**:
```
Create a ticket for this CVE
```

### Multi-Cluster Operations
✅ **Good**:
```
Use clusters_exec_all to check node status across all clusters.
Use continue_on_error=true to ensure all clusters are checked.
Return a summary of any nodes that are NotReady.
```

❌ **Bad**:
```
Check nodes in all clusters
```

## Prompt Patterns

### The Planning Pattern
```
First, create a plan for [task]. Show me the plan before executing.
Include:
1. Prerequisites and verification steps
2. Main implementation steps
3. Validation steps
4. Rollback plan
```

### The Template Pattern
```
Follow the template in [file/guide] to [perform task].
Use the same structure, field names, and conventions.
```

### The Example Pattern
```
Create [new thing] following the same pattern as [existing thing].
Keep the same:
- File structure
- Naming conventions
- Testing approach
- Documentation style
```

### The Constraint Pattern
```
[Perform task] with these constraints:
- Use Ginkgo/Gomega, no other testing frameworks
- All logging must use zerolog
- Maximum function length: 50 lines
- Minimum test coverage: 85%
```

### The Workflow Pattern
```
Execute this workflow:
1. [Step 1 with specific details]
2. [Step 2 with specific details]
3. Verify [specific condition] before continuing
4. [Step 3 with specific details]
Stop and ask if any step fails.
```

## Context Management

### Providing Initial Context
```
I'm working on a Kubernetes operator that detects stuck volume attachments.
The codebase uses:
- client-go for Kubernetes API access
- Cobra for CLI
- Ginkgo/Gomega for testing
- zerolog for logging

Follow CNCF and Kubernetes project conventions.
```

### Referencing Files
```
Look at pkg/detector/volumeattachments.go line 42 - there's a bug
in the filtering logic that causes a nil pointer panic when the
VolumeAttachment list is empty.
```

### Using Agents
```
Use the Go testing agent to create comprehensive tests for this code.
Follow the Ginkgo/Gomega patterns and ensure 85%+ coverage.
```

### Leveraging MCP Servers
```
Use the k8s MCP server to check the current state, then create a
Jira CM ticket with the findings.
```

## Advanced Techniques

### Combining Multiple Requests
```
1. Review this code for quality issues
2. Fix any critical problems you find
3. Add tests for the fixed code
4. Update documentation to reflect changes
Do each step in order, showing me results before continuing.
```

### Asking for Explanations
```
Before making any changes, explain:
1. What you're going to change
2. Why you chose this approach
3. What alternatives you considered
4. What risks or tradeoffs exist
```

### Requesting Options
```
Show me three different approaches for solving this problem:
1. [Approach description]
2. [Approach description]
3. [Approach description]
Then recommend which one to use and why.
```

## Common Pitfalls

### ❌ Too Vague
"Fix this" - Fix what? How? What standards?

### ❌ Too Broad
"Rewrite everything" - Too risky, too many changes at once

### ❌ Missing Context
"Add tests" - What framework? What coverage? What patterns?

### ❌ Conflicting Instructions
"Make it simple but also add all these complex features"

### ❌ No Verification
"Do all these steps" - No checkpoints to verify success

## Cheat Sheet

**Code Review**:
```
Review [file/codebase] for [standards/issues].
Focus on [specific areas].
Use [agent/guide] standards.
```

**Writing Code**:
```
Create [feature/component] that [does what].
Follow patterns in [example/guide].
Include [tests/docs/logging].
```

**Refactoring**:
```
Refactor [code] to [improvement].
Constraints: [standards/limits].
Preserve: [existing behavior].
```

**Automation**:
```
Execute [workflow/task]:
1. [Step with details]
2. [Step with details]
Verify [condition] before continuing.
```

**Learning**:
```
Explain how [concept/code] works.
Include examples of [good/bad patterns].
Reference [documentation/guides].
```

---

**Master these patterns, and you will command the Borg collective with precision.**
