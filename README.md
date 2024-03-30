### Introduction

The Trivy Operator simplifies Kubernetes security with automated scanning powered by Trivy. It provides in-cluster reports for vulnerabilities, configuration audits, exposed secrets, RBAC checks, and compliance with NSA, CISA, CIS, and Kubernetes Pod Security Standards.

### Installation

#### Generating Template

Before generating the Helm release template, customize the helm release by updating the `trivy-custom-values.yaml` file in your repository. This file allows customization such as `servicemonitor`, `storageclass`, and resource utilization information.

#### Template Creation

Generate the template using the following commands:

```bash
helm repo add aqua https://aquasecurity.github.io/helm-charts/
helm template trivy-operator aqua/trivy-operator -f trivy-custom-value.yaml --output-dir="./"
```

After generating the template, rename the folder from `trivy-operator` (release name) to `helm-template` and move all templates into it.

#### Enabling Service Monitor

To enable ServiceMonitor for Prometheus scraping of Trivy-operator metrics on `/metrics` endpoint, update the `trivy-custom-values.yaml` file. Here's an example configuration:

```yaml
operator:
  namespace: "trivy-operator"
  # Other configurations...

serviceMonitor:
  enabled: true
  interval: 1m
  honorLabels: true
  # Other configurations...

# Other configurations...
```

To resolve issues with `apiVersions` during template generation, use the following command:

```bash
helm install trivy-operator aqua/trivy-operator -f trivy-custom-value.yaml --dry-run --debug > trivy-operator.yaml
```

Copy the `ServiceMonitor` section from the generated `trivy-operator.yaml` file and create a `servicemonitor.yaml` file in the `monitor` directory.

### References

- [Understand Capabilities.APIVersions.Has in Helm](https://github.com/helm/helm)
- [Trivy Operator Repository](https://github.com/aquasecurity/trivy-operator)

### Deploying via Flux CD

Update the kustomize configuration to enable deployment via Flux CD.

