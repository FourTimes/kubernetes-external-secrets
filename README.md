

```bash
#Install the external secrets
helm repo add external-secrets https://external-secrets.github.io/kubernetes-external-secrets/
helm repo update
helm install external-secrets external-secrets/kubernetes-external-secrets

```

```bash
# Install the stakater/reloader
helm repo add stakater https://stakater.github.io/stakater-charts
helm repo update
helm install stakater-reloader stakater/reloader

```
