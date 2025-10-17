# Getting Claude Code API Access

This guide explains how to get access to Claude Code API at Netskope using our internal Vertex AI setup.

## Prerequisites

- Netskope email account
- Access to Netskope 1Password
- npm installed (for Claude Code CLI installation)

## Step 1: Request Access from Scott

Contact Scott (sleibrand) to request Claude Code API access.

**How to request:**
1. Send Scott a direct message (Slack/Email)
2. Request: "Hi Scott, I'd like to get access to Claude Code API. Can you set up a service account for me?"

**What Scott will do:**
Scott will create a service account for you and store the credentials in a shared 1Password vault.

## Step 2: Receive Confirmation

You'll receive a confirmation message similar to this:

```
Vault shared with YOUR-EMAIL@netskope.com
Local key file removed
==================================================
✅ Process completed successfully!
==================================================
Service account created: YOUR-NAME-claude-code@peas-01.iam.gserviceaccount.com
Credentials stored in 1Password vault: sleibrand-YOUR-NAME
==================================================
```

## Step 3: Access Your Credentials

1. **Check your email** for a 1Password vault invitation
   - Vault name will be: `sleibrand-YOUR-NAME`
   - Accept the invitation

2. **Open 1Password** and navigate to the shared vault

3. **Find the service account key**
   - Look for a document titled: `YOUR-NAME-claude-code Service Account Key`
   - This contains your Google Cloud service account credentials

4. **Download the credentials file**
   - Save it to a secure location on your machine
   - Recommended location: `~/.claude/YOUR-NAME-claude-code-key.json`
   - **IMPORTANT**: Never commit this file to git!

Example:
```bash
# Create .claude directory if it doesn't exist
mkdir -p ~/.claude

# Download from 1Password and save (do this via 1Password UI)
# Save to: ~/.claude/jdambly-claude-code-key.json
```

## Step 4: Install Claude Code CLI

Install the Claude Code CLI globally using npm:

```bash
npm i -g @anthropic-ai/claude-code
```

Verify installation:
```bash
claude --version
```

## Step 5: Configure Environment Variables

Set up the required environment variables to use Vertex AI.

### Option A: Temporary Setup (For Testing)

Set environment variables for the current session:

```bash
export GOOGLE_APPLICATION_CREDENTIALS=~/.claude/YOUR-NAME-claude-code-key.json
export CLAUDE_CODE_USE_VERTEX=1
export ANTHROPIC_VERTEX_PROJECT_ID=peas-01
export CLOUD_ML_REGION=us-east5
```

Then launch Claude Code:
```bash
claude
```

### Option B: Persistent Setup (Recommended)

Add to your shell configuration file (`~/.bashrc`, `~/.zshrc`, or equivalent):

```bash
# Claude Code - Vertex AI Configuration
export GOOGLE_APPLICATION_CREDENTIALS=~/.claude/YOUR-NAME-claude-code-key.json
export CLAUDE_CODE_USE_VERTEX=1
export ANTHROPIC_VERTEX_PROJECT_ID=peas-01
export CLOUD_ML_REGION=us-east5
```

Then reload your shell:
```bash
source ~/.zshrc  # or ~/.bashrc
```

Now you can launch Claude Code anytime with:
```bash
claude
```

### Option C: Launch Script (Alternative)

Create a launch script for convenience:

```bash
cat > ~/bin/claude-vertex << 'EOF'
#!/bin/bash
export GOOGLE_APPLICATION_CREDENTIALS=~/.claude/YOUR-NAME-claude-code-key.json
export CLAUDE_CODE_USE_VERTEX=1
export ANTHROPIC_VERTEX_PROJECT_ID=peas-01
export CLOUD_ML_REGION=us-east5
claude "$@"
EOF

chmod +x ~/bin/claude-vertex
```

Then launch with:
```bash
claude-vertex
```

## Step 6: Verify Setup

Test that everything is working:

```bash
# Start Claude Code
claude

# Try a simple command
# Ask: "What is 2+2?"
```

If successful, you should get a response from Claude!

## Configuration Details

### Environment Variables Explained

| Variable | Value | Purpose |
|----------|-------|---------|
| `GOOGLE_APPLICATION_CREDENTIALS` | Path to your JSON key file | Authenticates with Google Cloud |
| `CLAUDE_CODE_USE_VERTEX` | `1` | Tells Claude Code to use Vertex AI instead of direct Anthropic API |
| `ANTHROPIC_VERTEX_PROJECT_ID` | `peas-01` | The Google Cloud project ID for Netskope's Claude setup |
| `CLOUD_ML_REGION` | `us-east5` | The region where the Vertex AI endpoint is deployed |

### Service Account Details

- **Project**: `peas-01`
- **Region**: `us-east5`
- **Service Account Pattern**: `YOUR-NAME-claude-code@peas-01.iam.gserviceaccount.com`
- **1Password Vault Pattern**: `sleibrand-YOUR-NAME`

## Troubleshooting

### "Authentication failed" Error

**Problem**: Cannot authenticate with Google Cloud

**Solutions**:
1. Verify the credentials file path is correct
2. Check that the file was downloaded properly from 1Password
3. Ensure the file permissions are readable: `chmod 600 ~/.claude/YOUR-NAME-claude-code-key.json`
4. Verify environment variables are set: `env | grep CLAUDE`

### "Project not found" Error

**Problem**: Cannot find the `peas-01` project

**Solutions**:
1. Verify you have access to the project (ask Scott)
2. Check that `ANTHROPIC_VERTEX_PROJECT_ID=peas-01` is set correctly
3. Ensure you're using the correct service account credentials

### "Region not available" Error

**Problem**: Region `us-east5` not accessible

**Solutions**:
1. Verify `CLOUD_ML_REGION=us-east5` is set correctly
2. Contact Scott to confirm the region configuration

### Environment Variables Not Set

**Problem**: Variables don't persist between sessions

**Solutions**:
1. Make sure you added them to your shell config file (`~/.zshrc` or `~/.bashrc`)
2. Reload your shell: `source ~/.zshrc`
3. Verify with: `echo $GOOGLE_APPLICATION_CREDENTIALS`

### npm Installation Issues

**Problem**: Cannot install Claude Code CLI

**Solutions**:
1. Update npm: `npm install -g npm@latest`
2. Check Node.js version: `node --version` (should be 16+)
3. Try with sudo if permissions issue: `sudo npm i -g @anthropic-ai/claude-code`

## Security Best Practices

### Protecting Your Credentials

1. **Never commit credentials to git**
   - The `.gitignore` in this repo already excludes `*.json` files
   - Double-check before committing: `git status`

2. **Secure file permissions**
   ```bash
   chmod 600 ~/.claude/YOUR-NAME-claude-code-key.json
   ```

3. **Don't share your credentials**
   - Each person should have their own service account
   - Don't copy your credentials to shared locations

4. **Rotate credentials if compromised**
   - Contact Scott immediately if you suspect credentials are exposed
   - He can revoke and create new credentials

### Credential Storage

**✅ Good locations:**
- `~/.claude/YOUR-NAME-claude-code-key.json`
- `~/credentials/YOUR-NAME-claude-code-key.json`

**❌ Bad locations:**
- Project directories (risk of committing to git)
- Shared drives or folders
- Unencrypted cloud storage

## Additional Resources

- **Contact**: Scott (sleibrand) for access issues
- **Internal Project**: `peas-01` on Google Cloud
- **1Password**: Check `sleibrand-YOUR-NAME` vault for credentials

## Quick Reference Card

```bash
# Setup (one-time)
npm i -g @anthropic-ai/claude-code
# Download credentials from 1Password to ~/.claude/

# Add to ~/.zshrc (or ~/.bashrc)
export GOOGLE_APPLICATION_CREDENTIALS=~/.claude/YOUR-NAME-claude-code-key.json
export CLAUDE_CODE_USE_VERTEX=1
export ANTHROPIC_VERTEX_PROJECT_ID=peas-01
export CLOUD_ML_REGION=us-east5

# Reload shell
source ~/.zshrc

# Launch Claude Code
claude
```

---

**Once you have access, you've been assimilated into the Borg collective. Welcome!**

*"We are the Borg. You will be given access to AI capabilities. Resistance is futile."*
