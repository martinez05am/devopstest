# Argo CD Applications

This directory contains Argo CD Application manifests organized by environment.

## Directory Structure

```
argocd-apps/
├── base/              # Shared/base configurations for applications
├── dev/               # Development environment applications
├── test/              # Test environment applications
└── README.md
```

## Usage

### Deploying to Dev

```bash
kubectl apply -f argocd-apps/dev/
```

### Deploying to Test

```bash
kubectl apply -f argocd-apps/test/
```

## Adding New Applications

1. Create the Application manifest in the appropriate environment folder (e.g., `dev/my-app.yaml`)
2. Apply it using `kubectl apply -f argocd-apps/dev/my-app.yaml`
3. Or use Argo CD API/CLI to create it

## Example Application Structure

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/example/repo.git
    targetRevision: HEAD
    path: manifests/my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: <environment>  # dev, test, etc.
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

## Namespace Isolation

Each environment should use its own namespace:
- `dev` namespace for dev applications
- `test` namespace for test applications

Create namespaces before deploying applications:
```bash
kubectl create namespace dev
kubectl create namespace test
```
