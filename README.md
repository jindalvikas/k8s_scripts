# Kubectl Helper Script - Easy Kubernetes Management

> **📁 Latest Script**: [.k8s_aliases](https://github.com/jindalvikas/k8s_scripts/blob/master/.k8s_aliases) | **📊 Version**: 1.0 | **📅 Last Updated**: December 2024

## Overview

This script provides human-friendly aliases for common kubectl operations with the Axon namespace. It features fuzzy matching, interactive selection, and simplified commands that make Kubernetes management much easier.

## Key Features

- **Fuzzy Matching**: Type partial app names (e.g., `shell` matches `axon-shell-api`)
- **Interactive Selection**: When multiple matches exist, choose from a numbered list
- **Smart Pod Selection**: Automatically uses latest pod or lets you choose
- **Error Handling**: Clear error messages with helpful suggestions
- **One-Command Deployments**: Deploy new image tags with a single command

---

## Setup

### 1. Download the Script

**📁 [Download .k8s_aliases](https://github.com/jindalvikas/k8s_scripts/blob/master/.k8s_aliases)**

Save the script as `~/.k8s_aliases` or any preferred location:

```bash
# Option 1: Clone the repository
git clone https://github.com/jindalvikas/k8s_scripts.git
cp k8s_scripts/.k8s_aliases ~/.k8s_aliases

# Option 2: Download directly (raw file)
curl -o ~/.k8s_aliases https://raw.githubusercontent.com/jindalvikas/k8s_scripts/master/.k8s_aliases

# Option 3: Wget
wget -O ~/.k8s_aliases https://raw.githubusercontent.com/jindalvikas/k8s_scripts/master/.k8s_aliases
```

### 2. Source the Script

Add to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
source ~/.k8s_aliases
```

Or source it manually each session:

```bash
source ~/.k8s_aliases
```

### 3. Verify Installation

```bash
kdoc
```

---

## Commands Reference

### Basic Operations

| Command | Description | Example |
|---------|-------------|---------|
| `kapps` | List all available apps | `kapps` |
| `kpods [app]` | List pods (all or filtered) | `kpods shell` |
| `kdeps [app]` | Show deployments | `kdeps comm` |
| `kimgs <app>` | Show container images | `kimgs mock` |
| `kstatus <app>` | Comprehensive status check | `kstatus castor` |

### Pod Management

| Command | Description | Example |
|---------|-------------|---------|
| `klogs <app>` | Tail logs with pod selection | `klogs shell` |
| `kexec <app> [shell]` | Exec into pod | `kexec mock` |
| `kssh <app>` | Alias for kexec | `kssh comm` |
| `kdelete <app>` | Delete pods (restart them) | `kdelete shell` |

### Deployment Management

| Command | Description | Example |
|---------|-------------|---------|
| `kscale <app> <n>` | Scale deployment | `kscale shell 3` |
| `krestart <app>` | Rolling restart | `krestart comm` |
| `kedit <app>` | Edit deployment config | `kedit castor` |
| `kdeploy <app> <tag>` | Deploy new image tag | `kdeploy shell v1.2.3` |

### Monitoring

| Command | Description | Example |
|---------|-------------|---------|
| `krestarts <app>` | Show restart counts | `krestarts shell` |
| `kdescribe <app>` | Describe pods | `kdescribe mock` |

### Utilities

| Command | Description | Example |
|---------|-------------|---------|
| `latestpod <app>` | Get latest pod name | `latestpod shell` |
| `selectpod <app>` | Interactively select pod | `selectpod comm` |
| `kdoc` | Show help documentation | `kdoc` |

---

## Usage Examples

### Quick Pod Operations

```bash
# List all pods for shell API
kpods shell

# Tail logs for communication service
klogs comm

# Execute into mock switch pod
kexec mock
```

### Deployment Operations

```bash
# Scale shell API to 3 replicas
kscale shell 3

# Deploy new version
kdeploy castor v1.2.3

# Restart communication service
krestart comm
```

### Fuzzy Matching Examples

| Input | Matches |
|-------|---------|
| `shell` | `axon-shell-api` |
| `comm` | `axon-communication-service` |
| `mock` | `axon-mock-switch` |
| `castor` | `axon-castor`, `axon-castor-webhooks` |
| `config` | `axon-configuration-service-api` |

---

## Interactive Features

### Multiple Matches Selection

When your search matches multiple apps:

```bash
➜ ~ kpods castor
⚠️ Multiple matches found for 'castor':
1. axon-castor
2. axon-castor-webhooks
Enter choice number (1-2): 1
✔ Using app: axon-castor
```

### Pod Selection

When multiple pods exist for an app:

```bash
➜ ~ klogs shell
Multiple pods found for 'axon-shell-api':
1. axon-shell-api-6fc7cd664f-6m67c
2. axon-shell-api-6fc7cd664f-8x9yz
Enter choice number (1-2) or press Enter for latest: 
✔ Using pod: axon-shell-api-6fc7cd664f-8x9yz
```

### Deployment Confirmation

```bash
➜ ~ kdeploy shell v1.2.3
✔ Deploying new tag for app: axon-shell-api
Current image: your-registry/axon-shell-api:v1.2.2
New image: your-registry/axon-shell-api:v1.2.3
Updating from tag 'v1.2.2' to 'v1.2.3'
Continue with deployment? (y/n): y
```

---


## Common Workflows

### Development Workflow

1. **Check current status**: `kstatus myapp`
2. **View logs**: `klogs myapp`
3. **Deploy new version**: `kdeploy myapp v1.2.3`
4. **Monitor rollout**: Status is shown automatically
5. **Verify deployment**: `kpods myapp`

### Debugging Workflow

1. **List pods**: `kpods myapp`
2. **Check restart counts**: `krestarts myapp`
3. **View detailed info**: `kdescribe myapp`
4. **Check logs**: `klogs myapp`
5. **Exec into pod**: `kexec myapp`

### Scaling Operations

1. **Check current scale**: `kdeps myapp`
2. **Scale up/down**: `kscale myapp 5`
3. **Monitor pods**: `kpods myapp`
4. **Rolling restart if needed**: `krestart myapp`


---

## Troubleshooting


### Command Not Found

```bash
# Make sure the script is sourced
source ~/.k8s_aliases

# Check if functions are loaded
type kdoc
```

### No Matches Found

```bash
# List all available apps
kapps

# Check if kubectl works
kubectl get pods -n axon
```

### Permission Issues

```bash
# Make sure you have kubectl access to the axon namespace
kubectl auth can-i get pods -n axon
```

---

## Migration from Manual Commands

### Before (Manual Commands)

```bash
kubectl get pods -n axon
kubectl exec -it --namespace axon "axon-shell-api-6fc7cd664f-6m67c" -- /bin/bash
kubectl edit deployment axon-shell-api -n axon
kubectl rollout restart deployment axon-shell-api -n axon
kubectl logs -f axon-mock-switch-5dcb49db54-sx6vr -n axon
kubectl scale deployment axon-calliope-api --replicas=4 -n axon
```

### After (Helper Commands)

```bash
kpods
kexec shell
kedit shell
krestart shell
klogs mock
kscale calliope 4
```

---

## Contributing

If you have suggestions for improvements or additional features:

1. Fork the repository: https://github.com/jindalvikas/k8s_scripts
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Test your changes thoroughly
4. Update the documentation
5. Commit your changes: `git commit -m 'Add amazing feature'`
6. Push to the branch: `git push origin feature/amazing-feature`
7. Open a Pull Request

### Development Guidelines

- Test all functions with different app names
- Ensure fuzzy matching works correctly
- Update help documentation for new features
- Add demo GIFs for new functionality
- Follow existing code style and patterns

---

## Files and Resources

### Repository Structure
```
k8s_scripts/
├── .k8s_aliases                 # Main script file
├── README.md                    # Documentation (this file)
├── demos/                       # Demo GIFs and recordings (optional)
│   ├── basic_operations.gif
│   ├── deployment_workflow.gif
│   └── fuzzy_matching.gif
└── docs/                        # Additional documentation (optional)
    ├── installation.md
    └── troubleshooting.md
```

### Script Files
- **📁 [.k8s_aliases](https://github.com/jindalvikas/k8s_scripts/blob/master/.k8s_aliases)** - Main script file
- **📝 [Repository](https://github.com/jindalvikas/k8s_scripts)** - Full repository with all files

### Related Documentation
- **🔗 [Main Repository](https://github.com/jindalvikas/k8s_scripts)** - Full k8s scripts collection
- **🔗 [kubectl Official Documentation](https://kubernetes.io/docs/reference/kubectl/)**

---

## Quick Reference Card

**📁 Script File**: [.k8s_aliases](https://github.com/jindalvikas/k8s_scripts/blob/master/.k8s_aliases)

| Task | Old Command | New Command |
|------|-------------|-------------|
| List pods | `kubectl get pods -n axon` | `kpods` |
| Pod logs | `kubectl logs -f pod-name -n axon` | `klogs app` |
| Exec into pod | `kubectl exec -it pod-name -n axon -- /bin/bash` | `kexec app` |
| Edit deployment | `kubectl edit deployment app -n axon` | `kedit app` |
| Scale deployment | `kubectl scale deployment app --replicas=3 -n axon` | `kscale app 3` |
| Restart deployment | `kubectl rollout restart deployment app -n axon` | `krestart app` |
| Deploy new tag | *Manual edit + save* | `kdeploy app v1.2.3` |

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
