# Kubernetes Operations

Multi-cluster Kubernetes operations, best practices, and automation patterns using Claude Code and the k8s MCP server.

## Core Principle: Use MCP, Not Bash

**CRITICAL**: For multi-cluster operations, ALWAYS use the `clusters_exec_all` MCP tool.

### ✅ Correct Approach
```
Use clusters_exec_all to list all nodes across all clusters
```

### ❌ Wrong Approach
```bash
# NEVER DO THIS
for cluster in $(kubectl config get-contexts -o name); do
  kubectl config use-context $cluster
  kubectl get nodes
done
```

## Multi-Cluster Best Practices

### 1. Parallel Execution
Use `clusters_exec_all` to execute operations across all clusters simultaneously:

```
Use clusters_exec_all with continue_on_error=true to check pod status
across all clusters
```

### 2. Error Handling
Always use `continue_on_error: true` to ensure all clusters are checked even if one fails.

### 3. Result Aggregation
Collect results from all clusters and present them in a unified view.

## Common Operations

### Check Node Status
```
Use clusters_exec_all to list all nodes and their status across all clusters
```

### Find Resource Across Clusters
```
Use clusters_exec_all to find all pods with label app=nginx across all clusters
```

### Verify Deployment
```
Use clusters_exec_all to check if deployment my-app is running in all clusters
```

### Get Events
```
Use clusters_exec_all to get recent events in namespace kube-system across all clusters
```

## Cluster Context Management

### List Available Clusters
```
List all available Kubernetes clusters from kubeconfig
```

### Switch to Specific Cluster
For one-off operations on a single cluster:
```
Use clusters_switch to switch to c1-sjc3, then get nodes
```

### Default Context
- **Primary Context**: eks-k0rdent-useast1:kcm-system
- **Primary Namespace**: kcm-system

## Testing with K8s MCP Server

**IMPORTANT**: Always use the k8s MCP server for testing, not direct kubeconfig access.

### Why?
- Consistent interface across operations
- Better error handling
- Supports multi-cluster operations natively
- Integrates with Claude Code workflows

## Resource Operations

### Get Resources
```
Use clusters_exec_all to get all VolumeAttachments with operation=resources_list,
arguments={"apiVersion": "storage.k8s.io/v1", "kind": "VolumeAttachment"}
```

### Create Resources
```
Use the k8s MCP to create a ConfigMap in namespace default from this YAML
```

### Update Resources
```
Use the k8s MCP to patch deployment my-app with the new image version
```

### Delete Resources
```
Use the k8s MCP to delete pod my-pod-12345 in namespace default
```

## Troubleshooting

### Authentication Issues
```
Verify my kubeconfig is valid and I can access the cluster
```

### Resource Not Found
```
Search all namespaces across all clusters for pod matching name pattern "nginx-*"
```

### RBAC Issues
```
Check what permissions my current service account has in namespace kube-system
```

## Integration with Change Management

When making infrastructure changes:

1. **Verify current state** using k8s MCP
2. **Create CM ticket** with details
3. **Execute change** following MOP
4. **Validate with k8s MCP**
5. **Update CM ticket** with results

Example:
```
1. Use clusters_exec_all to verify current SR-IOV VF configuration
2. Create CM ticket for updating VF configuration
3. Follow MOP to update configuration
4. Use clusters_exec_all to verify new configuration
5. Update CM ticket with success status
```

## Best Practices

1. **Always use MCP for multi-cluster**: Don't write bash loops
2. **Specify namespaces**: Even if using default
3. **Use label selectors**: More efficient than listing all resources
4. **Check before changing**: Verify current state first
5. **Validate after changing**: Confirm operation succeeded
6. **Handle errors gracefully**: Use continue_on_error for cluster sweeps

## Resources

- **Kubernetes Documentation**: https://kubernetes.io/docs/
- **Multi-Cluster Best Practices**: [multi-cluster-best-practices.md](multi-cluster-best-practices.md)
- **K8s MCP Setup**: [../mcp-servers/kubernetes-mcp-setup.md](../mcp-servers/kubernetes-mcp-setup.md)

---

**The Borg navigate Kubernetes clusters with perfect coordination across the collective.**
