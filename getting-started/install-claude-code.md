# Installing Claude Code

This guide walks you through installing and setting up Claude Code CLI, Anthropic's official command-line interface for AI-assisted development.

## Important: Netskope Users Start Here

**If you're at Netskope**, you need API access first:
👉 **[Get API Access](api-access.md)** - Set up Vertex AI credentials before installing

After getting API access, come back here to install the CLI.

## Prerequisites

- macOS, Linux, or Windows (WSL2)
- Terminal/command-line access
- Internet connection
- **For Netskope users**: API access configured (see link above)

## Installation

### macOS and Linux

#### Quick Install
```bash
curl -sSL https://claude.ai/install.sh | bash
```

#### Manual Install
1. Download the latest release from the Claude Code website
2. Extract the archive
3. Move the binary to your PATH:
```bash
mv claude /usr/local/bin/
chmod +x /usr/local/bin/claude
```

### Windows (WSL2)
Use the same Linux installation method within WSL2:
```bash
curl -sSL https://claude.ai/install.sh | bash
```

## Verification

Verify the installation:
```bash
claude --version
```

You should see output like:
```
Claude Code CLI version x.x.x
```

## Authentication

### For Netskope Users

**Skip this section** if you're at Netskope - you're using Vertex AI authentication instead.

Your authentication is handled via the service account credentials you configured in the [API Access Guide](api-access.md). The `GOOGLE_APPLICATION_CREDENTIALS` environment variable points to your key file.

### For Non-Netskope Users

1. Run Claude Code:
```bash
claude
```

2. You'll be prompted to authenticate. Follow the on-screen instructions to link your Anthropic account.

3. Once authenticated, you'll have access to Claude Code features.

### API Key Management

Claude Code stores authentication credentials securely in:
- macOS: `~/.claude/`
- Linux: `~/.claude/`
- Windows: `~/.claude/`

**Netskope users**: Your service account key is stored separately (e.g., `~/.claude/YOUR-NAME-claude-code-key.json`)

## Configuration

### Global Configuration

Create a global configuration file at `~/.claude/CLAUDE.md`:

```bash
touch ~/.claude/CLAUDE.md
```

This file can contain project-agnostic instructions that Claude will use across all projects. See the Borg repository's global CLAUDE.md example for inspiration.

### Project-Specific Configuration

In any project, create a `.claude/` directory with project-specific instructions:

```bash
mkdir .claude
touch .claude/CLAUDE.md
```

## Verifying Installation

Test Claude Code with a simple command:

```bash
cd /tmp
mkdir test-project
cd test-project
echo "console.log('Hello, Borg!');" > hello.js
claude
```

Then ask Claude: "Review this JavaScript file and suggest improvements"

## Troubleshooting

### Authentication Issues

If you encounter authentication problems:

1. **Clear credentials**:
```bash
rm -rf ~/.claude/
```

2. **Re-authenticate**:
```bash
claude
```

3. **Check network connectivity**:
```bash
curl -I https://claude.ai
```

### Permission Issues

If you get permission errors:

```bash
sudo chown -R $USER ~/.claude/
chmod -R 700 ~/.claude/
```

### PATH Issues

If `claude` command is not found:

1. **Check if binary is in PATH**:
```bash
echo $PATH
which claude
```

2. **Add to PATH** (add to `~/.bashrc`, `~/.zshrc`, or equivalent):
```bash
export PATH="$PATH:/usr/local/bin"
```

3. **Reload shell**:
```bash
source ~/.bashrc  # or ~/.zshrc
```

## Next Steps

Now that Claude Code is installed:

1. **Read the basics**: Check out [`claude-code-basics.md`](./claude-code-basics.md)
2. **Set up MCP servers**: See [`../mcp-servers/README.md`](../mcp-servers/README.md)
3. **Explore workflows**: Browse [`../workflows/README.md`](../workflows/README.md)

## Advanced Configuration

### MCP Server Setup

MCP (Model Context Protocol) servers extend Claude Code's capabilities. See the `mcp-servers/` directory for:
- Kubernetes MCP server for multi-cluster operations
- Jira MCP server for ticket automation
- Custom MCP server development

### Custom Agents

Create reusable agents for specific tasks. See `agents/` directory for:
- Go code quality enforcement
- Testing framework setup
- Custom workflow automation

### Shell Integration

Add Claude Code to your shell for enhanced productivity:

#### Bash
Add to `~/.bashrc`:
```bash
# Claude Code alias
alias c='claude'
```

#### Zsh
Add to `~/.zshrc`:
```bash
# Claude Code alias
alias c='claude'

# Claude Code completion (if available)
# source <(claude completion zsh)
```

## Resources

- **Official Documentation**: https://docs.claude.com/en/docs/claude-code
- **GitHub Issues**: Report problems and request features
- **Community Examples**: Explore the Borg repository for real-world usage

## Support

If you encounter issues:

1. Check the [Troubleshooting](#troubleshooting) section above
2. Review the official Claude Code documentation
3. Search for similar issues in the community
4. File a bug report with detailed error messages

---

**Welcome to the collective. You are now equipped to leverage AI-assisted development.**

*"Resistance is futile. You will be assimilated... into the ranks of AI power users."*
