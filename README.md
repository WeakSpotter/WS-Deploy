## Deployment Diagram

````mermaid
graph TD
    A[ArgoCD] -->|Deploys| B[WeakSpotter-Kub/argocd]
    B -->|Includes| C[staging/application.yaml]
    B -->|Includes| D[production/application.yaml]

    subgraph Staging Environment
        C --> E[weakspotter-staging]
        E -->|Deploys| F[WeakSpotter-Kub/frontend/staging]
        E -->|Deploys| G[WeakSpotter-Kub/backend/staging]
        F -->|Uses| H[ghcr.io/weakspotter/weakspotter-frontend:develop]
        G -->|Uses| I[ghcr.io/weakspotter/weakspotter-backend:develop]
        F -->|API_URL| J[staging.weakspotter.ozeliurs.com/api]
        F -->|Ingress| T[staging.weakspotter.ozeliurs.com]
        G -->|Ingress| K[staging.weakspotter.ozeliurs.com/api]
    end

    subgraph Production Environment
        D --> L[weakspotter-production]
        L -->|Deploys| M[WeakSpotter-Kub/frontend/production]
        L -->|Deploys| N[WeakSpotter-Kub/backend/production]
        M -->|Uses| O[ghcr.io/weakspotter/weakspotter-frontend:main]
        N -->|Uses| P[ghcr.io/weakspotter/weakspotter-backend:main]
        M -->|API_URL| Q[weakspotter.ozeliurs.com/api]
        M -->|Ingress| Z[weakspotter.ozeliurs.com]
        N -->|Ingress| R[weakspotter.ozeliurs.com/api]
    end
```

## Deployment Process

Install ArgoCD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Install ArgoCD Image Updater in the cluster.

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/argocd-image-updater/stable/manifests/install.yaml
```
