# PakalSpot GitOps

This repository contains the GitOps configuration for PakalSpot application deployment using ArgoCD.

## Structure

```
pakalspot-gitops/
├── applicationset.yaml          # ArgoCD ApplicationSet for multi-environment deployment
├── shared/
│   └── base-values.yaml         # Common configuration for all environments
└── environments/
    ├── dev/
    │   └── values.yaml          # Dev environment overrides only
    ├── staging/
    │   └── values.yaml          # Staging environment overrides only
    └── prod/
        └── values.yaml          # Production environment overrides only
```

## ApplicationSet

The `applicationset.yaml` file defines an ArgoCD ApplicationSet that automatically creates and manages applications for all environments (dev, staging, prod). This replaces the need for individual ArgoCD application files.

### Features

- **Multi-environment support**: Automatically deploys to dev, staging, and production
- **Environment-specific configurations**: Each environment uses its own values file
- **Automated sync**: Applications are automatically synced with Git changes
- **Namespace management**: Automatically creates namespaces for each environment

### Deployment

1. **Apply the ApplicationSet**:
   ```bash
   kubectl apply -f applicationset.yaml
   ```

2. **Verify deployment**:
   ```bash
   kubectl get applicationsets -n argocd
   kubectl get applications -n argocd
   ```

3. **Check application status**:
   ```bash
   argocd app list
   argocd app get pakalspot-dev
   argocd app get pakalspot-staging
   argocd app get pakalspot-prod
   ```

### Environment Configuration

Each environment is configured with:

- **Dev**: `pakalspot-dev` namespace, development values
- **Staging**: `pakalspot-staging` namespace, staging values  
- **Prod**: `pakalspot-prod` namespace, production values

### Values Files

Configuration follows DRY principles with shared base configuration:
- `shared/base-values.yaml` - Common configuration for all environments
- `environments/dev/values.yaml` - Development overrides only
- `environments/staging/values.yaml` - Staging overrides only
- `environments/prod/values.yaml` - Production overrides only

### DRY Implementation

This repository implements **Don't Repeat Yourself (DRY)** principles:
- **Single source of truth** for all configurations
- **Shared base configuration** eliminates duplication
- **Environment-specific overrides** only contain differences
- **Centralized management** through ApplicationSet

See `DRY-IMPLEMENTATION.md` for detailed documentation.

### Migration from Individual Applications

If you're migrating from individual ArgoCD applications:

1. Apply the ApplicationSet
2. Verify all applications are created correctly
3. Remove the individual `argocd-app.yaml` files from each environment directory
4. The ApplicationSet will manage all applications going forward

### Troubleshooting

- Check ApplicationSet status: `kubectl describe applicationset pakalspot-application-set -n argocd`
- Check individual applications: `kubectl get applications -n argocd`
- View ArgoCD UI: `kubectl port-forward svc/argocd-server -n argocd 8080:443`
