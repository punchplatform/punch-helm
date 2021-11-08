# Punchplatform Helm Chart

This repository regroups Punchplatform Helm Chart for deploying services:
- [PunchOperator](./operator/README.md)
- [PunchOperator CRDs](./operator-crds/README.md)
- [Injector](./injector/README.md)
- [Extraction Server](./extraction-server/README.md)
- [Elastic Housekeeping](./elastic-housekeeping/README.md)
- [Elastalert](./elastalert/README.md)
- [Artifacts Server](./artifacts/README.md)
- [Archive Housekeeping](./archive-housekeeping/README.md)

## Quickstart

```sh
helm repo add punchplatform https://punchplatform.github.io/punch-helm
helm repo update
# this will list available helm charts
helm search repo punchplatform --devel

# Deploy a helm chart
helm install punchplatform/<punch helm chart> \
    --version <version> \
    --devel \
    --generate-name \
    --namespace <namespace>
```