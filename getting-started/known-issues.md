# Known Issues and Workarounds

This document tracks known issues encountered while using Claude Code, MCP servers, and related tools, along with workarounds and solutions.

## Claude Code Issues

### MCP Server Disconnects on macOS with Claude Code 0.6.1

**Issue**: [claude-code#9127](https://github.com/anthropics/claude-code/issues/9127)

**Description**: MCP servers intermittently disconnect when using Claude Code 0.6.1 on macOS, requiring manual reconnection or restart of Claude Code.

**Symptoms**:
- MCP server tools become unavailable mid-conversation
- Error messages about MCP server connection failures
- Need to restart Claude Code to restore MCP functionality

**Workaround**:
- Monitor for disconnection messages during long-running operations
- Restart Claude Code when MCP servers disconnect
- Break large operations into smaller chunks to reduce impact of disconnects
- Check MCP server logs for connection issues

**Status**: Open issue - tracking upstream fix

**Impact**: Medium - affects workflow reliability but workaround exists

**Affected Versions**:
- Claude Code 0.6.1
- macOS (specific versions TBD)

**Related MCP Servers**:
- Kubernetes MCP Server
- Atlassian MCP Server (Jira)
- Potentially all MCP servers

---

## Jira Integration Issues

### (No known issues at this time)

---

## Kubernetes MCP Server Issues

### (No known issues at this time - see Claude Code issue above for MCP connectivity)

---

## AWS Integration Issues

### (No known issues at this time)

---

## Reporting New Issues

If you encounter a new issue:

1. **Check this document first** to see if it's already known
2. **Search GitHub issues**:
   - Claude Code: https://github.com/anthropics/claude-code/issues
   - Kubernetes MCP: https://github.com/Flux159/kubernetes-mcp-server/issues
3. **Document the issue**:
   - Clear description of the problem
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (OS, versions, etc.)
   - Workarounds if discovered
4. **Add to this document**:
   - Create a new section under the appropriate category
   - Link to GitHub issue
   - Include workaround if available
   - Update README.md to reference this guide

### Issue Template

```markdown
### Brief Title of Issue

**Issue**: [link-to-github-issue]

**Description**: Clear description of what's wrong

**Symptoms**:
- Bullet list of what users will observe
- Error messages
- Unexpected behavior

**Workaround**:
- Steps to work around the issue
- Alternative approaches
- Any mitigation strategies

**Status**: Open/In Progress/Fixed in version X

**Impact**: Low/Medium/High - description of business impact

**Affected Versions**:
- Tool/component versions affected

**Related Components**:
- What other systems/tools are involved
```

---

## Fixed Issues (Historical Reference)

### (No fixed issues yet - this section will grow over time)

When issues are resolved:
1. Move them to this section
2. Document the fix/resolution
3. Include version where it was fixed
4. Keep for historical reference and knowledge sharing

---

## Best Practices to Avoid Issues

### MCP Server Stability
- Keep MCP server configurations simple
- Monitor server logs during operations
- Use health checks before critical operations
- Have fallback manual procedures documented

### Claude Code Usage
- Keep Claude Code updated to latest stable version
- Clear cache if experiencing unusual behavior: `rm -rf ~/.claude/cache`
- Use `/help` command to verify environment health
- Monitor token usage to avoid context limits

### Multi-Cluster Operations
- Test on single cluster before scaling to multiple clusters
- Use continue_on_error: true for resilience
- Implement proper timeouts
- Have rollback procedures documented

### Jira Automation
- Validate custom field IDs before bulk operations
- Test ticket creation in non-production projects first
- Keep API tokens secure and rotated
- Monitor rate limits

---

*Last Updated: 2025-10-17*
*Document Maintainer: Borg Collective*
