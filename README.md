# GitOps Deployment with Helm & Argo CD

This project demonstrates a complete **GitOps** workflow using **Helm** for packaging Kubernetes applications and **Argo CD** for continuous, declarative deployment and synchronization.

Git is the single source of truth. All changes — from scaling replicas to updating configurations — are made via pull requests and automatically reconciled in the cluster.

## Architecture Overview

- **Helm Chart**: Located in `charts/myapp/` — defines the core application resources
- **Environment-specific Values**:
  - `values-dev.yaml` → Development environment (higher replicas, debug settings, etc.)
  - `values-prod.yaml` → Production environment (optimized, secure defaults)
- **Argo CD Applications**:
  - `myapp-dev` → Tracks the `dev` branch
  - `myapp-prod` → Tracks the `main` branch
- **Sync Behavior**: Argo CD watches the Git repository → detects changes → applies them automatically (self-healing included)

## Key Features Demonstrated

- Declarative GitOps workflow with Argo CD
- Environment isolation using separate branches + values files
- Helm templating for dynamic resource creation (e.g. per-environment ServiceAccounts)
- Automated synchronization & drift detection
- Self-healing: Argo CD re-applies desired state if manual changes occur in the cluster
- Infrastructure as Code (IaC) principles

## Helm Chart Resources

The chart in `charts/myapp/` dynamically generates:

| Resource          | Purpose                                      | Notes                                      |
|-------------------|----------------------------------------------|--------------------------------------------|
| **Deployment**    | Runs the main application pods               | Configurable replicas, rolling updates     |
| **Service**       | Internal cluster exposure                    | ClusterIP (can be extended to LoadBalancer)|
| **ServiceAccount**| Identity for pods                            | Dynamically named per environment          |
| (optional) others | ConfigMap, Secret, Ingress, HPA, etc.        | Easily extendable via values               |
