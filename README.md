# ArgoCD Test Applications

This repository contains Kubernetes manifests for testing ArgoCD deployments across multiple clusters and environments.

**For detailed architecture, testing scenarios, and development information, see [claude.md](./claude.md).**

## Structure

```
argocd-apps/
├── frontend/           # Frontend application
│   ├── base/          # Base manifests
│   └── overlays/      # Environment-specific overlays
│       ├── dev/       # Development environment
│       └── prod/      # Production environment
├── backend/           # Backend application
│   ├── base/
│   └── overlays/
│       ├── dev/
│       └── prod/
├── api-service/       # API service (dev only)
│   ├── base/
│   └── overlays/
│       └── dev/
└── demo-app/          # Demo application for ArgoCD #2
```

## Applications

### Frontend
- **Dev**: 1 replica, deployed to kind-dev cluster
- **Prod**: 3 replicas, deployed to kind-prod cluster
- Manual sync policy

### Backend
- **Dev**: 1 replica, auto-sync enabled with prune
- **Prod**: 3 replicas, manual sync with conservative policies
- Environment-specific configuration

### API Service
- Development only
- Auto-sync enabled
- Deployed to kind-dev cluster

### Demo App
- Simple application for ArgoCD #2 testing
- Deployed to kind-argocd-2 cluster (local)

## Testing Out-of-Sync Scenarios

1. Make changes to any manifest file
2. Commit and push to GitHub
3. Wait 1-3 minutes for ArgoCD to detect changes
4. Manual sync apps will show "OutOfSync" status
5. Auto-sync apps will automatically sync

## Usage

ArgoCD applications reference specific paths in this repository:

```yaml
spec:
  source:
    repoURL: https://github.com/{user}/argocd-apps
    targetRevision: main
    path: frontend/overlays/dev  # Path to specific environment
```
