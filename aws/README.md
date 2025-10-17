# AWS Configuration and Operations

AWS SSO setup, EKS cluster access, and cloud infrastructure automation patterns.

## Quick Reference

### AWS SSO Login
```bash
aws sso login --sso-session Admin
```

### Default Settings
- **Primary SSO Session**: Admin (d-9a672a4d96.awsapps.com)
- **Primary Region**: us-east-2
- **Primary Profile**: AdministratorAccess-257394477137

### EKS Cluster Access
```bash
# Update kubeconfig for EKS cluster
aws eks update-kubeconfig --region us-east-1 --name k0rdent-useast1

# Verify cluster access
kubectl config current-context
# Should show: eks-k0rdent-useast1:kcm-system
```

## SSO Configuration

### Available SSO Sessions

**Admin Session (Primary)**:
```ini
[sso-session Admin]
sso_start_url = https://d-9a672a4d96.awsapps.com/start/#
sso_region = us-east-2
sso_registration_scopes = sso:account:access
```

**Netskope Session (Alternative)**:
```ini
[sso-session Netskope]
sso_start_url = https://d-92679ae04b.awsapps.com
sso_region = us-east-1
sso_registration_scopes = sso:account:access
```

### Quick Login Commands
```bash
# Primary SSO session (most commonly used)
aws sso login --sso-session Admin

# Alternative Netskope session
aws sso login --sso-session Netskope
```

## Common Commands

### Check Current Identity
```bash
aws sts get-caller-identity
```

### List Available Profiles
```bash
aws configure list-profiles
```

### Check SSO Login Status
```bash
aws sso list-cached-login-tokens
```

### EKS Cluster Info
```bash
aws eks describe-cluster --name k0rdent-useast1 --region us-east-1
```

## Troubleshooting

### SSO Authentication Issues

**Problem**: `InvalidRequestException` during SSO setup

**Solution**:
1. Use existing SSO sessions: `aws sso login --sso-session Admin`
2. Clear cache if needed: `rm -rf ~/.aws/sso/cache/ ~/.aws/cli/cache/`
3. Logout first: `aws sso logout`
4. Edit `~/.aws/config` manually instead of using `aws configure sso`

### Token Expiration

**Problem**: Commands fail with authentication errors

**Solution**:
```bash
aws sso login --sso-session Admin
```

### EKS Access Issues

**Problem**: Cannot access EKS cluster

**Solutions**:
1. Ensure SSO session is active
2. Verify profile has EKS permissions
3. Update kubeconfig: `aws eks update-kubeconfig --region us-east-1 --name k0rdent-useast1`
4. Check IAM role mappings in aws-auth ConfigMap

## Integration with Development Workflow

### Using Claude Code with AWS
```
Check my current AWS SSO session, then list all EKS clusters in us-east-1
```

```
Update my kubeconfig for the k0rdent-useast1 EKS cluster and verify access
```

### Branch Naming with AWS Resources
```
feature/eks-k0rdent-useast1-upgrade
fix/iam-role-mapping-issue
```

### Commit Messages with AWS Context
```
Update EKS cluster k0rdent-useast1 to version 1.29

- Update cluster version in us-east-1
- Verify node group compatibility
- Update aws-auth ConfigMap for new version

AWS Account: 257394477137
Region: us-east-1
```

## Best Practices

1. **Always use SSO**: Never use long-term credentials
2. **Specify region**: Always include `--region` flag for consistency
3. **Verify identity**: Run `aws sts get-caller-identity` before destructive operations
4. **Use profiles**: Specify `--profile` for non-default profiles
5. **Check VPN**: Some resources require VPN connection

## Resources

- **AWS CLI Documentation**: https://docs.aws.amazon.com/cli/
- **EKS Documentation**: https://docs.aws.amazon.com/eks/
- **SSO Configuration Guide**: [sso-setup.md](sso-setup.md)

---

**The Borg assimilate AWS infrastructure with precision and efficiency.**
