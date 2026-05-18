---
name: deploy
description: Use this skill when deploying containers to Kubernetes, building images via BuildKit, or managing k8s workloads.
---

# Kubernetes Deployment

Build, push, and deploy container images to the k3s cluster via BuildKit and kubectl.

## Available Tools

### build-workload

Build a container image via BuildKit and push to the in-cluster registry.

```bash
build-workload [context_dir] [image_name] [image_tag] [dockerfile_path]
```

**Arguments:**
- `context_dir` (optional): Build context directory. Defaults to `$BUILD_CONTEXT` or current directory
- `image_name` (optional): Name for the image. Defaults to basename of context_dir
- `image_tag` (optional): Tag for the image. Defaults to `latest`
- `dockerfile_path` (optional): Path to Dockerfile. Defaults to `Dockerfile` in context

**Example:**
```bash
# Build and push from current directory
build-workload . myapp latest

# Build from specific directory with custom tag
build-workload /workspace/myapp myapp v1.0.0
```

---

### kubectl-workloads

Wrapper for kubectl targeting the correct namespace `WORKLOADS_NAMESPACE`.

```bash
kubectl-workloads <kubectl args>
```

**Examples:**
```bash
# Deploy new manifests
kubectl-workloads apply -f /path/to/k8s/deploy/config/

# Update deployment image
kubectl-workloads set image deployment/myapp myapp=localhost:30500/myapp:v1.0.0

# Verify rollout
kubectl-workloads rollout status deployment/myapp

# Check pods
kubectl-workloads get pods

# Describe a pod for debugging
kubectl-workloads describe pod myapp-pod-abc123

# View logs
kubectl-workloads logs -f deployment/myapp

# Restart (zero-downtime)
kubectl-workloads rollout restart deployment/myapp

# Scale deployment
kubectl-workloads scale deployment/myapp --replicas=1
```

---

## Workflow

1. **Build**: `build-workload [context_dir] [image_name] [image_tag]`
2. **Deploy**: `kubectl-workloads apply -f [manifest_path]` or `kubectl-workloads set image [resource] [container]=[image]`
3. **Verify**: `kubectl-workloads rollout status [resource]`
4. **Debug**: `kubectl-workloads get pods`, `kubectl-workloads describe pod [pod]`

---

## Troubleshooting

### BuildKit not reachable
```
buildctl --addr tcp://buildkit.buildkit.svc.cluster:1234 debug workers
```

### Pod not starting
```bash
kubectl-workloads describe pod <pod-name>
kubectl-workloads logs <pod-name>
```

### Image pull failure
Ensure `$REGISTRY_PULL` matches what was pushed via `$REGISTRY_PUSH`.
The in-cluster registry uses `localhost:30500` (NodePort), but pods pull via the cluster DNS name.

### RBAC issues
The deployment's ServiceAccount must have RBAC permissions in the target namespace.
This is configured via Role/RoleBinding in the k8s manifests.