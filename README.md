# Punchplatform Helm Chart

This repository regroups Punchplatform Helm Chart for deploying services:
- PunchOperator
- ArtifactServer

## Quickstart

```sh
helm repo add punchplatform https://punchplatform.github.io/punch-helm
helm repo update
# this will list available helm charts
helm search repo punchplatform --devel

# Deploy operator-crds
helm install punch/operator-crds \
    --version 1.0-SNAPSHOT \
    --devel \
    --generate-name

# Deploy operator
helm install punch/operator \
    --version 1.0-SNAPSHOT \
    --devel \
    --generate-name \
    --namespace punchoperator-system \
    --create-namespace
```

## Documentation

- [PunchOperator Helm Documentation](./operator/README.md)
